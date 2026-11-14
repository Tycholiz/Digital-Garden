
Probes allow us to do health checks on containers.

Kubernetes is designed to start up containers automatically when the original fails, but what happens if the container didn't fail, but is serving content 3x slower because of a memory leak? We need to be able to check the statuses of containers, and we can do that with Probes.

## Probe Types
### `livenessProbe`
Can be used to confirm whether an app running in a container is in a healthy state.
- If the probe fails, Kubernetes will kill the container and apply restart policy which defaults to Always.


The below probe will make a get request, and will kill the container if it fails.
```yaml
# podfile
...
livenessProbe:
	httpGet:
		path: /this/path/does/not/exist
		port: 8080
	initialDelaySeconds: 5
	timeoutSeconds: 2 # Defaults to 1
	periodSeconds: 5 # Defaults to 10
	failureThreshold: 1 # Defaults to 3
```

### `readinessProbe`
The readinessProbe should be used as an indication that the service is ready to serve requests.
- e.g. our app depends on Redis, so the success of the readinessProbe should therefore depend on Redis having been initialized properly.

When combined with [[Services|k8s.objects.service]] construct, only containers with the readinessProbe state set to Success will receive requests.

The lack of the retry mechanism in Kubernetes is mitigated with readinessProbe, which we added to the ReplicaSet.
The readinessProbe has the same fields as the livenessProbe

the readinessProbe is used by the iptables
