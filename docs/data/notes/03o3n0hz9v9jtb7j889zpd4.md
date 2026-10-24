
Machine learning is the ability for computers to learn something without explicitly being programmed to know that thing.
- it is done by turning data into numbers (which are then stored in [[tensors|math.algebra.linear.tensors]]), and then passing it into a neural network (which tries to find patterns in those numbers)
    - in fact, you can use machine learning for pretty much anything as long as you can convert the inputs into numbers and write the machine learning algorithm that will allow the machine to find the patterns.
- The whole premise of machine learning is to learn a representation of the input and how it maps to the output
    - ex. imagine we had an input $X$ and an ouput $Y$. We are curious to see how these 2 variables relate to each other. Using machine learning, we could reveal that a simple regression formula ($Y = a + bX$) lies beneath the relationship. In this case, machine learning is about discovering the nature of that relationship with only the input and the output.

Whereas traditional programming is focused on defining inputs and rules in order to arrive at an output, machine learning is focused on defining inputs and an output, and figuring out the rules necessary to arrive at that output
- ex. we are building a program to cook a roast chicken. The traditional programming paradigm would have us defining the ingredients (ie. inputs) and defining the cooking steps (ie. rules), which yields the cooked meal (ie. output). In machine learning, we define the ingredients, define the output, and the machine learning algorithm will figure out how to make the meal itself.

![[hardware.GPU#use-in-machine-learning]]

# Machine Learning Paradigms
There are three basic machine learning paradigms:
- Supervised Learning
- Unsupervised Learning
- Reinforcement Learning

![](/assets/images/2023-07-08-21-35-07.png)

## Supervised Learning
Supervised learning is using labeled datasets to train algoritms to classify data or predict outcomes. It does this by iteratively making predictions on the data and adjusting for the correct answer.
- "labeled" means that the rows in the dataset are tagged or classified in some interesting way that tells us something interesting about that data
    - ex. "is this a picture of a t-shirt?", "does the picture of the plant have mites on the leaves?"

The goal of any supervised learning algorithm is to find a function that best maps a set of inputs to their correct output.
- it is the job of [[backpropagation|ml.deep-learning.nn.functions#backpropagation]] to train a neural network to learn the appropriate internal representation of how an input relates to an output.

In supervised learning, the goal is to predict outcomes for new data. You know up front the type of results to expect.

Supervised learning models tend to be more accurate, but also require much more human intervention
- ex. a supervised learning model can predict how long your commute will be based on the time of day, weather conditions and so on. But first, you’ll have to train it to know that rainy weather extends the driving time.

### Classification Model
This method involves us recognizing and grouping ideas/objects into predefined categories
- ex. customer retention - we can make a model that will help us identify customer that are about to churn, allowing us to take action to retain them. We can do this by analyzing their activity
- ex. classify spam in a separate folder from your inbox.
- ex. sentiment analysis to determine the sentiment or emotional polarity of a piece of text, such as a review or a social media post. 

Classification models are more suitable when the task involves assigning discrete labels
- ex. Is a given email message spam or not spam?
- ex. Is this an image of a dog, a cat, or a hamster?

Common types of classification algorithms are:
- linear classifiers
- support vector machines
- decision trees
- random forest

### Regression Model
This method involves us building an equation using various input values with their specific weights, determined by their overall value of the impact on their outcome. In this way, it helps us understand the relationship between dependent and independent variables
- ex. airlines use these models to determine how much they should charge for a particular flights, using various input factors such as days before departure, day of week, destination etc.
- ex. Weather forecasting - well-suited for regression, since they can estimate a numerical value based on the input features. 

Regression models are helpful for predicting numerical values based on different data points, such as sales revenue projections for a given business.

Regression models are more suitable when the output is a continuous value
- ex. What is the value of a house in California?
- ex. What is the probability that a user will click on this ad?

Popular regression algorithms are: 
- linear regression
- logistic regression
- polynomial regression

## Unsupervised Learning
Using machine learning algorithms to analyze and cluster unlabelled datasets, allowing us to discover hidden patterns/groupings without the need for human intervention.

With an unsupervised learning algorithm, the goal is to get insights from large volumes of new data. The machine learning itself determines what is different or interesting from the dataset.

Unsupervised models still require some human intervention for validating output variables. 
- ex. an unsupervised learning model can identify that online shoppers often purchase groups of products at the same time. However, a data analyst would need to validate that it makes sense for a recommendation engine to group baby clothes with an order of diapers, applesauce and sippy cups.

Unsupervised learning models are used for three main tasks: 
- clustering
- association
- dimensionality reduction

### Clustering
- ex. customer segmentation - it is not always clear how individual customers are similar or different from one another. Clustering algorithms can take into account a variety of information on the customer, such as their purchase history, social media activity, geography, demographic etc., with the goal being to segment similar customers into separate buckets so the company be more targetted with their efforts

### Association
- ex. recommendation engines

### Dimensionality Reduction
Techniques that reduce the number of input variables in a dataset so we don't let redundant parameters overrepresent the impact on the outcome.

## Reinforcement Learning
Reinforcement Learning is semi-supervised learning where we typically have an agent take actions in an environment. The environment will then reward the agent for correct moves, or punish it for incorrect moves
- Through many iterations of this, we can teach a system a particular task
- ex. with self-driving cars

## Transfer Learning
Take a pattern that one model has learned from a certain dataset, and apply those learnings to a different model to give us a head start.
- More specifically, it's about leveraging the knowledge learned from one task to aid the learning process in another related task. This can be done within the same model or across models.
- ex. for image classification, knowledge gained while learning to recognize cars could be applied when trying to recognize trucks.

# Machine Learning Approaches
### Seq2Seq (Sequence to Sequence)
Here, we put one sequence into the model, and get one out
- ex. with Google Translate, we put in our sequence of source language text and get our target language out
- ex. with speech recognition, we put in our sequence of audio waves and get some text out

### Classification/regression
Classification is about predicting if something is one thing or another (e.g. if an email is spam or not)
- Computer Vision
    - ex. recognizing a truck within a security camera picture. 
        - In this case, the regression predicts where the corners of the box should be (predicting a number is what regression does), and the classification part would be the machine recognizing whether or not the particular vehicle was the one that did the hit and run.
        ![](/assets/images/2023-07-09-08-40-22.png)
- Natural Language Processing

* * *

## Datasets
The idea is that we want to split up our data and assign different portions to different sets.
- the main reason to split datasets into Training, Validation, and Test sets is to ensure that we don't overfit our model to the data it was trained on.

### Training Set
The training set is the data that the model learns from

This usually encompasses around 60-80% of our data

### Validation Set
The validation set is the data that the model gets tuned to

Validation sets are used often, but are not required like training sets and test sets are.

The validation set helps tune hyperparameters and choose the best version of the model

This usually encompasses around 10-20% of our data

### Test Set
The test set is the data that the model gets evaluated on to test what it has learned (ie. it is the final evaluation)

The test set should always be kept separate from all other data, since we want our model to learn on training data and then evaluate it on test data to get an indication of how well it generalizes to unseen examples.

This usually encompasses around 10-20% of our data

## UE Resources
- [Why are machine learning operations run on GPUs?](https://timdettmers.com/2023/01/30/which-gpu-for-deep-learning/)
