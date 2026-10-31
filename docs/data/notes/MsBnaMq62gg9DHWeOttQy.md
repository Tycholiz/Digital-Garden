
A controller is a [control loop](https://kubernetes.io/docs/concepts/architecture/controller/) that watches the (shared) state of the cluster through the [[API server|k8s.node.master.components.api-server]], and makes changes so that the actual state matches the desired state.

A controller watches the API server for new events. When it detects that there is a new object (eg. a new ReplicaSet), it acts in accordance with the type of controller it is.
- ex. In the case of a ReplicaSet, the controller would create pods equal to the number found in the replica-set yaml file

Controllers themselves are stateless.
