
#### What may cause an algorithm to not be deterministic?
1. The algorithm uses external state, that is not passed in as an argument to the function
    - ex. global variable, hardware timer value, random value etc.
2. Timing is sensitive. If the algorithm is run two times, is there a possibility that different processes of the algorithm finish in different orders (e.g. if there are multiple processes)
3. hardware error causes its state to change in an unexpected way.

Although real programs are rarely purely deterministic, it is easier for humans as well as other programs to reason about programs that are.

most programming languages and especially functional programming languages make an effort to prevent the above events from happening except under controlled conditions

### Making an algorithm that is open-ended
Consider a function that counts occurrences in an array
```js
const countVowels = (str) => {
    const vowels = { a: 0, e: 0, i: 0, o: 0, u: 0 }

    for (let i = 0; i < str.length; i++) {
        const currentChar = str[i].toLowerCase()
        if (Object.keys(vowels).includes(currentChar)) {
            vowels[currentChar]++
        }
    }
    return Object.values(vowels).reduce((acc, sum) => acc + sum)
}
```

Conceptually, this could be made a bit simpler, because we don't actually care about the breakdown of each vowel. In other words, we don't really care if there are 2 `a`s, or 3 `e`s... All we care about is how many there are in total. Therefore, we could have just iterated on the string and counted the vowels and returned that value. However, there is a benefit to having put the results of the vowel count into an object. It may be a reasonable assumption that the breakdown of values is of value (or future value) to us. Building our algorithm this way makes it a simple adjustment to include the breakdown, and doesn't really add much overhead.
