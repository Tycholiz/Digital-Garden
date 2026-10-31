
An RPC is when we call a function that lives on a different machine, without the programmer explicitly coding the details for the remote interaction
- That is, the programmer writes essentially the same code whether the subroutine is local to the executing program, or remote
- caller is client, executor is server

The RPC model implies a level of location transparency, namely that calling procedures are largely the same whether they are local or remote, but usually they are not identical, so local calls can be distinguished from remote calls.

Although RPC seems convenient at first, the approach is fundamentally flawed, stemming from the fact that a network request is very different from a local function call:
- A local function call is predictable and either succeeds or fails, depending only on parameters that are under your control. A network request is unpredictable, and is mostly out of our control.
- A local function call either returns a result, or throws an exception, or never returns, while a network call may return without a result due to a *timeout*
- When you call a local function, you can efficiently pass it references (pointers) to objects in local memory. When you make a network request, all those parameters need to be encoded into a sequence of bytes that can be sent over the network. That’s okay if the parameters are primitives like numbers or strings, but quickly becomes problematic with larger objects.

All of these factors mean that there’s no point trying to make a remote service look too much like a local object in your programming language, because it’s a fundamentally different thing. 
- Part of the appeal of REST is that it doesn’t try to hide the fact that it’s a network protocol

Remote calls are usually orders of magnitude slower and less reliable than local calls, so distinguishing them is important.

Using RPCs involve tight coupling and difficulty to debug

RPCs are a form of inter-process communication
- ie. different processes have different address spaces

# UE Resources
[node and Go tutorial](https://blog.logrocket.com/introduction-to-rpc-using-go-and-node/)

