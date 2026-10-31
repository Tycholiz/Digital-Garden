
With brace expansion, we can generate arbitrary strings
- similar to filename expansion (ex. `*.env`), except the generated strings actually need to exist.

To be a proper brace expansion, there must be opening and closing braces, and at least one `,`
```sh
$ echo a{d,c,b}e
# ade ace abe
```

Brace expansion is performed before any other expansions, and any characters special to other expansions are preserved in the result
- Bash does not apply any syntactic interpretation to the context of the expansion or the text between the braces.

### Alpha example
```sh
$ ls
alpha.py alphabeta.py

### Brace expansion
$ mv alpha{,beta}.py ../
```
