
## What is it?
Deep learning is a subset of machine learning that uses artificial neural networks to mimic the learning process of the human brain
- [[Neural networks|ml.deep-learning.nn]] make up the backbone of deep learning algorithms

The "deep" in deep learning is referring to the depth of layers in a neural network. A neural network that has at least 1 hidden layer can be considered a deep learning algorithm. This is generally represented using the following diagram:
![Deep neural network](/assets/images/2023-07-07-21-58-46.png)
- Classical, or "non-deep", machine learning is more dependent on human intervention to learn.

Deep learning is thought of as "scalable machine learning", since it automates a lot of the feature extraction process, and eliminates some need for human intervention whcih enables use for large datasets.

"Deep" machine learning can leverage labeled datasets (ie. [[supervised learning|ml#supervised-learning]]) to inform its algorithm, but it doesn’t necessarily require a labeled dataset.
- It can ingest unstructured data in its raw form (e.g. text, images), and it can automatically determine the set of features which distinguish "pizza", "burger", and "taco" from one another.

By observing patterns in the data, a deep learning model can cluster inputs appropriately.
- we could group pictures of pizzas, burgers, and tacos into their respective categories based on the similarities or differences identified in the images.
- however, a deep learning model would require more data points to improve its accuracy, whereas a machine learning model relies on less data given the underlying data structure.

Deep learning is primarily leveraged for more complex use cases, like virtual assistants or fraud detection.

Most deep neural networks are feed-forward, meaning they flow in one direction only from input to output. 
- you can also train your model through *backpropogation*, meaning it moves in opposite direction from output to input.
    - this allows us to calculate and attribute the error associated with each neuron, allowing us to adjust and fit the algorithm appropriately.

## Why use Deep learning?
We use deep learning when we have a long set of rules that the program needs to adhere to
- ex. a program to cook a roast chicken is easy since the ruleset is small. a program to enable cars to drive autonomously is complicated, because the rules involved are enormous
- We want to discover insights within a large set of data

Deep learning is a bit of a chainsaw solution to a problem. Sometimes all we need is a pair of scissors to solve in the problem, in which case deep learning is overkill. Traditional methods are lighter, easier to train, and require less training data.
- ex. XGBoost is still the state of the art for tabular data because those datasets are typically too small for deep learning.

### Why not use it?
- machine learning doesn't offer us much in the way of explanation for the patterns that it finds
- machine learning isn't deterministic— the results are based on probability.

## Resources
- [Hugging Face](https://huggingface.co/learn/nlp-course/chapter1/1)
- [Practical Deep Learning for Coders](https://course.fast.ai/)
- [Dive into Deep Learning](https://d2l.ai/)
- [Neural Networks and Deep Learning online book](http://neuralnetworksanddeeplearning.com/)