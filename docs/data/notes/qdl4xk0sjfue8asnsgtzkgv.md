
Note: you should never need to run any of these commands directly. Their use should be restricted to learning only.

#### `cat-file`: display contents of an object
```sh
# show object type
git cat-file -t <SHA>

# show object content
git cat-file -p <SHA>

# show the tree object associated with a revision
git cat-file -p main^{tree}
```

#### `hash-object`
```sh
# -w to write to object database
echo 'hello' | git hash-object -w --stdin
```

#### `ls-tree`: list the tree contents for any given revision
```sh
git ls-tree <SHA> .
```

#### `rev-parse`: see which SHA-1 a branch points to

#### `write-tree`: write the contents of the index to a tree object

#### `commit-tree`: create a commit object from a specified tree and a parent
```sh
git commit-tree 
```