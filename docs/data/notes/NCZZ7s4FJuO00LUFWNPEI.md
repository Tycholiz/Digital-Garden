
A Deque (pronounced "Deck") is a Double-ended queue, meaning elements can be added to or removed from either the front (head) or back (tail).

The basic operations on a deque are enqueue (add) and dequeue (remove) on either end.

Deques are normally implemented with:
- A modified dynamic array that can grow from both ends
    - which has time-complexity of `O(1)` for all operations except insertion/deletion in middle, which is `O(n)`
- A doubly linked list
    - which has time-complexity of `O(1)`
