
A RAG is a way of including authoritative knowledge as input into an LLM.
- ex. if we implement an LLM for our hospital, we want to include an authoritative set of data that the LLM can use in its output.

Since an LLM does not remember context, all input information has to be included in the prompt. A RAG system has access to the prompt the user submits and is able to use that prompt to retrieve relevant information from a vector database, which then gets included in the prompt.
- ex. 

Most RAG systems take your documents and chop them into tiny, isolated chunks. Each chunk lives in its own bubble. When you ask a question, the system retrieves a handful of these fragments and expects the AI to make sense of them. The result is a disconnected, context-poor answer that often misses the bigger picture.

### Graph RAG
Instead of isolated chunks, it builds a rich network of interconnected knowledge. Think of it as a well-organised library, complete with cross-references and relationships. When you ask a question, Graph RAG does not just find relevant information. It understands how everything connects, delivering answers that are both accurate and deeply contextual.

This is not just theory. Research shows that Graph RAG systems can reduce token usage by 26% to 97% while delivering more accurate, contextual responses. The difference is not subtle. By understanding relationships, Graph RAG provides answers that make sense, not just answers that match keywords.

#### Open Source Tools
- TrustGraph
- Mem0.ai

https://www.chrismdp.com/graph-rag/