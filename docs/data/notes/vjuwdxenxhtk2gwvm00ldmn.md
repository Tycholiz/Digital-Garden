
`requests` and `limits` are the mechanisms Kubernetes uses to control resources such as CPU and memory. 
- `requests` are what the container is guaranteed to get. If a container requests a resource, Kubernetes will only schedule it on a node that can give it that resource. 
- `limits`, on the other hand, make sure a container never goes above a specific value.
  - as a general best practice, never set CPU limit
  - also as a general best practice, always set memory limit == request

The most common resources to specify are *memory* and *CPU*, which are collectively referred to as *compute resources*
- Memory resources are considered non-compressible resources– if a container exceeds its limit, it will get terminated.
- CPU resources are considered compressible resources– if a container exceeds its limit, it will get throttled. This means the CPU will be artificially restricted, giving your app potentially worse performance.

#### Example config:
```yml
resources:
  limits:
    memory: "400Mi"
    cpu: "1"
  requests:
    memory: "200Mi"
    cpu: "500m"
```

### Custom Resource Definition (CRD)
CRD is somewhat of an alternative to ConfigMap: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/

CRD is an extension mechanism that enables users to define custom resources and their behavior in a Kubernetes cluster. 

With CRDs, we can introduce and manage our own custom resources, which may not be part of the core Kubernetes API.

* * *

### CPU
CPU resources are defined in millicores or “thousandth of a core.” 
- ex. In the above example, the container process needs 500/1000 (50%) of a core and is allowed to use at most 1000/1000 (100%) of a core.

By default, the kubelet uses [CFS](https://en.wikipedia.org/wiki/Completely_Fair_Scheduler) quota to enforce pod CPU limits, and uses two configuration options:
1. `cpu_period_us` - (100ms default) The CPU interval the scheduler uses to reset the used quota for a process.
2. `cpu_quota_us` - The runtime for which the process can use the CPU in a given period before being reset
  - If your application is multi-threaded, then `cpu_quota_us` is calculated across all the threads.
    - ex. in the case of the NodeJS application, which runs 4 threads (libuv) by default for system tasks like networking, the quota per thread will be 25ms (=100/4). So, in a given `cpu_period_us` (100ms), if your application spends more than 25ms processing, it will be throttled for 75ms of that `cpu_period_us` cycle (see below)
  
![](/assets/images/2022-08-25-09-18-22.png)

If a request takes 100ms to complete, in the above scenario, it will take at least 3 times cpu_period_us to complete, which will end up being ~325ms. This happens because, during the throttled period, the application is essentially "paused" even though CPU capacity might be available.

## UE Resources
- https://home.robusta.dev/blog/stop-using-cpu-limits/
- https://www.netdata.cloud/blog/kubernetes-throttling-doesnt-have-to-suck-let-us-help/