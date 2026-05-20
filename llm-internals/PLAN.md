# Plan: LLM Internals

A 4-week plan to move from "API caller" to "can read any modern LLM paper and reproduce a small piece of it from scratch." Each phase ends in a concrete artifact in code.

---

## Phase 1: Transformer mechanics (Week 1)

**Goal**: Understand the forward pass of a decoder-only transformer at the level of every tensor shape, by reproducing it.

**Deliverable**: A ~300-line single-file PyTorch implementation of a GPT-style decoder that overfits a tiny dataset (Shakespeare or TinyStories sample). No `nn.MultiheadAttention` — write attention from scratch.

- **Build**: Fork or re-derive `nanoGPT`'s `model.py`. Add inline comments for every tensor's shape transition.
- **Read**:
  - "Attention Is All You Need" — Vaswani et al. (2017). Skim §3 in detail.
  - The Annotated Transformer (Harvard NLP) — match the math to code line by line.
  - Karpathy's "Let's build GPT" video / `nanoGPT` repo.
- **Drill**: On paper, derive the shapes through a 12-layer, 8-head, d_model=512, seq_len=1024 model. Count parameters. Compare to your code's `sum(p.numel() for p in model.parameters())`.
- **Exit criteria**:
  - Model overfits a 1MB text file to <0.5 loss in <500 steps on CPU/MPS/CUDA.
  - You can explain (without notes) why attention is O(n²) in sequence length and where exactly that cost lives in the code.
  - You can answer: why pre-norm vs post-norm? Why RoPE vs absolute positional embeddings? Why GQA?

---

## Phase 2: Training dynamics & optimization (Week 2)

**Goal**: Understand what actually happens when a model is trained — loss landscape, optimizer state, mixed precision, scaling.

**Deliverable**: Your Phase 1 model trained on TinyStories (or similar) with: AdamW, cosine LR schedule, gradient clipping, bf16 mixed precision, gradient checkpointing toggle. A loss curve + a 1-page write-up of the training run.

- **Build**: Add a training loop with WandB or simple CSV logging. Toggle one feature at a time (no schedule → cosine → warmup; fp32 → bf16; no clip → clip) and observe the curve.
- **Read**:
  - "Adam: A Method for Stochastic Optimization" — Kingma & Ba (2014). Then "Decoupled Weight Decay Regularization" (AdamW) — Loshchilov & Hutter (2017).
  - "Scaling Laws for Neural Language Models" — Kaplan et al. (2020).
  - "Training Compute-Optimal Large Language Models" (Chinchilla) — Hoffmann et al. (2022).
  - Mixed precision primer: NVIDIA's bf16/fp16 training docs.
- **Drill**: Given a fixed FLOP budget of 1e20, which (N_params, D_tokens) does Chinchilla recommend? Derive it. Then explain why "Chinchilla-optimal" was about *training* compute, not *inference* cost — and why production models (Llama, Mistral) deliberately over-train smaller models.
- **Exit criteria**:
  - You can explain Adam's moment estimates and why decoupling weight decay matters.
  - You can produce a defensible answer to "should I train a 1B model on 20B tokens or a 250M model on 100B tokens?" given a constraint.
  - You know what a "loss spike" looks like, why it happens, and three mitigations.

---

## Phase 3: Inference internals (Week 3)

**Goal**: Understand where every microsecond goes during generation. KV cache, sampling, quantization, batching, speculative decoding.

**Deliverable**: Extend your Phase 1 model with: (a) a proper KV cache (not just naive recomputation), (b) top-k / top-p / temperature sampling, (c) a working speculative-decoding loop using a smaller "draft" model. Benchmark tokens/sec for each.

- **Build**:
  - Step 1: Add the KV cache. Measure prefill vs decode latency separately.
  - Step 2: Add a sampler with temperature, top-k, top-p, and min-p. Verify with a "needle-in-haystack" prompt that greedy ≠ sampled.
  - Step 3: Speculative decoding — train (or distill) a tiny draft model, run target verification, measure accept rate.
- **Read**:
  - "Efficiently Scaling Transformer Inference" — Pope et al. (2022). Background on prefill/decode asymmetry.
  - "Fast Inference from Transformers via Speculative Decoding" — Leviathan, Kalman, Matias (2023).
  - "FlashAttention" — Dao et al. (2022) and FlashAttention-2.
  - "GPTQ" or "AWQ" papers — pick one and understand the rounding math.
  - Skim `llama.cpp`'s `llama.cpp` and `ggml.c` for at least one quantized matmul kernel (e.g. Q4_K_M).
- **Drill**: Draw the memory layout of a KV cache for batch=4, seq=2048, layers=32, heads=32, head_dim=128, dtype=fp16. How many bytes? Now redo for GQA with 8 kv_heads. Then explain why MoE complicates this.
- **Exit criteria**:
  - You can name 3 reasons inference is memory-bandwidth bound, not compute bound.
  - You can explain speculative decoding's correctness guarantee (rejection sampling), not just its speedup.
  - You can read a quantized GEMM kernel and explain what the block structure is doing.

---

## Phase 4: Post-training & alignment (Week 4)

**Goal**: Understand the difference between a base model and a chat model, and how RLHF / DPO actually update weights.

**Deliverable**: Fine-tune your Phase 1 model (or a HuggingFace small model like Pythia-160m) with: (a) supervised fine-tuning on an instruction dataset, (b) LoRA adapters, (c) one round of DPO on a preference dataset. Compare base vs SFT vs DPO outputs on a held-out prompt set.

- **Build**:
  - SFT loop: standard cross-entropy on (prompt, response) pairs from a small dataset (Alpaca, Dolly, or a synthetic one you generate).
  - LoRA: implement rank-r adapters from scratch (don't just `peft.get_peft_model`); inject into Q,V projections.
  - DPO: implement the DPO loss from the paper (no `trl.DPOTrainer`); train against a preference dataset.
- **Read**:
  - "Training language models to follow instructions with human feedback" (InstructGPT) — Ouyang et al. (2022).
  - "Direct Preference Optimization" — Rafailov et al. (2023).
  - "LoRA: Low-Rank Adaptation of Large Language Models" — Hu et al. (2021).
  - "Constitutional AI" — Bai et al. (2022). And recent: "RLAIF" papers.
  - Llama 3 paper (Meta, 2024) — read §4 post-training in full.
- **Drill**: Derive the DPO loss from the RLHF objective. Show why the reference model term cancels in a way that lets you skip the separate reward model.
- **Exit criteria**:
  - You can sketch the gradient flow through a LoRA-adapted attention layer.
  - You can explain why DPO replaced RLHF in many open pipelines, and where RLHF still wins.
  - You can articulate the difference between a base model, an instruct/chat model, and a reasoning model (o1-style) at the level of training objective, not just behavior.

---

## Open questions (revisit after the plan)

- Mixture of Experts: routing, load balancing, capacity factor. (DeepSeek-MoE, Mixtral.)
- Long-context: ring attention, sliding window, retrieval-augmented context. RoPE scaling tricks (NTK, YaRN).
- Reasoning models: process reward models, RL on verifiable rewards, chain-of-thought as a trained behavior vs prompted one.
- Multimodal: vision encoders, cross-attention vs early fusion, native multimodal tokenizers.
- Diffusion LMs and continuous-time language models.
- Mechanistic interpretability: superposition, sparse autoencoders, circuit analysis.
- Training infrastructure: ZeRO, FSDP, pipeline parallelism, expert parallelism — what stays in memory where.
