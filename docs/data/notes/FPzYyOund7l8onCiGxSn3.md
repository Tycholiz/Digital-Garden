
A state machine is a directed graph where nodes represent all possible states of a view or of the whole app, and where edges represent possible transitions between the states.

State machines are particularly useful when the state is very complex and involves multiple transitions.

Turnstile:
![](/assets/images/2021-10-27-09-53-05.png)

An arrow between two nodes means that it’s possible to go through one state to another via some action. All non-listed transitions are not possible. There can be a meaningful transition from a state to the same state that is marked by a circular arrow.

Here is a state machine for the storage of fetched data that is to be stored in the browser
![](/assets/images/2021-10-27-09-56-33.png)

There are four states:
- empty - fetching has not started yet or has been canceled
- loading - fetching is in progress
- withData - fetching was successful
- error - fetching failed
