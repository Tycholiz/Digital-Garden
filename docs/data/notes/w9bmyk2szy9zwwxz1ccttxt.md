
PodDisruptionBudget is a configuration that lets us limit the number of concurrent disruptions that our application experiences.
- the purpose is to increase availability.
- in other words it lets us decide "how many instances can be down at the same time for a short period due to a voluntary disruption?"
  - ex. if you have 3 pods and `minAvailable: 2`, then Kubernetes can only kill 1 pod

Most likely, we use PDB because we want to protect an application specified by a Kubernetes controller:
- Deployment
- ReplicationController
- ReplicaSet
- StatefulSet

### `minAvailable` and `maxUnavailable`
`minAvailable`
- a description of the number of pods from that set that must still be available after the eviction, even in the absence of the evicted pod
  - ex. if you set `minAvailable` to 10, then 10 Pods must always be available, even during a disruption. This means that evictions are allowed as long as they leave behind 10 or more healthy pods selected by the PodDisruptionBudget's `selector`

`maxUnavailable`
- a description of the number of pods from that set that can be unavailable after the eviction.
  - ex. With a `maxUnavailable` of 5, evictions are allowed as long as there are at most 5 unhealthy replicas among the total number of desired replicas.