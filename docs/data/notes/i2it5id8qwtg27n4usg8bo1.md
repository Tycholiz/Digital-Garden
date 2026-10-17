
## Data Primitives
PyTorch has two primitives to work with data: `torch.utils.data.DataLoader` and `torch.utils.data.Dataset`

### Dataset
`Dataset` represents a map between key (label) and sample (features) pairs of your data.
- ex. images and their associated labels

### DataLoader
`DataLoader` wraps an iterable around the `Dataset`
- this is accomplished by passing `Dataset` as an arg to `DataLoader`, giving us automatic batching, sampling, shuffling and multiprocess data loading.

## Domain-specific Libraries
- TorchText 
- TorchVision
- TorchAudio
- TorchRec - Recommendation Systems

### TorchVision
Every TorchVision Dataset includes two arguments: `transform` and `target_transform` to modify the samples and labels respectively.

* * *

## Reproducibility
Generating random numbers is an [[essential aspect|ml.deep-learning.nn#how-a-neural-network-learns]] to a deep learning model. However, this presents a problem: if we are generating random numbers, how can we reproduce the same results across different executions of the code and across different machines, given the same data and parameters?

There are a couple of things we can do to limit the amount of nondeterministic behaviour from Pytorch:
1. control sources of randomness that can cause multiple executions of your application to behave differently
2. configure PyTorch to avoid using nondeterministic algorithms for some operations. As a result, multiple calls to those operations, given the same inputs, will produce the same result.
    - this involves specifying the seed so that the [[PRNG|general.algorithms.RNG]] will produce the same sequence of "random" numbers, even run across different machines.

* * *

### Torch Hub / `torchvision.models`
Allows us to access many pre-built deep learning models (allowing us to leverage [[transfer learning|ml#transfer-learning]])

## Resources
- [Pytorch Cheatsheet](https://pytorch.org/tutorials/beginner/ptcheat.html)

## Learning Resources
- https://www.dataquest.io/blog/pytorch-for-beginners/
- [Learn PyTorch for deep learning in a day. Literally](https://youtu.be/Z_ikDlimN6A?si=FX6o8eF3Xh6fbqnA&t=23078)
    - [Accompanying notes](https://www.learnpytorch.io/)

