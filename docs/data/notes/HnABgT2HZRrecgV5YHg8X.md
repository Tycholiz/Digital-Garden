
All of these commands send requests to the Kubernetes API

## Tips
- For any command, you can specify which namespace to use with the `-n` flag. 
	- ex. `kubectl get pod -n <namespace>`
	- By default, the `default` namespace will be used.
- Instead of passing the identifier of the actual Kubernetes object, we can pass the yaml config file with the `-f` flag
	- ex. `kubectl describe -f <config>.yml`
- run shell command in a pod - `kubectl exec -it -- <podname>`
	- open a bash shell in pod - `kubectl exec -it <podname> bash`
	- list processes - `kubectl exec <podname> -- ps aux`
	- list env variables - `kubectl exec <podname> env`
- pass `-c <containername>` to specify which container to run the process in.

## Read
#### Config
`kubectl config current-context`
- see where kubectl is pointing

`kubectl config view`
- show (redacted) contents of `~/.kube/config`

#### Describe
- give information on a resource (aka Kubernetes object)
`kubectl describe pod <podname>`

#### Get
`kubectl get pods`
- get all pods of a cluster
- pass `-o wide` or `-o yaml` for more info
- `--show-labels` to see the labels on each pod

`kubectl get namespaces`

`kubectl get all --all-namespace kubectl get all --all-namespacess`
- get all components that make up the cluster

`kubectl get nodes`
- get all nodes of a cluster

`kubectl get rs`
- get all ReplicaSets

`kubectl get ep`
- get endpoints of a cluster

`kubectl get -f pod/go-demo-2.yml -o jsonpath="{.spec.containers[*].name}"`
- format output as only including the `name` property, within `spec.containers`

#### Logs
- get logs from a pod
`kubectl logs <podname>`
- `-f` to follow the logs in real-time

#### Context
`kubectl config get-contexts`
- get clusters

`kubectl config use-context <CLUSTER-NAME>`
- switch cluster

## Create/Update
#### Apply
`kubectl apply -f rs/rs.yml`
- apply a configuration to a resource.
- we can create resources with this command, by `apply`ing to a resource that doesn't exist.
	- we can also `create` with `--save-config`

#### Create
`kubectl create -f pod/db.yml`
- create a pod based on the db.yml file
- note: we didn't need to specify that we were creating a pod, since the resource type is already defined in the yml file (`kind` field).
- `--record` allows us to track each change to our resources such as a Deployments.

## Destroy
#### Delete
`kubectl delete pod <podname>`
- delete a pod

`kubectl delete namespace <namespace>`
- delete a namespace

* * *

#### Expose
- expose a resource as a new Kubernetes service
	- the resource can be a Deployment, another Service, a ReplicaSet, a ReplicationController, or a Pod
ex
```sh
kubectl expose rs go-demo-2 \
	--name=go-demo-2-svc \
	# this port will be exposed on every node of the cluster to the outside world,
	# and it will be routed to one of the Pods controlled by the ReplicaSet
	--target-port=28017 \
	--type=NodePort # what type of service?
```

#### Label
- We can remove a label from a pod:
	- `kubectl label pod/pod.yml service-` will remove the service label (the `-` at the end is the syntax for removing labels)

#### Rollout
- allows us to manage the rollout of a resource
`kubectl rollout status -w -f deploy/go-demo-2-api.yml`

`kubectl rollout history -f deploy/go-demo-2-api.yml`
- This lets us see how many revisions of the software there have been, and which command created each revision.
`kubectl rollout undo -f deploy/go-demo-2-api.yml`
`kubectl rollout undo -f deploy/go-demo-2-api.yml --to-revision=2`

#### Set
- set configuration properties for resources
	- ex. update docker image of a pod template, update env variables of a pod template etc.
- note: the output of this command only indicates that the definition (ex. of the image used in the Deployment) was successfully updated. This means that if we used an image that doesn't exist, we would still get a "success" output from this command.