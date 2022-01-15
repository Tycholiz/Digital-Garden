
# Stack
Contrast with [[heap|memory.heap]]
- A place in memory where variables declared by a function are stored
- Any time we create a variable In a normal way, we are putting it on the stack
- Every time a function declares a new variable, it is "pushed" onto the stack. Then every time a function exits, all of the variables pushed onto the stack by that function are deleted.
- when a function exits, all of its variables are popped off of the stack. Thus stack variables are local in nature
- variables are declared, stored and initialized during runtime.
- Storage is temporary, and when The computing task is complete, the memory location will be erased
- Data structure is linear
- Data is physically located together (contiguous blocks)
- Variable de-allocation is automatic
