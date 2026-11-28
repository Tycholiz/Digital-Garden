
Since Javascript is an interpreted language, it needs an engine to interpret and execute code.
- The V8 engine interprets JavaScript and compiles it down to native machine code.

V8's compiler is lazy— it won't convert a function's code until that function is called.

### Pipeline
Immediately prior to execution, Javascript gets compiled to native machine code.
1. Right before execution, an [[general.lang.AST]] is generated from the Javascript with a parser.
2. Ignition (the interpreter) generates Bytecode from this AST.
    - bytecode is an abstraction on top of machine code. As a result, it uses the same computational model as the [[hardware.cpu]]
3. TurboFan (the optimizing compiler) takes the Bytecode and generates optimized machine code.
    - known as *just-in-time compilation*, and this is where Javascript engines get their speed edge from.

![](/assets/images/2022-04-20-14-57-08.png)


The V8 engine itself it written in C++

Since JavaScript is single-threaded V8 also uses a single process per JavaScript context
- therefore if you use service workers it will spawn a new V8 process per worker.

### Bytecode
V8 bytecodes are small building blocks that implement Javascript functionality when composed together.
- bytecode is about 25%-50% the size the average baseline machine code.

Each bytecode specifies its inputs and outputs as [[register|hardware.cpu.register]] operands.

Bytecode works at the level of abstraction where the code is determining how to store and retrieve values stored in memory register.

Bytecode uses 2 type of register:
1. accumulator register
  - the `return` keyword of a function basically says "get me the value in the accumulator register"
2. normal register (e.g. `r0`, `r1` ...)

We can see the bytecode generated with
```
node --print-bytecode myFile.js
```

#### Example syntax
- Load (`Ld`) small integer (`Smi`) 42 (`[42]`) into the accumulator (`a`) register
```
LdaSmi [42]
```

## Memory structure of V8 Engine
![](/assets/images/2022-04-19-14-57-02.png)

Resident Set - A running program has some allocated memory in the V8 process, called the *resident set*.

### Heap memory
This is where objects or dynamic data is stored

Only the *new space* and *old space* is managed by garbage collection.

The heap is divided into:
- New space
- Old space
- Large object space
- Code space
- Cell space, property cell space, and map space

#### New space
- New space is made of two parts, *to* and *from*.
- New space is where new objects live. 
- These objects are short-lived.
- objects stored here are quickly garbage collected
- The size of this space can be controlled using the V8 flags `--min_semi_space_size` (initial) and `--max_semi_space_size` (max).

#### Old space
- objects that survived two rounds of garbage collection are moved to the old space.
- Old space contains 2 parts:
  - old pointer space - Contains survived objects that have pointers to other objects.
  - old data space - Contains objects that just contain data (e.g. strings, numbers, arrays)
- The size of this space can be controlled using the V8 flags `--initial_old_space_size` (initial) and `--max_old_space_size` (max).

### Stack memory
there is one stack per V8 process

This is where static data including method/function frames, primitive values, and pointers to objects are stored.

The stack memory limit can be set using the `--stack_size` V8 flag.

The Stack is automatically managed and is done so by the operating system rather than V8 itself.

## References
https://deepu.tech/memory-management-in-v8/