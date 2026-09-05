
## Overview
A Deployment controls the deployment and running of a pod on your cluster.

Deployment objects manage the creation of pods for us. 
- If we run a [[k8s.objects.pod]] directly and it fails, Kubernetes will not automatically reschedule it.

Aside from rescheduling our pods when they die, Deployments can also:
- scale our applications by increasing or decreasing the number of replicas we have running
- handle the rollout of new versions of our application, so we can go from v1 to v2 without any downtime. 
- allow us to easily rollback bad releases, as well as preventing bad releases from going through altogether 

Kubernetes Deployments are all about: how do we swap out the old [[ReplicaSets|k8s.controllers.replica-set]] with the new ones, and not lose any downtime in the process? How do we gently replace them 1 by 1, without having to replace them all at once?
- ex. Imagine that we want to start using the Docker image `Postgres:12.2` instead of `Postgres:11.0`. The deploy.yml file specifies that we want to have 3 ReplicaSets. The implication of this, is that if we run `kubectl set image -f deploy.yml <containername>=postgres:12.2`, we would expect the Kubernetes engine to drop one `11.0` RS, and add a `12.2`, then drop another `11.0` and add a `12.2`, and so on until the rollout is complete and we only have `12.2`.
  - note: when we rollback (ie. undo a rollout), this process happens in reverse.

Imagine the systems of Kubernetes, Docker, and everything in between. If Docker were at the bottom of a physical chain, we could run containers ourselves with `docker run`. Moving up, we can create pods directly with `kubectl create pod

### The Problem
With just [[Pods|k8s.objects.pod]], [[ReplicaSets|k8s.controllers.replica-set]] and [[Services|k8s.objects.service]], we can deploy, scale and enable communication for our application. 
- However, this is all useless if we cannot update those applications with new releases. This is the problem that Deployments solve.

The desired state of our applications is changing all the time. The most common reasons for new states are new releases. The process is relatively simple. We make a change and commit it to a code repository. We build it, and we test it. Once we’re confident that it works as expected, we deploy it to a cluster.
- It does not matter whether that deployment is to a development, test, staging, or production environment. We need to deploy a new release to a cluster, even when that is a single-node Kubernetes running on a laptop. No matter how many environments we have, the process should always be the same or, at least, as similar as possible. We need to do all of this with zero-downtime. Deployments allow us to do that.

### What is it?
A deployment is a higher-level abstraction that controlling and scaling a set of pods
- Behind the scenes, it uses a ReplicaSet to keep the Pods running, but it offers sophisticated logic for deploying, updating, and scaling a set of Pods within a cluster.

A Deployment provides declarative updates for Pods and ReplicaSets.
- You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate

The service in front of the deployment has an IP address, but this address only exists within the Kubernetes cluster. This means the service isn’t available to the Internet at all.

### Zero-Downtime Deployment
Deployments support rolling updates and rollbacks

Similar to [[Graphile-Migrate's|graphile-migrate]] opinion about rolling forward only (ie. adding new migrations rather than removing them), we should follow this approach to deployments in Kubernetes (although the opinion is not as strong as Graphile-Migrate's).
- We definitely do not want to rollback (ie. `kubectl rollout undo`) in the situation where there is a database change, which would result in backend services that depend on a different version of the database.

New deployments do not destroy ReplicaSets, but rather scale them to 0. Therefore, to undo a rollout all we need to do is scale the bad ReplicaSets to 0 and scale the good ReplicaSets to our desired number.
- This fact is reflected when we run `kubectl rollout history`. When we rollback, what we will see is that the associated command with the new revision will be the initial command that was used to create it (eg. `kubectl create --filename=deploy/go-demo-2-api.yml`). It might be natural to think that the command would be `kubectl set image`, but this is not the case.

* * *

Just as we are not supposed to create Pods directly but using other controllers like ReplicaSet, we are not supposed to create ReplicaSets either. Kubernetes Deployments will create them for us.
- Therefore, if we naively observe creating Pods via the 2 procedures, there is no difference:
  - Deployment -> ReplicaSet -> Pods
  - ReplicaSet -> Pods

However, the advantage becomes evident if we try to change some of its aspects.
- ex. we might choose to upgrade MongoDB to version 3.4.

The Deployment Controller is what creates the objects that are listed in the `deploy.yml` file. It watches the API server for new events, and creates the objects (eg. ReplicaSet) in response.
- Once the ReplicaSet is created, the ReplicaSet Controller (which also watches for API events) notices that a new ReplicaSet was created, and in response, creates the Pods that it specified.
- From there, the scheduler takes over, which watches for unassigned pods and assigns them to nodes.
- Following that, Kubelet (which is watching for pods being assigned to nodes) causes the containers to be run (via Docker), then sends a signal to the API server indicating the updated status of the Pods.

Below, we tell the Deployment that it is to manage all the pods that have a label called `app` with the value `nginx`

Example object spec for a Kubernetes Deployment:
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```