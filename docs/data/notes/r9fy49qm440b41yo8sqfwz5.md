
### Label
A label is the thing we're predicting—the $y$ variable in simple linear regression. 
- ex. The label could be the future price of wheat, the kind of animal shown in a picture, the meaning of an audio clip, or just about anything.

### Feature
A feature is an input variable—the x variable in simple linear regression. A simple machine learning project might use a single feature, while a more sophisticated machine learning project could use millions of features, specified as:

$$
x_{1}, x_{2}...x_{N}
$$

- ex. In a spam detector, the features could include: words in the email text, sender's address, time of day the email was sent, email contains the phrase "one weird trick."

### Example
An example is a particular instance of $x$ (note: $x$ is a vector)
- each example is often represented by a vector of features.

Examples can be either unlabeled or labeled.
- A labeled example includes both feature(s) and the label
- An unlabeled example contains features but not the label.

### Model
A model defines the relationship between features and label. 
- ex. a spam detection model might associate certain features strongly with "spam". 

There are 2 principle phases of a model's life:
1. *Training*, which means creating or learning the model. That is, you show the model labeled examples and enable the model to gradually learn the relationships between features and label.
2. *Inference*, which means applying the trained model to unlabeled examples. That is, you use the trained model to make useful predictions (`y'`). 
    - ex. during inference, you can predict `medianHouseValue` for new unlabeled examples.

### Parameter
A model parameter is a configuration variable that is internal to the model and whose value can be estimated from data.
- that is, a parameter is a value that the model sets itself

In the context of [[neural networks|ml.deep-learning.nn]], `parameters` usually refer to `weights` and `biases` of the network.

### Hyperparameter
A hyperparameter is a value that we (as a developer) set.
- ex. `learning rate`

### Epoch
Each time a dataset passes through an algorithm, it is said to have completed an epoch
- therefore, one epoch is one entire passing of training data through the algorithm

An epoch is a hyperparameter that determines the process of training the machine learning model