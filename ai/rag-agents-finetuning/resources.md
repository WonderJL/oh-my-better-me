# Resources: RAG, AI Agents, and Model Fine-Tuning

## Foundational reading

- "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" — Lewis et al. (NeurIPS, 2020): https://arxiv.org/abs/2005.11401
- "ReAct: Synergizing Reasoning and Acting in Language Models" — Yao et al. (ICLR, 2023): https://arxiv.org/abs/2210.03629
- "Toolformer: Language Models Can Teach Themselves to Use Tools" — Schick et al. (2023): https://arxiv.org/abs/2302.04761
- "LoRA: Low-Rank Adaptation of Large Language Models" — Hu et al. (2021): https://arxiv.org/abs/2106.09685
- "QLoRA: Efficient Finetuning of Quantized LLMs" — Dettmers et al. (NeurIPS, 2023): https://arxiv.org/abs/2305.14314

## Papers & specs

- "Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection" — Asai et al. (2023): https://arxiv.org/abs/2310.11511
- "RAGAS: Automated Evaluation of Retrieval Augmented Generation" — Es et al. (EACL Demo, 2024): https://arxiv.org/abs/2309.15217
- "Reflexion: Language Agents with Verbal Reinforcement Learning" — Shinn et al. (2023): https://arxiv.org/abs/2303.11366
- "DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines" — Khattab et al. (2023): https://arxiv.org/abs/2310.03714
- "A Practical Guide to Building Agents" — OpenAI (2025/2026): https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf

## Official docs

- LlamaIndex introduction to RAG: https://docs.llamaindex.ai/en/stable/understanding/rag/
- OpenAI Agents guide: https://platform.openai.com/docs/guides/agents
- OpenAI Agents SDK docs: https://platform.openai.com/docs/guides/agents-sdk/
- LangChain agents reference: https://reference.langchain.com/python/langchain/agents
- DSPy GitHub and docs hub: https://github.com/stanfordnlp/dspy
- Hugging Face PEFT LoRA docs: https://huggingface.co/docs/peft/en/package_reference/lora
- OpenAI fine-tuning help center entry: https://help.openai.com/en/articles/11162441-how-can-i-get-started-with-fine-tuning

## Source to read

- `stanfordnlp/dspy` examples — read how modules, signatures, and optimizers replace hand-tuned prompt strings: https://github.com/stanfordnlp/dspy
- `huggingface/peft` LoRA implementation — read configuration surfaces and adapter lifecycle: https://github.com/huggingface/peft
- LangChain / LangGraph agent examples — read how graph state and tool calls are represented before adopting the framework.
- LlamaIndex examples — read ingestion, node parsing, query engines, and eval examples.

## Talks & courses

- Hugging Face open-source AI cookbook RAG evaluation guide: https://huggingface.co/learn/cookbook/en/rag_evaluation
- OpenAI platform docs and cookbook examples for agents, evals, and fine-tuning. Prefer current docs over archived blog posts.
- Conference talks from ICLR, NeurIPS, and ACL on RAG, agent evaluation, and PEFT. Use paper titles above as search anchors.

## Communities

- Hugging Face forums for PEFT, Transformers, and open-model fine-tuning.
- LlamaIndex and LangChain community forums or Discord servers for framework-specific implementation issues.
- Papers With Code and arXiv for tracking newer RAG, agent, and PEFT papers.

## Link hygiene notes

- Fine-tuning provider availability changes quickly. Check current vendor docs before planning hosted training work.
- Treat blog posts as implementation hints, not authoritative sources.
- For frontier papers, validate claims by reproducing a small eval rather than copying leaderboard conclusions.

