
We're able to pipe the output of one command into the input of another command precisely because these commands all implement the same interface: a file descriptor.

### Tips
You can end the pipeline at any point, pipe the output into less, and look at it to see if it has the expected form. This ability to inspect is great for debugging.

You can write the output of one pipeline stage to a file and use that file as input to the next stage. This allows you to restart the later stage without rerunning the entire pipeline.