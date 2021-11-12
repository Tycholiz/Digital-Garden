A ReplicaSet ensures that a set of identically configured Pods are running at the desired replica count. If a Pod drops off, the ReplicaSet brings a new one online as a replacement.

ReplicaSets are considered a low-level type in Kubernetes

- Higher level abstractions can be used instead, like [[deployments|k8s.objects.deployment]] and [[daemon sets|kubernetes.daemon-set]]

If we run pods without a controller, than a failing pod will remain dead (and will not be restarted by Kubernetes). Pods run in this manner have no fault-tolerance.

- ReplicaSet serves as a self-healing mechanism
- Pods associated with a ReplicaSet are guaranteed to run. They provide fault-tolerance and high availability.

A ReplicaSet operates at the cluster level, and therefore has control over all pods in the cluster

ReplicaSets are rarely used independently. You will almost never create a ReplicaSet directly just as you’re not going to create Pods.

- Instead, we tend to create ReplicaSets through [[Deployments|k8s.objects.deployment]]. In other words, we use ReplicaSets to create and control Pods, and Deployments to create ReplicaSets (and a few other things).

ReplicaSet `MatchLabels` are used to identify pods and must be the same as the pod labels

Since ReplicaSets and Pods are loosely coupled objects with matching labels, we can remove one without deleting the other.

- We can, however, pass `--cascade=true` if we want to delete pods that are associated with a RS

ReplicaSet uses labels to decide whether the desired number of Pods is already running in the cluster

- this fact means that if we are to delete a RS and then re-create it, it will use the same pods that were running in the cluster. This shows how a RS knows what the pods it defines look like, and if there is a pod (or pods) already in existence that satisfy that definition, then it will simply consider them as part of its pod # count.

## Process of Creating a ReplicaSet

1. Kubernetes client (kubectl) sent a request to the API server requesting the creation of a ReplicaSet defined in the rs yaml file.
2. The controller is watching the API server for new events, and it detected that there is a new ReplicaSet object.
3. The controller creates two new pod definitions because we have configured replica value as 2 in the rs yaml file.
4. Since the scheduler is watching the API server for new events, it detected that there are two unassigned Pods.
5. The scheduler decided which node to assign the Pod and sent that information to the API server.
6. Kubelet is also watching the API server. It detected that the two Pods were assigned to the node it is running on.
7. Kubelet sent requests to Docker requesting the creation of the containers that form the Pod. In our case, the Pod defines two containers based on the mongo and api image. So in total four containers are created.
8. Finally, Kubelet sent a request to the API server notifying it that the Pods were created successfully.

![](/assets/images/2021-05-30-18-19-36.png)

