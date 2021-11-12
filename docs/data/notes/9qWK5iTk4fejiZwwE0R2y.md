
### Approach for algorithmic problems
Example: implement a method on a LinkedList class that will delete a node from that list, given a position.

#### Have a clear understanding of what the resulting data will be after the algorithm is run.
First, we have to understand what does it mean to delete an item from a linked list. In technical terms, it means to change the pointer of the previous node to point to the node after the one being deleted. If we had a linked list of 4 elements:

first -> second -> third -> fourth

and wanted to remove the node at the second index:

first -> second -> fourth

We would need to modify the second node's `next` value to point to the fourth element. This is effectively deleting a node from a linked list. If this is not clear to us, then it is impossible to write an algorithm that solves the problem.

"A problem well put is a problem half-solved."

A way to fulfill this step might be to satisfy some test cases. For example...
- If the list does not have a head, then return early, since there are no elements to remove
- If we want to delete the node at position 0 (the head), then we need to assign a new head. 
    - Therefore, after the removal does `list.head` properly point to the second node instead of the first?
- If we want to delete any other node, we need to change the `next` value of the previous node so that it refers to the node that was after the deleted node.

### Tips
- Best starting point might be the end-interface. This answers the question "how would my code be used by others?". If you are implementing a Binary Search Tree, then you probably want users to be able to add an element something like this: `tree.insert(32)`. From there, work backwards.
- Any time you have to map over an array and you have to *remember* something about each iteration, consider using a map/object as a way to remember
    - ex. The problem "loop over array, sort it, and return an array of the original indices" requires us to remember which index the number was at before we sort it. We can solve this by making an object where the key is the original index, and the value is the value at that index. From there, we can sort it and do whatever else we want with it, because we have already "remembered" that mapping between the value and its index.