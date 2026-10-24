
In most cases, Kubernetes has sensible defaults, but there are things we will generally want to tweak

Multiple resources can be grouped together in a single manifest, separated by a line with three dashes (`---`). 
- It’s a common pattern to have Service and Deployment together.

### Strategy
#### RollingUpdate (default)
- creates a new ReplicaSet with zero replicas and, depending on other parameters, increases the replicas of the new one, and decreases those from the old one. This process is done one at a time, and as a new ReplicaSet is added, and old one is removed.
- The process is finished when the replicas of the new ReplicaSet entirely replace those from the old one.

Can be fine-tuned with the `maxSurge` and `maxUnavailable` fields.
- `maxSurge` defines the maximum number of Pods that can exceed the desired number (set using replicas). It can be set to an absolute number (e.g., 2) or a percentage (e.g., 35%). The total number of Pods will never exceed the desired number (set using replicas) and the maxSurge combined. The default value is 25%.
- `maxUnavailable` defines the maximum number of Pods that are not operational. If, for example, the number of replicas is set to 15 and this field is set to 4, the minimum number of Pods that would run at any given moment would be 11. Just as the maxSurge field, this one also defaults to 25%. If this field is not specified, there will always be at least 75% of the desired Pods.

#### Recreate
- this will kill all the existing Pods before an update
- this resembles the processes we used in the past when the typical strategy for deploying a new release was first to stop the existing one and then put a new one in its place.
  - This approach inevitably leads to downtime
- The only case when this strategy is useful is when applications are not designed for two releases to coexist.
- ask yourself the following question: Would there be an adverse effect if two different versions of my application are running in parallel? If yes, then Recreate might be a good option
- ex. Since most databases cannot have multiple instances writing to the same data files, killing the old release before creating a new one is a good strategy when replication is absent. This strategy is more suitable for singe-replica databases

### Selector
Used to select which pods should be included in the ReplicaSet. It does not distinguish between the Pods created by a ReplicaSet or some other process. In other words, ReplicaSets and Pods are decoupled.

- If Pods that match the selector exist, ReplicaSet will do nothing. If they don’t, it will create as many Pods to match the value of the replicas field.
- Not only that ReplicaSet creates the Pods that are missing, but it also monitors the cluster and ensures that the desired number of replicas are (almost) always running. In case there are already more running Pods with the matching selector, some will be terminated to match the number set in replicas.
