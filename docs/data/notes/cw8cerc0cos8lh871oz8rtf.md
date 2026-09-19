
Two big things to be aware of in Babel are parsing and transforming:

### Parsing
Parsing is the process of taking source Javascript code and converting it into an [[AST|general.lang.AST]].
- The AST allows Babel to understand the code's structure and the relationships between different parts of the code. 

Parsing does not modify or transform the code in any way; it simply analyzes it and creates an AST.

### Transforming
Transforming is the process of making changes to the AST generated during parsing. In Babel, transforming involves modifying the AST to apply various changes to the code, such as transpiling modern JavaScript features into older versions of JavaScript that are compatible with a wider range of browsers and environments.

* * * 

### Gotchas
- Babel changes do not come into effect until we manually restart our server (e.g. if we are using nodemon or something similar). Therefore, a good rule of thumb is to *always* reset the server when we make changes to our `babel.config.js` file. This way, we don't break something without instantly knowing it.