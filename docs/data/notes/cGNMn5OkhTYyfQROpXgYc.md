
## What is it?
- Streams are used to read or write input into output sequentially
- What makes streams unique, is that instead of a program reading a file into memory all at once like in the traditional way, streams read chunks of data piece by piece, processing its content without keeping it all in memory.
	- This makes streams really powerful when working with large amounts of data, for example, a file size can be larger than your free memory space, making it impossible to read the whole file into the memory in order to process it. That’s where streams come to the rescue!
- Streams are a way to handle reading/writing files, network communications, or any kind of end-to-end information exchange in an efficient way
- streams are not only about working with media (ie. streaming movies/music) or big data. They also give us the power of ‘composability’ in our code
	- it’s possible to compose powerful pieces of code by piping data to and from other smaller pieces of code, using streams.

it is best to think of a Stream as an established connection to some input, some output, or both. A lot of things can muddy this definition of stream, but that is the fundamental idea... A connection to some input or some output or some I/O endpoint that can handle both input and output.
- ex. one might have a readable stream to a file that one wants to get the contents of. In this case the stream is an input connection to the file. The input stream doesn't hold the contents of the file, but rather holds the connection to the file. It is up to the program to then read and store the contents (assuming that is the desired operation).
	- One place a stream might store the content is in a [[Buffer|memory.buffer]] object (if we were using Node).

## Why use streams over other data handling methods?
1. Memory efficiency: you don’t need to load large amounts of data in memory before you are able to process it
2. Time efficiency: it takes significantly less time to start processing data as soon as you have it, rather than having to wait with processing until the entire payload has been transmitted

## Standard Streams
- communication channels between a program and its environment (ie. the host that executes it).
- stdin and stdout are inherited from the parent process, though it can be redirected.
- anal: The Services menu, as implemented on Mac OSX, is analogous to standard streams. On this operating systems, graphical applications can provide functionality through a systemwide menu that operates on the current selection in the GUI, no matter in what application.
- The correct way to think about redirection is that one redirects output to a file (`>`), and one redirects a file's contents to stdout (`<`).

### File Descriptor (FD)
- A FD is a number that uniquely identifies an open file in a computer's operating system
	- A file can be thought of simply as a resource. In other words, something that provides data. A file as we know it fulfills this definiion, but also consider that streams do as well. Therefore, a file in this sense is more general, and simply stands for "a resource that provides data".
- A FD gives us the means to access a file, or perhaps more interestingly, a standard stream
	- therefore an FD is a reference to the standard stream
- ex. The file descriptor for standard input has id = 0
- Part of what makes Unix so well designed is that the common interface between so many commands is a simple file descriptor. If we expect the output of one program to become the input to another program, then those programs must use the same data format (to get a compatible interface). In Unix, that interface is a file descriptor, which is just a sequence of bytes.
	- ex. `awk`, `sort`, `uniq`, and `head` all treat their input file as a list of records separated by `\n`
- Because the file descriptor is such a simple interface, so many different things can be represented with that interface.
	- an actual file on the filesystem
	- a communication channel to another process (Unix socket, stdin, stdout)
	- a device driver (e.g. `/dev/audio`, `/dev/lp0`)
	- a socket representing a TCP connection

A file descriptor is associated with an input/output resource, which would include regular files, network sockets, and pipes
<!-- - except for redirections with `<&`, any time we do a redirection, -->

### STDIN
Standard input is a stream from which a program reads its input data.
- the program requests this data transfer by using the *read* operation
- ex. the keyboard is the stdin to a text editor, and also to an interactive shell.

When we redirect with `<`, implicitly we are running `1<`

### STDOUT
Standard output is a stream to which a program writes its output data.
- the program requests this data transfer by using the *write* operation

### STDERR
- having 2 different output streams is analogous to functions that return a pair of values
- The terminal that executes the program is the default stderr destination.
	- sometimes stdout is redirected, so the fact that stdout and stderr are different streams allows stderr to continue outputting its streams elsewhere than stdout.
		- ex. when we pipe the output of one program into another. Imagine we were piping data between different hosts. If there was a single stream, it would get passed along the chain. Now with stderr, we are able to see stderr in the executing terminal and trace down the location of the logs to find the offending program.

#### 2>&1
- we can append `2>&1` on any unix command to redirect stderr to the same destination as stdout
	- often, we see `2>/dev/null`, which means "redirect stderr to `/dev/null`, a blackhole"
	- ex. the **find** utility displays both stdout and stderr to the terminal. If we append the command with blackhole redirection, then only the stdout will be shown.
- placement of `2>&1` is critical to determining its meaning. Consider the following variants of the same command:

- `|&` is shorthand for `2>&1 |`
- `echo hello > stuff.txt` === `echo hello 1> stuff.txt`.
	- ie. the `1` (stdout) is implicit. we are saying "redirect the stdout of the echo command to `stuff.txt`"
- `0` means stdin, `1` means stdout, `2` means stderr
- in the context of redirections, `&` means "the following is a file descriptor (ie. the type of standard stream) and not a filename". Without `&`, we would be writing to a file named `1`
```
// stderr goes to terminal, and nothing gets written to outfile
command_does_not_exist 2>&1 > outfile

// stderr goes to stdout, and gets written into outfile
command_does_not_exist > outfile 2>&1
```
In the first example, when we encountered `2>&1`, we were saying "pipe all stderr into the same stream as stdout". At this point in the command, stdout was simply the terminal (spec: since that is the default stdout of a command). Since stdout was the terminal at this point, we also declared that stderr should point to the terminal. In this way, `2>&1` doesn't cause stderr to equal stdout. It merely points it the same way stdout is facing. Nothing is stopping stdout from changing direction later on, which is exactly what happens. When we write `> outfile`, we are declaring that stdout should be `outfile`.

In the second example, we see that by the time we evaluate `2>&1`, stdout is `outfile`. This causes stderr to also point at `outfile`, hence is why in this example, the file is populated with `command not found: command_does_not_exist`
- the `2>&1` operator points file descriptor 2 to the same target file descriptor 1 is pointing at.

## Special File
- spec: In Unix, the most comfortable means for programs to communicate is via files. It is easy to set up standard streams with files, so it is an appropriate level of abstraction to communicate with the Unix system. For this reason, Unix has this concept of a *special file*, which is a file that represents something that is not a file at all.
- ex. consider a partition of a hard drive that "exists" in the FS at `/dev/sda3`. In reality, this is just how the filesystem "knows of" the partition. The `sda3` file enables the partition to communicate with the Unix system via standard streams.
