
### Processors
Any time we want to make a lint rule for a non-`.js` file, we need to include a processor 

A processor is composed of a preprocessor and a postprocessor.

#### Preprocessor
The preprocess method takes the file contents and filename as arguments, and returns an array of code blocks to lint
A code block has two properties text and filename; 
- the text property is the content of the block
- filename property is the name of the block.
    - Name of the block can be anything, but should include the file extension, that would tell the linter how to process the current block
The linter will consult the `--ext` extensions list of the linting command as executed on the CLI.

#### Postprocessor

* * *
# E Resources
[Eslint docs for custom rules](https://eslint.org/docs/developer-guide/working-with-rules)
[Eslint docs for custom plugins](https://eslint.org/docs/developer-guide/working-with-plugins)
[Create custom eslint rules](https://www.webiny.com/blog/create-custom-eslint-rules-in-2-minutes-e3d41cb6a9a0)
[creating an eslint plugin](https://medium.com/@bjrnt/creating-an-eslint-plugin-87f1cb42767f)