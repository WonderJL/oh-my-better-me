# Curriculum: LLM Internals

## Tier 1 — Prerequisites (skim-only for seniors)

- ✅ Linear algebra: matrix multiplication, matrix-vector products, broadcasting. (Don't re-study.)
- ✅ Python, NumPy, PyTorch basics.
- 📖 Calculus chain rule + Jacobians (for grasping autograd). Refresh if rusty.
- 📖 Softmax, cross-entropy, log-likelihood. One refresher reading.
- 📖 RNN / LSTM at a high level — useful only as a contrast point ("here's what attention replaced and why").
- 🔬 Probability: KL divergence, MLE, importance sampling. (DPO and RLHF lean on this.)
- 🔬 Information theory minimum: entropy, perplexity, bits per token.

---

## Tier 2 — Core load-bearing ideas

### Tokenization (BPE / SentencePiece / tiktoken)
- **Definition**: a deterministic mapping from byte sequences to a fixed-size vocabulary of integer IDs, learned greedily by merging frequent pairs.
- **Leverage**: vocabulary size and tokenizer choice silently dominate context-length math, multilingual quality, and a class of "stupid LLM" bugs (e.g. counting letters).
- **Source**: Sennrich et al. "Neural Machine Translation of Rare Words with Subword Units" (2015); `tiktoken` source.
- **Maps onto**: Huffman coding + a learned dictionary. A compiler's lexer, but trained.

### Self-attention
- **Definition**: `softmax(Q Kᵀ / √d) V` — for each position, a learned weighted average of all positions' values, weighted by query-key similarity.
- **Leverage**: the entire field hinges on this primitive. Every later trick is making it cheaper, longer-range, or more structured.
- **Source**: Vaswani et al. (2017), §3.2.
- **Maps onto**: a soft, learned hash table lookup. Or: a content-addressable memory with learned addressing.

### Multi-head attention (MHA / MQA / GQA)
- **Definition**: run h attention operations in parallel with different learned projections, concatenate, project back. MQA shares K,V across heads; GQA shares them across groups.
- **Leverage**: GQA is why modern models can have long context affordably — it's a KV-cache memory optimization disguised as a model architecture choice.
- **Source**: Vaswani (MHA); Shazeer "Fast Transformer Decoding" (MQA, 2019); Ainslie et al. "GQA" (2023).
- **Maps onto**: ensembles with shared input but different projections — like SIMD lanes for attention.

### Positional encoding (absolute / RoPE / ALiBi)
- **Definition**: attention is permutation-invariant; positional encodings inject "where am I in the sequence." RoPE rotates Q,K vectors in 2D planes by a position-dependent angle.
- **Leverage**: RoPE's design is *why* you can length-extrapolate via NTK/YaRN scaling. The encoding choice constrains the future of the model.
- **Source**: Su et al. "RoFormer: Enhanced Transformer with Rotary Position Embedding" (2021).
- **Maps onto**: complex-number multiplication; phase encoding in DSP.

### Feed-forward / MLP block (and SwiGLU)
- **Definition**: two linear layers with a nonlinearity, applied per position. SwiGLU uses gated linear units (Swish gate × linear).
- **Leverage**: this is where most of the *parameters* live (typically 4× hidden expansion). It's the "memory" of the model in the Anthropic/Geva sense.
- **Source**: Shazeer "GLU Variants Improve Transformer" (2020).
- **Maps onto**: a per-token key-value memory bank with content-addressed lookup via the first linear.

### Normalization (LayerNorm / RMSNorm) + residual stream
- **Definition**: LayerNorm normalizes activations; RMSNorm drops the mean-subtraction. The residual stream is the running sum threaded through every block — every layer reads from and writes to it.
- **Leverage**: the residual stream view (Anthropic's framing) reframes the whole model as "many parallel circuits writing into a shared channel." This is the cleanest mental model.
- **Source**: Ba et al. "Layer Normalization" (2016); Anthropic's "A Mathematical Framework for Transformer Circuits."
- **Maps onto**: a software bus / blackboard architecture.

### Autoregressive language modeling objective
- **Definition**: maximize `P(x_t | x_<t)` under the model, summed over a corpus. Cross-entropy loss on next-token prediction.
- **Leverage**: every emergent behavior (reasoning, instruction following, role-play) is downstream of this single objective + scale + post-training. Reasoning models bend it but don't replace it.
- **Source**: Bengio et al. "A Neural Probabilistic Language Model" (2003) for the lineage; GPT-2 paper for the modern version.
- **Maps onto**: maximum likelihood estimation; a generalized n-gram model.

### KV cache
- **Definition**: during autoregressive decoding, cache the K and V tensors of all previous tokens so each new token only needs to compute attention against the cache + itself.
- **Leverage**: this is the single biggest reason inference and training have different cost structures, and the single biggest constraint on long-context serving.
- **Source**: implicit in any production inference codebase; explicit in the FlashAttention and PagedAttention papers.
- **Maps onto**: memoization in a dynamic-programming recurrence; the difference between a Mealy machine recomputing vs. caching its state.

### Sampling (temperature / top-k / top-p / min-p)
- **Definition**: temperature scales logits before softmax; top-k restricts to top k tokens; top-p restricts to smallest set covering p mass; min-p restricts to tokens above p × max_prob.
- **Leverage**: most "bad output" complaints are actually sampler complaints, not model complaints.
- **Source**: Holtzman et al. "The Curious Case of Neural Text Degeneration" (top-p, 2019).
- **Maps onto**: simulated annealing temperature; rejection sampling.

### Scaling laws (Kaplan / Chinchilla)
- **Definition**: loss as a power-law in compute, parameters, and data: `L(N, D) ≈ A/N^α + B/D^β + L∞`. Chinchilla updated the optimal allocation toward more data.
- **Leverage**: this is what makes "how big a model should I train?" a math problem instead of vibes.
- **Source**: Kaplan et al. (2020); Hoffmann et al. "Training Compute-Optimal LLMs" (Chinchilla, 2022).
- **Maps onto**: economic production functions; isocost curves.

---

## Tier 3 — Advanced / applied

- **FlashAttention** — fused attention kernel that avoids materializing the N×N attention matrix in HBM. The block-wise softmax trick is the key insight.
- **Quantization**: INT8, INT4, GPTQ, AWQ, GGUF formats. K-means vs. round-to-nearest vs. learned-rounding. Per-channel vs. per-group.
- **Speculative decoding**: a small "draft" model proposes k tokens; the target model verifies via parallel forward pass and rejection-samples. Acceptance rate × draft-cheapness determines speedup.
- **Mixed precision training**: bf16 vs fp16 (range vs precision), loss scaling, master weights, optimizer state in fp32.
- **Gradient checkpointing**: trade compute for memory by recomputing activations during the backward pass.
- **Distributed training**: DDP, ZeRO stages 1/2/3, FSDP, tensor parallelism, pipeline parallelism, expert parallelism. Which one to reach for first depends on what's blowing up (params, activations, gradients, optimizer state).
- **Parameter-efficient fine-tuning**: LoRA, QLoRA, DoRA, IA³, prefix tuning. Why LoRA dominates: gradient flow goes through a small rank-r update, and you can hot-swap adapters.
- **Length extrapolation**: NTK-aware scaling, YaRN, position interpolation. All tricks that lean on RoPE's structure.
- **MoE routing**: top-k gating, load balancing loss, capacity factor, expert parallelism.
- **PagedAttention / vLLM**: treating the KV cache like virtual memory pages. Why it matters for batched serving.
- **Inference batching**: continuous batching vs. static batching, prefill/decode disaggregation, the "memory wall" of decode.

---

## Tier 4 — Frontier (2024-2026)

- **Reasoning models** (o1, R1, etc.): RL on verifiable rewards, process reward models, training-time chain-of-thought. The shift from "scale pretraining" to "scale RL on reasoning traces."
- **DeepSeek-V3 / R1** architecture: MLA (multi-head latent attention), MoE with auxiliary-loss-free routing, FP8 training. The most read recent open architecture paper.
- **Long context techniques**: ring attention, blockwise parallel transformer, sliding window + global tokens (Mistral / Gemma), state-space hybrids (Mamba, Jamba).
- **Speculative decoding extensions**: EAGLE, Medusa heads, draft-target distillation.
- **Mechanistic interpretability**: sparse autoencoders (SAEs), monosemantic features, circuit-level analysis (Anthropic's "Scaling Monosemanticity," 2024).
- **Multimodal native models**: tokenizing images/audio into the same vocabulary (Chameleon, Fuyu) vs. cross-attention adapters (Llava-style).
- **Hybrid architectures**: Mamba-Transformer hybrids (Jamba, Zamba), why pure SSMs underperform on associative recall, why hybrids work.
- **Synthetic data and self-play**: rejection-sampling fine-tuning, distillation from larger models, programmatic data generation for reasoning.

---

## Text concept map (substitute for Excalidraw)

```
                           [ Pretraining objective ]
                                     │
                                     ▼
        ┌───────────[ Transformer architecture ]───────────┐
        │                                                  │
        ▼                                                  ▼
[ Attention ]──────────────────────────────[ MLP / FFN (SwiGLU) ]
   │                                              │
   ├─ MHA / GQA / MQA                             └─ Most parameters live here
   ├─ Positional: RoPE / ALiBi                            │
   └─ FlashAttention (kernel)                             │
                                                          ▼
[ Residual stream ] ◄────── threads through every block ──┘
        │
        ▼
[ Tokenizer (BPE) ] ──► sets vocab, context math, multilingual quality

                                     │
                                     ▼
                          [ Training dynamics ]
                            │           │
                       [ AdamW ]   [ Scaling laws ]
                            │           │
                   [ Mixed precision, grad checkpointing, FSDP/ZeRO ]
                                     │
                                     ▼
                         [ Base model checkpoint ]
                                     │
                ┌────────────────────┼───────────────────┐
                ▼                    ▼                   ▼
        [ Inference ]          [ Post-training ]   [ Evaluation ]
            │                       │
        KV cache                  SFT
        Sampling                  LoRA / QLoRA
        Speculative dec.          RLHF / DPO
        Quantization              Constitutional AI
        PagedAttention            Reasoning RL
        FlashAttention
```
