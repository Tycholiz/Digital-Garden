
## Mutations
- While query fields are executed in parallel, mutation fields run in series, one after the other.
	- This means that if we send two `updateNugget` mutations in one request, the first is guaranteed to finish before the second begins
### Input types
often we will be using the same inputs to different mutations (ex. createNugget, updateNugget). Input types offers us a way to decalare these inputs once, and alias it as a type. 
