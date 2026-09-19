
A HPA controls the scale of a [[k8s.objects.deployment]] and its [[k8s.controllers.replica-set]]
- The HPA will also instruct the workload resource to scale back down when load decreases.

A HPA automatically updates a workload resource (such as a [[k8s.objects.deployment]] or StatefulSet), with the aim of automatically scaling the workload to match demand.

[[Horizontal scaling|deploy.scaling]] here means that the strategy is to deploy more pods (scale out), rather than adding more computational resources like CPU and memory to existing pods.


### How it works
The HPA runs within the [[Control Plane (master node)|k8s.node.master]]

HPA is implemented as a control loop that runs intermittently (ie. it's not a continuous process).
- default is every 15s

Once each period, the controller manager queries for information about how resources are being utilized, and compares it against the metric specified in the HPA spec definition.
- the target resource is found by `scaleTargetRef`.
- it then selects the pods based on the target's resources `.spec.selector` labels and obtains the metrics.

Example:
```yml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: $APP_NAME
spec:
  scaleTargetRef:
    apiVersion: apps/v1beta1
    kind: Deployment
    name: $APP_NAME
  minReplicas: 2
  maxReplicas: 4
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 80
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 100
```
