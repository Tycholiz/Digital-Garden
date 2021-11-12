
An array is the most basic of all data structures. The implementation of its data storage is low-level, since adjacent memory blocks are used to store the elements. Therefore, to find elements we just need to count by memory blocks.

An array has a fixed number of cells that are allocated upon creation. This makes it expensive to "insert" or "delete" values, as the original array (at its original memory location) cannot easily be reused.

### List vs Tuple
Arrays commonly are used to implement Lists and Tuples.
- A list is where all elements have the same shape and the length is often unknown
- A tuple has a fixed length and elements can be of different types