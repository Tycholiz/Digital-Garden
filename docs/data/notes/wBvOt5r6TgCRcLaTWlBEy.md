
# Query & Mutation
- `useQuery` will execute on component mount, giving us back the loading, error and data state. Therefore, it is declarative 
- `useLazyQuery` will execute the query on command. This is perfect to use in events other than component mount, like on button click 
	- To be clear, you could put this in `useEffect` and it would operate similarly to `useQuery`, since `useEffect` gets run on component mount
- `useMutation` will return a function that we can execute to perform the mutation 

### Errors
we need to stringify errors:
`console.log('error', JSON.stringify(err, null, 2))`
spec: alternatively, we can use `apollo-link-error`

