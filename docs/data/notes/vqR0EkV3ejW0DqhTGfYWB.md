
### File Compression
- The core idea of file compression is: the content within a file has redundancies. Why not list data once, then simply back to it when it occurs again?
	- ex. the JFK quote: *"Ask not what your country can do for you -- ask what you can do for your country."* has 17 words, made up of 61 letters, 16 spaces, 1 `-` and 1 `.`. If each character was 1 unit of memory, that would be 79 units. If instead of storing each instance of a word in memory, we just refered the repeated words back to the first occurrence, we would cut the phrase in half.
- In reality, the compression program doesn't look for words, but looks for patterns. Therefore, the larger the file we are compression, the more compression there will be.
	- This is why binary files like mp3 don't compress well — there is hardly any repetition of patterns.
