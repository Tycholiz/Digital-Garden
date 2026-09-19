
Labels are key/value pairs that are attached to objects, such as pods
- Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system
- Labels can be used to organize and to select subsets of objects.

Labels allow for efficient queries and watches and are ideal for use in UIs and CLIs

Labels can be seen with `kubectl describe pod`

Labels enable users to map their own organizational structures onto system objects in a loosely coupled fashion, without requiring clients to store these mappings.

```
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

It is throught Labels that Pods and ReplicaSets are able to be loosely coupled.

Labels are what a ReplicaSet uses to identify the pods that are in existence that satisfy the RepicaSet's requirement for pod count.
- ex. we have a ReplicaSet that specifies 4 pods should exist. If we remove the `service` label from a pod, then ReplicaSet will no longer be aware of it, and will create a brand new pod in order to reach its quota again. At the end of the day, we will have one extra pod (5 total) in the cluster. Of course, this pod with the removed `service` label will be running freely and won't be controlled by the ReplicaSet, since the labels no longer match.
  - Following this exercise, if we then add back the label that we removed, the ReplicaSet will be made aware of it, and will remove one of the other pods to reach its desired state of 4 pods.
