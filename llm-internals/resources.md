# Resources: LLM Internals

Rule: every URL listed is one I'm confident exists at the canonical location named. Where I'm not certain of a stable URL, I cite by title/author/year only — search and verify.

## Foundational reading

- *The Annotated Transformer* — Harvard NLP / Sasha Rush. [verify link — published at nlp.seas.harvard.edu / annotated-transformer]
- *Speech and Language Processing* (3rd ed. draft) — Jurafsky & Martin. Chapters on transformers, fine-tuning. [verify link — web.stanford.edu/~jurafsky/slp3/]
- *Deep Learning* — Goodfellow, Bengio, Courville. Optimization + regularization chapters only.
- *Build a Large Language Model (From Scratch)* — Sebastian Raschka (2024). Book; pairs well with Phase 1-2.

## Papers & specs (canonical)

### Architecture
- "Attention Is All You Need" — Vaswani et al. (NeurIPS, 2017). arXiv:1706.03762
- "Language Models are Few-Shot Learners" (GPT-3) — Brown et al. (NeurIPS, 2020). arXiv:2005.14165
- "RoFormer: Enhanced Transformer with Rotary Position Embedding" — Su et al. (2021). arXiv:2104.09864
- "GQA: Training Generalized Multi-Query Transformer Models" — Ainslie et al. (2023). arXiv:2305.13245
- "GLU Variants Improve Transformer" — Shazeer (2020). arXiv:2002.05202

### Training
- "Adam: A Method for Stochastic Optimization" — Kingma & Ba (2014). arXiv:1412.6980
- "Decoupled Weight Decay Regularization" (AdamW) — Loshchilov & Hutter (2017). arXiv:1711.05101
- "Scaling Laws for Neural Language Models" — Kaplan et al. (2020). arXiv:2001.08361
- "Training Compute-Optimal Large Language Models" (Chinchilla) — Hoffmann et al. (2022). arXiv:2203.15556
- "The Llama 3 Herd of Models" — Meta (2024). arXiv:2407.21783

### Inference
- "Efficiently Scaling Transformer Inference" — Pope et al. (2022). arXiv:2211.05102
- "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness" — Dao et al. (NeurIPS, 2022). arXiv:2205.14135
- "FlashAttention-2" — Dao (2023). arXiv:2307.08691
- "Fast Inference from Transformers via Speculative Decoding" — Leviathan, Kalman, Matias (ICML, 2023). arXiv:2211.17192
- "Efficient Memory Management for Large Language Model Serving with PagedAttention" — Kwon et al. (vLLM, SOSP 2023). arXiv:2309.06180

### Quantization
- "GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers" — Frantar et al. (2022). arXiv:2210.17323
- "AWQ: Activation-aware Weight Quantization for LLM Compression and Acceleration" — Lin et al. (2023). arXiv:2306.00978
- "LLM.int8(): 8-bit Matrix Multiplication for Transformers at Scale" — Dettmers et al. (2022). arXiv:2208.07339

### Post-training & alignment
- "Training language models to follow instructions with human feedback" (InstructGPT) — Ouyang et al. (NeurIPS, 2022). arXiv:2203.02155
- "Direct Preference Optimization: Your Language Model is Secretly a Reward Model" — Rafailov et al. (NeurIPS, 2023). arXiv:2305.18290
- "LoRA: Low-Rank Adaptation of Large Language Models" — Hu et al. (2021). arXiv:2106.09685
- "QLoRA: Efficient Finetuning of Quantized LLMs" — Dettmers et al. (2023). arXiv:2305.14314
- "Constitutional AI: Harmlessness from AI Feedback" — Bai et al. (2022). arXiv:2212.08073

### Decoding / sampling
- "The Curious Case of Neural Text Degeneration" (top-p sampling) — Holtzman et al. (ICLR, 2020). arXiv:1904.09751

### Frontier (2024-2025)
- "DeepSeek-V3 Technical Report" — DeepSeek-AI (2024). arXiv:2412.19437
- "DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning" — DeepSeek-AI (2025). arXiv:2501.12948
- "Mamba: Linear-Time Sequence Modeling with Selective State Spaces" — Gu & Dao (2023). arXiv:2312.00752
- "Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet" — Anthropic (2024). [verify link — transformer-circuits.pub/2024/scaling-monosemanticity]

## Source to read

- `karpathy/nanoGPT` (github) — the reference 300-line GPT. Read `model.py` end to end. *Required* for Phase 1.
- `karpathy/llm.c` — same model in raw C/CUDA. Read after Phase 1 to see the kernels.
- `ggerganov/llama.cpp` — production-grade C++ inference; focus on `llama.cpp` (the main file) and one quantized matmul kernel in `ggml.c` / `ggml-quants.c`. *Required* for Phase 3.
- `huggingface/transformers` — `src/transformers/models/llama/modeling_llama.py`. The canonical reference implementation of a modern decoder-only model. Compare its `LlamaAttention` to nanoGPT's.
- `vllm-project/vllm` — production batching server. Read `vllm/attention/backends/` and the scheduler.
- `Dao-AILab/flash-attention` — the actual FlashAttention kernels. The CUDA is dense; focus on the Python wrapper and the algorithm comments.
- `huggingface/trl` — reference impls of SFT, DPO, PPO. Useful for Phase 4 to compare with your hand-rolled DPO.

## Talks & courses

- Karpathy, "Let's build GPT: from scratch, in code, spelled out" (YouTube, 2023). The single best Phase-1 companion.
- Karpathy, "Intro to Large Language Models" (1hr talk, 2023).
- Karpathy, "Let's reproduce GPT-2 (124M)" (YouTube, 2024).
- Stanford CS336 / CS25 — transformer lecture series. [verify current offering]
- Princeton COS 597G — Andrej Karpathy & Sasha Rush guest lectures on transformers (search YouTube by course name).
- 3Blue1Brown's "Neural Networks" series, episodes on transformers and attention (2024). Visual intuition.

## Communities

- `r/LocalLLaMA` (Reddit) — practical inference / quantization / fine-tuning chatter.
- `EleutherAI` Discord — research-grade discussion. [verify invite — check eleuther.ai]
- `huggingface` forums — fine-tuning and serving issues.
- arXiv-sanity (Karpathy's paper filter) — for tracking new releases.
- "Interconnects" newsletter by Nathan Lambert — post-training and RL on LLMs.
- "Import AI" by Jack Clark — frontier signal at a manageable cadence.

## What to skip

- Generic "intro to ML" courses. You don't need them.
- Most "build a chatbot with LangChain" tutorials. They're orthogonal to internals.
- HuggingFace's `Trainer` for the first pass. Write the training loop yourself once; then use `Trainer` afterward.
- Most "10x your prompting" content. This plan is about the other side of the API.
