# Exercises: LLM Internals

Four build projects, one per phase, each non-trivial. Plus design drills, code-reading targets, and teach-back prompts.

---

## Build projects (one per phase)

### Phase 1 build — `nanoGPT-rewrite`
**Goal**: A single-file PyTorch decoder-only transformer that overfits a small corpus, written without copying.
**Scope (minimum)**:
- Token embedding + learned (or RoPE) positional encoding.
- N layers of: pre-LayerNorm → MHA (from scratch, no `nn.MultiheadAttention`) → residual → pre-LayerNorm → MLP (GeLU or SwiGLU) → residual.
- Final LayerNorm + LM head (weight-tied to embedding).
- Causal mask correctness verified with a unit test (a token's logits must not change when later tokens are perturbed).
- Trains via plain Adam on `input_text.txt` (any 1MB English text). Loss < 0.5 within 500 steps.
**Stretch**:
- Swap absolute positional embedding for RoPE; observe the parameter count drop.
- Implement GQA with `n_kv_heads < n_heads`.
- Profile with `torch.profiler` — find the slowest op and explain why.

### Phase 2 build — `tinystories-trainer`
**Goal**: Take the Phase 1 model and train it properly on TinyStories (or similar small narrative corpus).
**Scope**:
- Proper data pipeline: tokenize with `tiktoken` (cl100k or gpt2), pack sequences to fixed length, shuffle, save as a memmap binary.
- AdamW with cosine LR schedule + warmup.
- bf16 autocast on GPU/MPS, gradient clipping at 1.0.
- Eval loop: held-out loss + a sample-text-generation hook every N steps.
- Save/resume checkpoints.
**Stretch**:
- Gradient checkpointing toggle; measure peak memory delta.
- Run a Chinchilla-style 3-point scan: same compute budget split into (small model, more tokens) vs (large model, fewer tokens). Plot loss.
- Diagnose and reproduce a loss spike; explain three mitigations you'd try.

### Phase 3 build — `kv-and-speculative`
**Goal**: Make the Phase 2 model serve fast. Add KV cache, samplers, speculative decoding.
**Scope**:
- Refactor attention to accept and update a KV cache (typed as a list of `(K, V)` tensors per layer).
- Implement temperature, top-k, top-p, and min-p samplers as a single composable pipeline.
- Train (or distill) a tiny "draft" model (e.g. 4 layers, d=128) on the same data. Implement speculative decoding:
  - Draft generates k tokens autoregressively.
  - Target runs one parallel forward pass to score them.
  - Accept/reject each via the rejection-sampling rule from the Leviathan paper.
- Benchmark: tokens/sec for naive, KV-cache, and KV-cache + speculative. Report acceptance rate.
**Stretch**:
- Implement INT8 quantization on the LM head (the largest matmul) and measure latency + perplexity delta.
- Implement PagedAttention-style KV cache (block-allocated) for batched prompts; serve batch=8 with different prompt lengths.
- Compile one layer's attention with `torch.compile` or write a Triton kernel; compare against FlashAttention.

### Phase 4 build — `align-it`
**Goal**: Take a base model (yours from Phase 2, or `EleutherAI/pythia-160m`) through SFT → LoRA → DPO.
**Scope**:
- SFT loop on a small instruction dataset (e.g. Alpaca, Dolly-15k, or a synthetic one you generate with an existing model). Use the chat template properly — masking labels on the prompt tokens.
- Implement LoRA from scratch: a `LoRALinear` wrapper that adds `B @ A @ x * scaling` to the base linear's output. Inject into Q and V projections. Verify only LoRA params have `requires_grad=True`.
- Implement DPO from the paper: `loss = -log σ(β · (log π_θ(y_w|x) - log π_θ(y_l|x) - log π_ref(y_w|x) + log π_ref(y_l|x)))`. Train on a small preference dataset (e.g. `Anthropic/hh-rlhf` subset, or UltraFeedback).
- Compare base vs SFT vs SFT+DPO on 20 held-out prompts via side-by-side generation.
**Stretch**:
- Hot-swap LoRA adapters at inference: load two adapters, switch between them per prompt.
- Implement KTO or IPO as alternatives to DPO; compare on the same data.
- Add a reward model and a tiny PPO loop for a single epoch. Observe how much harder it is than DPO.

---

## Design drills

- **Attention cost**: A 70B parameter model with d=8192, n_heads=64, head_dim=128, n_layers=80, GQA with 8 kv_heads, serving batch=16 at seq_len=8192 in fp16. Compute KV cache size in GB. Is it H100-feasible (80GB)?
- **Sampler tradeoffs**: A user reports "the model loops." Walk through which sampler parameter you'd suspect first, second, third — and the evidence that distinguishes them.
- **Pretrain vs fine-tune budget**: Given $100k of compute, do you train a 1B model on 100B tokens or fine-tune Llama-3-8B with your data? Defend the answer for the use case "domain-specific code completion for a niche language."
- **MoE vs dense**: Same inference budget, same training budget — when does MoE win and when does dense win? Be specific about workload.
- **Quantization choice**: You have a 13B model that runs at fp16 on an A100 (80GB). You need to serve it on an A10 (24GB). Walk through INT8 → INT4 → INT2 options and what you'd lose at each step.
- **Long context**: You want to extend a model trained at 4k context to 32k. List the options (RoPE NTK, YaRN, fine-tune, retrieval) and what each costs in compute and quality.

---

## Read-and-explain targets

- `karpathy/nanoGPT/model.py` — entire file. Explain *why* `Block` does pre-norm and *why* `register_buffer` is used for the causal mask.
- `huggingface/transformers/src/transformers/models/llama/modeling_llama.py` — `LlamaAttention.forward`. Explain every reshape and what `apply_rotary_pos_emb` does.
- `ggerganov/llama.cpp/ggml-quants.c` — pick `quantize_row_q4_K_reference` (or current equivalent). Explain the block size, the super-block, and where the scales live.
- `vllm-project/vllm/vllm/core/scheduler.py` — the batching scheduler. Explain how it decides which requests to prefill vs decode each step.
- `Dao-AILab/flash-attention` — the Python wrapper + the algorithm box in the paper. Explain the online softmax trick in your own words.

---

## Teach-back targets

End of each phase, write a 1-page explainer for a hypothetical mid-level SWE who's never read a transformer paper. Surfaces holes ruthlessly.

- **End of Phase 1**: "What is a transformer, mechanically? Take an input string to logits in 1 page."
- **End of Phase 2**: "Why is training an LLM hard? What goes wrong, and what optimizer/precision tricks fix it?"
- **End of Phase 3**: "Why is generating text slower than reading it? Walk through KV cache, then speculative decoding, in 1 page."
- **End of Phase 4**: "What is RLHF, and why is DPO replacing it in many places? In 1 page, no math beyond log-prob ratios."

If a teach-back takes >2 hours or you can't write it without re-opening sources, you don't yet understand the phase — repeat the build at reduced scope.
