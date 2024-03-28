
A hash table is an alternative to storing the type of data that we might find in an array. In a hash table, a large pool of memory is allocated, and a hash function is chosen that always returns values lying somewhere within the available memory. In order to store a value, the key is hashed, and the value is placed at the location given by the hash. To look a value up given a key, you just hash the key and get back the location of the corresponding value.

One complication of this method is that if two keys hash to the same value, the previous value would be overwritten. This is called a hash collision, and any hash table implementation must have a method of dealing with them.

A hash table is an implementation of an associative array (also called a dictionary, map,symbol table or key-value store).

## Purpose
A hash table is one particular way of storing items that are associated with a key in such a way that the item can be looked up quickly using the key.
- The key might be a person's name, an ID number, a directory path or anything else.

Let's say you want to write a program, for example, that calculates how frequently each word is used in a book. You could use the word as the key, and the value associated with the key would be the number of times that word has been seen. If we wanted to access this information fast, we could store the results in a hash table, and make that data available for consumers. This could especially be useful if we need multiple clients need to access the same data.

Let's say you want to store video game information in hash tables (like weapon stats). You could use hash tables to store the data for easy querying.
- To give an idea of the scope of what could be included in a hash table, we might include weapon stats, the enemy being hit, your buffs, your accessories, etc. You formulaicly concatenate them together, like:
```
[weapon][buff1][buff2][accessory1][accessory2][accessory3][enemy][random]
```

and then you take the hash of that, and look up the resulting damage in a pre-computed table, indexed by that hash. That way, you're not spending in-game computation time adding, multiplying, subtracting, dividing, and etcetera for potentially dozens or hundreds of damage incidents each second, each of which has to be calculated instantly


hash tables have been called "the backbone of most programming".

Hashing and trees perform similar jobs, but have different usefulness:
- Trees are more useful when an application needs to order data in a specific sequence. 
- Hash tables are the smarter choice for randomly sorted data due to its key-value pair organization.

## Composition

Hash tables are made up of two parts:
1. *Object*: An object with the table where the data is stored. The array holds all the key-value entries in the table. The size of the array should be set according to the amount of data expected.
2. *Hash function* (or mapping function): This function determines the index of our key-value pair. It should be a one-way function and produce the a different hash for each key.
    - takes an item’s key as an input, assigns a specific index to that key and returns the index whenever the key is looked up.

*Hashing* is the process of assigning an object into a unique index, known as a key. Each object is identified using a key-value pair, and the collection of objects is known as a dictionary.
- Hash functions help to limit the range of the keys to the boundaries of the array

## Implementation

A hash table is implemented by storing elements in an array and identifying them through a key. A hash function takes in a key and returns an index, giving us the location of the data in the array.
- whenever you input the key into the hash function, it will always return the same index, which will identify the associated element.

In JavaScript, hash tables are generally implemented using arrays as they provide access to elements in constant time.

A hash table has a number of useful applications:
- When a resource is shared by multiple consumers
- Password verification
- Linking file name and path

Some common uses of hash tables are:
- Database indexing
- Caches
- Unique data representation
- Lookup in an unsorted array
- Lookup in sorted array using binary search

Hash tables provide access to elements in constant time, so they are highly recommended for algorithms that prioritize search and data retrieval operations. 
Hashing is ideal for large amounts of data, as they take a constant amount of time to perform insertion, deletion, and search.

time complexity is `0(1)`

To properly understand the difference between a map and an object (in JavaScript), we have to understand the difference between a concept and its implementation. For instance, we most think of a POJO when needing to implement a hash map in JavaScript. However, we can also use a Map object, and we can also just use an array. The implementation in all 3 cases are differ, but in the end of each, we end up with a hash map.
