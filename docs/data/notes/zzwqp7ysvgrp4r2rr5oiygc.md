
MCP is an open standard that enables developers to build secure, two-way connections between their data sources and AI-powered tools
- ex. if you ask your AI assistant to analyze sales data from a local database, the assistant sends a request to the MCP server, which gets the data and sends it back to the assistant. It’s like ordering takeout — the AI assistant places the order, the MCP server delivers the data, and the database is the restaurant.

## Open Source MCP Servers
### Sequential Thinking
Sequential Thinking provides a tool for dynamic and reflective problem-solving through a structured thinking process. Instead of trying to tackle large, overwhelming problems all at once, it guides you through breaking them into logical, sequential steps

Instead of diving headfirst into implementation, MCP Sequential Thinking emphasizes:
1. Precise Problem Definition: Clearly articulating the problem you're trying to solve, leaving no room for ambiguity.
2. Atomic Sub-Task Decomposition: Breaking down the problem into smaller, manageable, and independent sub-tasks.
3. Dependency Sequencing: Identifying and organizing the dependencies between these sub-tasks to ensure a logical execution flow.
4. Optimized Execution Flow: Streamlining the execution of these sub-tasks for maximum efficiency and effectiveness.

With Sequential Thinking, the LLM is basically writing notes to itself, step by step. The MCP server just collects those notes and feeds them back, so each time the LLM writes a new thought, it sees its whole chain of thinking so far.
- It’s like the LLM is creating its own prompts instead of waiting for the user - it reads its own past thoughts and decides the next step. Same way you’d prepend or append a bunch of messages before sending a prompt to an LLM, except here the model builds that chain itself as it thinks.

OpenRouter is good to pair with Sequential Thinking, since it gives us access to many different LLM models via a single interface, allowing us to leverage the unique strengths of different models for each step of the sequence.

### Context7
Context7 MCP pulls up-to-date, version-specific documentation and code examples straight from the source — and places them directly into your prompt

Context7 tackles the problem of stale information, acting as a dynamic bridge between your coding prompt and software documentation found on the internet. This ensures the chatbot response is using up to date version code, instead of potentially outdated training data.

### Firecrawl
An MCP that takes a URL, crawls it, and converts it into clean markdown or structured data. 