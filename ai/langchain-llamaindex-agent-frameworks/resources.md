# Resources: LangChain and LlamaIndex Agent Frameworks

## Foundational reading

- LangChain frameworks, runtimes, and harnesses overview: https://docs.langchain.com/oss/python/concepts/products
- LangChain agents docs: https://docs.langchain.com/oss/python/langchain/agents
- LangGraph workflows and agents docs: https://docs.langchain.com/oss/python/langgraph/workflows-agents
- LangGraph persistence docs: https://docs.langchain.com/oss/python/langgraph/persistence
- LlamaIndex framework overview: https://developers.llamaindex.ai/python/framework/
- LlamaIndex building an agent: https://developers.llamaindex.ai/python/framework/understanding/agent/
- LlamaIndex multi-agent patterns: https://developers.llamaindex.ai/python/framework/understanding/agent/multi_agent/
- LlamaIndex Workflows introduction: https://docs.llamaindex.ai/en/stable/workflows/

## Papers & specs

- "ReAct: Synergizing Reasoning and Acting in Language Models" — Yao et al. (ICLR, 2023): https://arxiv.org/abs/2210.03629
- "Toolformer: Language Models Can Teach Themselves to Use Tools" — Schick et al. (2023): https://arxiv.org/abs/2302.04761
- "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" — Lewis et al. (NeurIPS, 2020): https://arxiv.org/abs/2005.11401
- Model Context Protocol specification: https://modelcontextprotocol.io/specification/
- OpenTelemetry specification: https://opentelemetry.io/docs/specs/otel/

## Source to read

- LangChain Python repository: https://github.com/langchain-ai/langchain
- LangGraph repository: https://github.com/langchain-ai/langgraph
- LlamaIndex repository: https://github.com/run-llama/llama_index
- LangSmith SDK examples and tracing setup in official docs.
- LlamaIndex instrumentation examples, especially OpenTelemetry-related examples.

## Talks & courses

- LangChain Academy or official LangChain learning material: https://academy.langchain.com/
- LlamaIndex official examples and notebooks linked from the docs.
- Berkeley or public LLM agents course materials for agent patterns. Use these for concepts, not framework API details.

## Communities

- LangChain forum and GitHub discussions for framework changes and migration questions.
- LlamaIndex Discord and GitHub discussions for data-agent patterns and retrieval integrations.
- Framework release notes and migration guides. Check these before starting any real project.

## Link hygiene notes

- These APIs are actively evolving. Verify import paths and package names against official docs before coding.
- Prefer official docs and source code over blog posts for current framework behavior.
- Treat social posts as operational anecdotes only; do not encode them as architecture rules without reproduction.

