
instead of using indexes or positions, Linked lists use a referencing system: elements are stored in nodes that contain a pointer to the next node, repeating until all nodes are linked.
- Therefore, each node in a linked list contains a reference to the next node in the chain, in addition to its own data.

This system allows efficient insertion and removal of items without the need for reorganization.

Best used when data must be added and removed in quick succession from unknown locations

Linked lists do not use physical placement of data in memory, unlike arrays.

even if you have millions of elements inside a list, the operation of adding a new element in the head or in the tail of the list is performed in constant time.

The `head` is a reference to the first node in the linked list. The last node on the list points to `null`.
- Therefore, if the linked list is empty, the `head` points to `null`.

### vs Arrays
Simplistically, "Use an Array for storing and accessing data, and LinkedList to manipulate data."

Nodes can easily be removed or added from a linked list without reorganizing the entire data structure. This is not the case with arrays, and adding/removing an element to any position of the array that is not the head or tail is non-trivial, and the whole array needs to be destroyed and created again. In circumstances such as this, linked lists add a lot of value.

However, accessing nodes by position from a linked list is less efficient than with an array. If we consider that an array is just a contiguous row of memory blocks, then if we want to access `Array[5]`, then we just need to count up from the first block allocated to the array. With a linked list however, we need to manually traverse the chain until we reach the 5th node.

Also, search operations are slow in linked lists. Unlike arrays, random access of data elements is not allowed. Nodes are accessed sequentially starting from the first node.

#### Deleting nodes
If we want to remove a non head/tail node from a linked list, the node *behind* the deleted node must update its pointer value to point to the node that the deleted node was previously pointing to:

first -> second -> third
first -> <s>second</s> -> third
first -> third

To delete `second`, we must update `first` to now point to `third`.

### Code
This is what the head node (ie. first node) of a 3-element linked list looks like:
```js
ListNode {
  data: 'First data',
  next: ListNode {
    data: 'Second data',
    next: ListNode { data: 'Third data', next: null }
  }
}
```

If a linked node were a class, it would look like this:
```js
class ListNode {
    constructor(data) {
        this.data = data
        this.next = null                
    }
}
```

And the LinkedList itself would look like:
```js
class LinkedList {
    constructor(head = null) {
        this.head = head
    }
}
```

Implementing it would look like this:
```js
const node1 = new ListNode('First data')
const node2 = new ListNode('Second data')
const node3 = new ListNode('Third data')
node1.next = node2
node2.next = node3

const list = new LinkedList(node1)
console.log(list.head.next.next.data); // 'Third data'
// notice how we repeat `next` to get to the subsequent node. If we only had it
// once, it would have returned the second node.
```
