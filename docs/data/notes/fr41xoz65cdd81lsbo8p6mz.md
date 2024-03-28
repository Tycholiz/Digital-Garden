
Used for storing a dynamic set of strings, where the keys share prefixes. 
- commonly used in dictionary and autocomplete applications.
- [[time complexity|general.algorithms.big-o]] of `O(n)` where `n` is length of searched prefix.

```ts
class TrieNode {
    children: Map<string, TrieNode>;
    isEndOfWord: boolean;

    constructor() {
        this.children = new Map();
        this.isEndOfWord = false;
    }
}

class Trie {
    root: TrieNode;

    constructor() {
        this.root = new TrieNode();
    }

    add(word: string): void {
        let node = this.root;
        for (const char of word) {
            if (!node.children.has(char)) {
                node.children.set(char, new TrieNode());
            }
            node = node.children.get(char) as TrieNode;
        }
        node.isEndOfWord = true;
    }

    get(prefix: string): string[] {
        const result: string[] = [];
        let node = this.root;
        for (const char of prefix) {
            if (!node.children.has(char)) {
                return result;
            }
            node = node.children.get(char) as TrieNode;
        }

        this.collectWords(node, prefix, result);
        return result;
    }

    private collectWords(node: TrieNode, prefix: string, result: string[]): void {
        if (node.isEndOfWord) {
            result.push(prefix);
        }

        for (const [char, child] of node.children.entries()) {
            this.collectWords(child, prefix + char, result);
        }
    }
}

// Example usage:
const trie = new Trie();
trie.add("apple");
trie.add("app");
trie.add("banana");

console.log(trie.get("ap")); // Output: ["apple", "app"]
console.log(trie.get("b")); // Output: ["banana"]
console.log(trie.get("c")); // Output: []
```