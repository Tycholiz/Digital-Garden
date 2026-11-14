
### Nested for-loops
Generally, they are a bad idea, but not always. They are perfect if they describe the correct algorithm e.g. if we need to access every value in a matrix

If we find ourselves having to loop over the same array multiple times, and the reason for doing so is because we need to compare some elements of the array with other elements, then we should think of using [[general.algorithms.patterns.sliding-window]]

#### Tips for avoiding
Does sorting the array first help you?
