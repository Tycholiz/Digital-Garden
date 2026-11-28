
### Testing for non-equality
When testing that 2 values are *not* equal to each other, we have to be careful with the logic. Note that checking for non-equality is not the opposite of checking for equality.

Consider that the follow equality test is strict. It will only pass the test if they are exactly the same:
```js
if (myVersion.title === yourVersion.title) 
```

On the other hand, when we test for non-equality, there are additional values that can cause our test to pass.
- ex. if myVersion is undefined, then we would be testing `(undefined !== yourVersion.title)`, which is `true`. However, this is probably not what we meant to test for. In the case where `myVersion` does not even exist, we'd probably want to exit and handle it differently. Otherwise, the assumption is made that we should handle it as if we were checking non-equality between 2 different version titles.
```js
if (myVersion?.title !== yourVersion.title) 

// to fix this, check truthiness of `myVersion` first
if (myVersion && myVersion?.title !== yourVersion.title) 
```


# UE Resources
[Getting rid of nested ifs](https://lawyerdev.medium.com/i-never-write-nested-ifs-e4e91a5440ee)
