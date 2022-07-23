
### What is it?
Kustomize is a tool that allows us to manage K8S objects via a [[YML]] configuration file.

It allows you to create an entire Kubernetes application out of individual pieces — without touching the YAML configuration files for the individual components.

### What does it do?
It allows us to...
- generate resources from other sources
- set cross-cutting fields for resources
- compose and customizing collections of resources

It does this via layering to preserve the base settings of your applications and components by overlaying declarative yaml artifacts (called *overlays* or *patches*) that selectively override default settings without actually changing the original files.
- this approach is powerful because it allows us to combine common off-the-shelf applications ([COTS](https://kubectl.docs.kubernetes.io/references/kustomize/glossary/#off-the-shelf-configuration)) with ones that are internally created ([bespoke](https://kubectl.docs.kubernetes.io/references/kustomize/glossary/#bespoke-configuration)).

### Why use it?
Benefits of Using Kustomize:
1. Reusability
    - Kustomize allows you to reuse one base file across all of your environments (development, staging, production) and then overlay unique specifications for each.

2. Fast Generation
    - Since Kustomize has no templating language, you can use standard YAML to quickly declare your configurations.

3. Easier to Debug
    - YAML itself is easy to understand and debug when things go wrong. Pair that with the fact that your configurations are isolated in patches, and you’ll be able to triangulate the root cause of performance issues in no time. Simply compare performance to your base configuration and any other variations that are running.

Another benefit of utilizing patches is that they add dimensionality to your configuration settings, which can be isolated for troubleshooting misconfigurations or layered to create a framework of most-broad to most-specific configuration specifications.

Imagine we found a third-party [[chart|k8s.tools.helm#chart]] that we want to use as a basis for our cluster. It is 98% what we want, but requires some slight modifications to fit our use-case, so we fork the Chart, make the modifications, and apply it to our cluster. Some time later a new version of the Chart is released, so we have to fork it again and apply the same modifications.
- this approach is of course untenable, so we use Kustomize to have our custom patches overlayed on top of the base Chart.
- In our [[CI/CD|deploy.CI-CD]] pipeline, we can imagine that we are fetching the yaml files (the Chart) from [[helm|k8s.tools.helm]], and then our Kustomize yaml files are being patched over. These patches can be environment-specific values based on the events.
    - ex. if branch is `master` and we are deploying to production, then Kustomize will apply the yaml files in `k8s/overlays/production`.

### Bases and Overlays 
#### Bases directory
- A base is a directory with a `kustomization.yaml`, which contains a set of resources and associated customization. 
- A base could be either a local directory or a directory from a remote repo, as long as a `kustomization.yaml` is present inside. 
- A base has no knowledge of an overlay and can be used in multiple overlays. 
- The `kustmization.yaml` file is the most important file in the base folder and it describes what resources you use.

#### Overlay directory
- The overlays folder houses environment-specific overlays. It has 3 sub-folders (one for each environment).
    - ex. imagine we want to use different [[Service types|k8s.objects.service.types]] for each environment: ClusterIP for dev, NodePort for staging, and LoadBalance for production.
- An overlay is a directory with a `kustomization.yaml` that refers to other kustomization directories as its bases. 
- An overlay may have multiple bases and it composes all resources from bases and may also have customization on top of them.

#### Example
This is an example of a Kustomize configuration.
- each environment has different HPA (Horizontal Pod Autoscaling) settings 

```
├── base
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── kustomization.yaml
│   └── service.yaml
└── overlays
    ├── dev
    │   ├── hpa.yaml
    │   └── kustomization.yaml
    ├── production
    │   ├── hpa.yaml
    │   ├── kustomization.yaml
    │   ├── rollout-replica.yaml
    │   └── service-loadbalancer.yaml
    └── staging
        ├── hpa.yaml
        ├── kustomization.yaml
        └── service-nodeport.yaml
```


# Resources
- [Hello world example](https://www.mirantis.com/blog/introduction-to-kustomize-part-1-creating-a-kubernetes-app-out-of-multiple-pieces/)
https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/
