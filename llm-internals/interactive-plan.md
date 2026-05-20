# Interactive Plan: LLM Internals

**Timeline**: 4 weeks · ~5 hrs/week
**Started**: 2026-05-20
**Last touched**: 2026-05-20

## How to run a week

1. Open this file when you sit down for the week's session.
2. Find the next week with `Status: [ ] not started`.
3. Paste that week's section to your agent (Claude/Codex/etc.) along with:
   - what you finished last week
   - what surprised you
   - what you got stuck on
4. The agent runs the **Check-in prompts**, scores your deliverable, and applies the **Adjust-if** rules to subsequent weeks if needed.
5. Update `Status`, append notes under `Notes`, and commit (if tracked in git).

---

## Phase 1: Transformer mechanics

### Week 1 — Build a decoder-only transformer from scratch
- **Status**: `[ ] not started`
- **Goal**: Produce a single-file ~300-line PyTorch GPT that overfits a 1MB text file and that you can explain line by line.
- **Pre-read** (≤2 hrs):
  - "Attention Is All You Need" §3 (Vaswani et al., 2017)
  - Karpathy's "Let's build GPT" video — first 90 minutes
  - Skim `karpathy/nanoGPT/model.py` once before writing your own
- **Build / drill** (~3 hrs):
  - Write `model.py` from scratch (don't copy nanoGPT — re-derive). MHA from scratch, no `nn.MultiheadAttention`.
  - Verify causal-mask correctness with a unit test: perturbing token t shouldn't change logits at positions < t.
  - Overfit `input.txt` (any 1MB English) to loss < 0.5 in ≤500 steps.
- **Deliverable**: `model.py` + `train.py` + a screenshot/log of the loss curve hitting <0.5.
- **Check-in prompts** (agent asks):
  1. Show me your `forward()`. Walk me through every tensor shape with batch=4, seq=64, d=128, h=4.
  2. Why does causal masking happen inside attention rather than at the output? What would break if you moved it?
  3. Why is the LM head weight-tied to the embedding? What's the tradeoff?
  4. Write the 1-page teach-back: "What is a transformer, mechanically?" — can you do it without re-opening any source?
- **Adjust-if**:
  - If model never reaches loss < 1.0 → debug attention shapes first (causal mask, softmax axis). Repeat week with a smaller config (2 layers, d=64) and a 100KB corpus.
  - If you finished the build in < 2hrs and teach-back is sharp → add RoPE and GQA stretch goals; promote them into Week 2's pre-read budget.
  - If you can build it but can't explain pre-norm vs post-norm → don't move on; spend 1 extra hr reading "On Layer Normalization in the Transformer Architecture" (Xiong et al., 2020) and rewrite the teach-back.
- **Notes**:

---

## Phase 2: Training dynamics & optimization

### Week 2 — Train your model properly
- **Status**: `[ ] not started`
- **Goal**: Take your Week 1 model from "overfits 1MB" to "trains stably on a real small corpus with modern training hygiene."
- **Pre-read** (≤2 hrs):
  - "Decoupled Weight Decay Regularization" (AdamW), Loshchilov & Hutter (2017) — focus on §3
  - "Scaling Laws for Neural Language Models," Kaplan et al. (2020) — §3 + figures
  - Skim the Chinchilla paper's abstract + §3 (Hoffmann et al., 2022)
- **Build / drill** (~3 hrs):
  - Add: AdamW, cosine LR schedule with warmup, gradient clipping, bf16 autocast.
  - Add: proper eval loop (held-out loss + sample generation).
  - Train on TinyStories (or equivalent ~100MB corpus) for 1-2 epochs.
  - Run a Chinchilla-style 3-point scan within your budget: same total FLOPs, three (N, D) splits. Plot loss vs N.
- **Deliverable**: A loss curve + a 1-page training-run write-up explaining what each toggle changed.
- **Check-in prompts**:
  1. Show me the loss curve. What does the first 100 steps look like, and why?
  2. Without AdamW's decoupled weight decay, what specifically would go wrong with the LN/embedding params?
  3. Did your Chinchilla scan agree with the paper? If not, why might it disagree at your scale?
  4. You see a loss spike at step 4000. Walk me through three diagnoses and three mitigations.
- **Adjust-if**:
  - If training diverges (loss → NaN) repeatedly → drop bf16, drop LR, then re-add one at a time. Don't move on with a hack; understand the root cause.
  - If you blow past the build in < 3hrs → add gradient checkpointing and measure the memory/compute tradeoff. Also add `torch.compile` and benchmark.
  - If the Chinchilla scan is too expensive to run within hardware → switch to a paper-tracing exercise: re-derive Chinchilla's compute-optimal frontier from the published numbers in §3 of Hoffmann et al.
- **Notes**:

---

## Phase 3: Inference internals

### Week 3 — KV cache + samplers + speculative decoding
- **Status**: `[ ] not started`
- **Goal**: Make your Week 2 model serve fast. Understand where every microsecond of decode goes.
- **Pre-read** (≤2 hrs):
  - "Fast Inference from Transformers via Speculative Decoding," Leviathan et al. (2023) — read in full, it's short
  - "FlashAttention" paper, Dao et al. (2022) — algorithm section + figures only
  - Skim `huggingface/transformers/.../modeling_llama.py` `LlamaAttention.forward` and find where it handles `past_key_values`
- **Build / drill** (~3 hrs):
  - Add KV cache to your model: attention now accepts and returns `(K, V)` per layer.
  - Implement temperature, top-k, top-p, min-p as a composable sampler pipeline.
  - Distill a tiny draft model (4 layers, d=128) from your Week 2 target.
  - Implement speculative decoding with k=4 draft tokens; measure acceptance rate and tokens/sec vs naive and vs cached-only.
- **Deliverable**: A benchmark table — naive / KV-cache / KV-cache+speculative — tokens/sec at seq=512 and seq=2048, plus acceptance rate for speculative.
- **Check-in prompts**:
  1. Compute the KV cache size in MB for your model at batch=1, seq=2048. Now do it for GQA with kv_heads=2. Show your math.
  2. What's the correctness guarantee for speculative decoding? Why does the *same distribution* come out despite using a different model?
  3. Show me your acceptance rate. If it's low, what would you change about the draft model?
  4. Why is decode memory-bandwidth bound and prefill compute bound? Where in your code can you see this?
- **Adjust-if**:
  - If acceptance rate < 0.3 → draft model is mismatched. Either train it on outputs of the target (distillation) or shrink the divergence by sharing layers.
  - If KV cache integration breaks shape-wise repeatedly → step back, write a 6-line pseudocode of the autoregressive recurrence with cache before re-coding.
  - If you breeze through speculative → add INT8 quantization on the LM head and measure latency + perplexity delta; or implement PagedAttention.
- **Notes**:

---

## Phase 4: Post-training & alignment

### Week 4 — SFT → LoRA → DPO
- **Status**: `[ ] not started`
- **Goal**: Take a base model through the modern post-training pipeline, end-to-end, with hand-rolled SFT/LoRA/DPO.
- **Pre-read** (≤2 hrs):
  - "InstructGPT" — Ouyang et al. (2022). Read §3 + Figure 2.
  - "Direct Preference Optimization" — Rafailov et al. (2023). Read in full; derive the loss yourself.
  - "LoRA" — Hu et al. (2021). §3 + §4.
- **Build / drill** (~3 hrs):
  - SFT on a small instruction dataset (Alpaca / Dolly-15k) using a proper chat template with label masking on prompt tokens.
  - Implement LoRA from scratch (`B @ A @ x * scaling`) and inject into Q,V projections. Verify only LoRA params have gradients.
  - Implement DPO loss from the paper. Train on a small preference dataset (HH-RLHF subset / UltraFeedback).
  - Compare base vs SFT vs SFT+DPO on 20 held-out prompts (side-by-side outputs).
- **Deliverable**: A markdown table with 20 prompts × 3 columns (base/SFT/SFT+DPO outputs), plus your derivation of the DPO loss from the RLHF objective.
- **Check-in prompts**:
  1. Walk me through the DPO loss term by term. Why does the reference-model term cancel cleanly?
  2. Sketch the gradient flow through a LoRA-adapted attention layer. What gets a gradient and what doesn't?
  3. Compare base vs SFT vs DPO outputs on 3 prompts where they diverge. What does the divergence tell you?
  4. When would you still reach for RLHF (PPO) over DPO? Be specific.
- **Adjust-if**:
  - If SFT looks identical to base → check your label masking on prompt tokens; you may be training on the wrong tokens.
  - If DPO collapses (model outputs degenerate) → β too high or reference model drift; either lower β or reset reference snapshot.
  - If you finish early → implement KTO or IPO and compare on the same data; or add a small PPO loop with a reward model to feel the operational cost difference vs DPO.
- **Notes**:

---

## End-of-plan review

Once all weeks are `done` or intentionally `skipped`, run this session with the agent:

1. Walk through every Phase deliverable. What's worth showing to a colleague?
2. Re-attempt the Week 1 teach-back ("What is a transformer, mechanically?") — is it sharper than 4 weeks ago?
3. List open questions from your `Notes` and `progress.md` "what I still don't understand" entries.
4. Decide: deepen (loop on Phase 3 with FlashAttention kernel writing, or Phase 4 with PPO), apply (use these skills in a real project — fine-tune for a domain task), or pivot (next adjacent topic: distributed training? mech-interp? MoE?).
