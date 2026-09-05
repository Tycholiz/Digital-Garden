
### Jupyter kernel
A Jupyter kernel is a programming language-specific engine that executes the code contained in a Jupyter notebook.

The kernel is what communicates between the Jupyter notebook and the Python interpreter.
- therefore, each kernel is tied to a specific python interpreter (e.g. the interpreter installed at `/usr/local/bin/python3.11`).

Within Python itself, you can have different kernels for different conda environments.

### Jupyter Server
If we have a PC with high performing GPU then we can run the Jupyter Server from that computer, and connect remotely to it from our work computer.