
- [Trunk-based development](https://trunkbaseddevelopment.com/)
    - ie. using short-lived branches, merging often into main and feature toggles

### The problem with long-lived feature branches
We need to merge main into the feature branch often. The problem with that is that you can have multiple long-lived feature branches that are pulling in main, but they're not syncing with each other.
- ex. if featureA is doing some refactoring of a module, and featureB is editing that same module, then it doesn't matter how frequently they pull in main, there's still going to be massive conflicts when one of the branches (whichever isn't first) tries to merge.