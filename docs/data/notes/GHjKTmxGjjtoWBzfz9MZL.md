
A Remote Procedure Call (RPC) is when we call a function that lives on a different machine, without the programmer explicitly coding the details for the remote interaction
- That is, the programmer writes essentially the same code whether the subroutine is local to the executing program, or remote
- caller is client, executor is server

The RPC model implies a level of location transparency, namely that calling procedures are largely the same whether they are local or remote, but usually they are not identical, so local calls can be distinguished from remote calls.

Remote calls are usually orders of magnitude slower and less reliable than local calls, so distinguishing them is important.

RPCs are a form of inter-process communication
- ie. different processes have different address spaces

# UE Resources
[node and Go tutorial](https://blog.logrocket.com/introduction-to-rpc-using-go-and-node/)
