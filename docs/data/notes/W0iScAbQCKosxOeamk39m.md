
## Compiled vs Interpreted Languages
- A compiler figures out everything a program will do, turns it into “machine code” (a format the computer can run really fast), then saves that to be executed later.
- An interpreter steps through the source code line by line, figuring out what it’s doing as it goes.

Technically any language could be compiled or interpreted, but one or the other usually makes more sense for a specific language. 
- Generally, interpreting tends to be more flexible, while compiling tends to have higher performance.
- a lot of language design decisions are affected by the decision to make a language interpreted or compiled 
    - for example, static typing is a big benefit to compiled languages, but not so much for interpreted ones.

Interpreted languages are generally easier design, build and learn

### Writing your own language
If you are writing an interpreted language, it makes a lot of sense to write it in a compiled one (like C, C++ or Swift) because the performance lost in the language of your interpreter and the interpreter that is interpreting your interpreter will compound.

If you plan to compile, a slower language (like Python or JavaScript) is more acceptable. Compile time may be bad, but that isn’t nearly as big a deal as bad run time.

#### From source file to execution
A programming language is generally structured as a pipeline. The first stage is a string containing the entire input source file. The final stage is something that can be run. This will all become clear as we go through the Pinecone pipeline step by step.

##### Lexer
The first stage. The lexer is supposed to take in a string containing an entire files worth of source code and spit out a list containing every token (a token is a unit of the language, like a function name or an operator).
- also included in tokenization is determining how whitespace is handled, brackets, semi-colons etc. Essentially, we are focused on the atomic units of the language here.

The lexer provides all details of the source file that are needed by future stages.

##### Parser
The parser turns a list of tokens into a tree of nodes. 
- A tree used for storing this type of data is known as an [[AST|general.lang.AST]].

## Terms
### Expression
An expression is a piece of code that will be evaluated to produce a value, or a piece of code that is already at its furthest level of evaluation (string, number etc) 
- ex. `5`
- ex. `"hello"`
- ex. `1 / 2` evaluates to `0.5`
- ex. `i++`
- ex. `'A' + 'string'`
- ex. `a && b`
- ex. `a ? b : c`
- ex. `function() {}`
- ex. `window.resize()`

### Assignment
- any time there is a LHS and RHS separated by `=`
