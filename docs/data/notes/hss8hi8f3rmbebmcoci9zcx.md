
## How an LLM works
### Temperature
Temperature determines how deterministic the model's response will be. This is not baked into the model itself, but is rather a parameter that is provided at the time we submit the prompt. Higher temperatures make the output much more random. Tokens have a probability assigned to them, and when we increase the temperature value, we are giving even tokens with lower probabilities a fighting chance.

Temperature = 1.0: Uses probabilities as-is
Temperature < 1.0: Makes high-probability tokens even more likely (sharpens the distribution)
Temperature > 1.0: Makes all tokens more equally likely (flattens the distribution)

mn: when humans have a high temperature (ie. fever), we tend to hallucinate more.

For tasks like coding, we want a low temperature, since we don't want random selections; We want the highest probability token selection for coding.

### Context
An LLM doesn't remember context. Instead, to have any semblance of a memory, chat history has to be included in the prompt. There is a limit to the amount of tokens that can be included in the context
- ex. As of June 2025, Claude has a context window size of 200,000 tokens, corresponding to ~150,000 words (about 500 pages of text).

Larger contexts have trade-offs:
- Cost: Longer context = more expensive to process
- Speed: Takes longer to process massive contexts
- Quality: Some models perform worse with very long contexts (attention dilution, due to more tokens in competition during weighting)

### Tokens
A Tokenizer can be thought of as the LLM's dictionary and translator. It converts human text into numbers that the model can understand, and vice versa.

Breaking down text: "Hello world" might become tokens like ["Hello", " world"] or even ["Hel", "lo", " wor", "ld"] depending on the tokenizer
Converting to numbers: Each token gets assigned an ID number, so "Hello" might be token #1234
Vocabulary: Contains all possible tokens the model knows (usually 30,000-100,000+ tokens)
Handles unknowns: Breaks down unfamiliar words into smaller pieces it recognizes

Example: "unhappiness" might tokenize as ["un", "happy", "ness"] if those are known subword pieces.

### Weights
Weights are the "brain" of the model - billions of numbers that encode everything the model learned during training.

The weights in an LLM are absolutely fundamental to its effectiveness - they essentially are the model. The weights encode all the learned patterns, knowledge, and capabilities that emerge from training on massive datasets. Without the specific weight values, you just have an empty neural network architecture that can't perform any meaningful tasks.
- weights represent the core intellectual property - the result of millions of dollars in compute, data curation, and research
- if competitors get access to another model's weights, they are able to replicate capabilities without the massive training costs

Neural network parameters: Each connection between neurons has a weight value
Learned patterns: Weights capture relationships like "after 'The cat sat on the', 'mat' is more likely than 'elephant'"
Massive scale: A 7B parameter model has 7 billion weight values, a 70B model has 70 billion
File size: This is why model files are so large - storing billions of decimal numbers takes lots of space

How They Work Together:

Tokenizer converts your text input to numbers
Those numbers flow through the neural network guided by the weights
Weights determine what the model "thinks" comes next
Output numbers get converted back to text via the tokenizer

The weights are what make each model unique - they're the actual "intelligence" learned from training data.

* * *

### Integration Tools
#### LangChain
LangChain is a framework for developing applications powered by large language models (LLMs) Introduction | 🦜️🔗 LangChain. Think of it as scaffolding that makes it much easier to build complex LLM applications. Here's what it actually does:

What LangChain Solves:
- The "Glue Code" Problem: Without LangChain, building LLM applications means writing lots of repetitive code to connect different pieces - APIs, databases, prompt templates, error handling, etc. LangChain provides pre-built components for common patterns.
- Standardization: At the core of LangChain is the ability to seamlessly integrate with a variety of large language models (LLMs) from different providers, such as OpenAI, Anthropic, and Google. LangChain provides a standardized interface to interact with these What is Langchain? - Analytics Vidhya models, so you can switch between providers without rewriting your application.
- Complex Workflows: LangChain provides tools and abstractions to improve the customization, accuracy, and relevancy of the information the models generate. For example, developers can use LangChain components to build new prompt chains or customize existing templates What is LangChain? - LangChain Explained - AWS.

Key LangChain Components:
- Chains: Link multiple LLM calls together. For example, summarize a document, then translate the summary, then extract key points.
- Agents: LLMs that can use tools - like searching the web, running code, or querying databases - and decide which tools to use based on the task.
- Memory: Give LLMs persistent memory across conversations or sessions.
- Retrievers: Connect to external data sources (databases, documents, APIs) for RAG applications.
- Prompt Templates: Reusable, parameterized prompts that you can modify programmatically.

Real-World Example:
Imagine building a customer service system:
Without orchestration:
```
1. Manually parse customer email
2. Write custom code to search knowledge base
3. Write custom code to call LLM with context
4. Write custom code to validate response
5. Write custom code to format reply
6. Handle errors at each step individually
```
With LangChain orchestration:
```
pythonchain = (
    EmailParser() 
    | KnowledgeBaseRetriever() 
    | ResponseGenerator() 
    | QualityValidator() 
    | EmailFormatter()
)
response = chain.invoke(customer_email)
```

The framework handles error propagation, retries, logging, and coordination automatically.

### Model Hosting
These platforms essentially created the "cloud computing" equivalent for AI models - turning AI from a capital expenditure requiring specialized knowledge into an operational expense that any developer can use.

- Hugging Face: offers hosted inference through their "Inference Endpoints"; Use case: Experimenting with different models, prototyping, community-driven projects
- Replicate: Focus on simplicity - run models through simple API calls; Dead-simple APIs, good for non-technical users, handles image/video models well; integrating AI into apps without ML expertise
- Together AI: Specialized in hosting large language models efficiently

## Blogs
- https://www.philschmid.de/
- https://huggingface.co/blog
- https://blog.langchain.dev/