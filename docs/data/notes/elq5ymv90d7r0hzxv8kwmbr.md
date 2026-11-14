
- `torch.nn.Module` - The base class for all neural network modules. Our neural network should subclass this module
- `torch.nn.Parameter` - Stores tensors that can be used with `nn.Module`. These are the parameters that our model should try and learn.
    - often, a layer from `torch.nn` will set these for us.
    - If `requires_grad=True`, gradients (used for updating model parameters via gradient descent) are calculated automatically, this is often referred to as "autograd".
- `def forward()` - this defines the computation that will take place on the data passed to the particular `nn.Module`
    - all `nn.Module` subclasses require us to override this method
- `torch.optim` - Contains various optimization algorithms (these tell the model parameters stored in `nn.Parameter` how to best change to improve gradient descent and in turn reduce the loss).

## Neural Network instance methods
- `model.state_dict()` - gives us the state of the model's parameters