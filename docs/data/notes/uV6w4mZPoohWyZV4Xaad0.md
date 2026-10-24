
A deployment is a running instance of the app.

A deployment happens in 3 stages:
1. *build stage* - transform source code into an executable bundle known as a **build**.
2. *release stage* - takes the build and combines it with the deployment's current config (e.g. the Kubernetes config), producing something that is ready for immediate execution in an environment.
3. *run stage* (aka runtime) - runs the app in the execution environment by launching some of the app's processes against a selected release.
    - should be kept to as few moving parts as possible, since these moving parts will be the things that cause breakages in the middle of the night.

### Config
An app’s *config* is everything that is likely to vary between deploys (staging, production, developer environments, etc), including...
- backing services, such as databases and caches
- credentials to 3rd party services
- per deploy values like hostname (URL) of where the app will be deployed

Config varies substantially across deploys, while code does not.

* * *

### Blue-green deployments
A blue-green deployment is one without any downtime. In contrast to rolling updates, a blue-green deployment works by starting a cluster of replicas running the new version while all the old replicas are still serving all the live requests. Only when the new set of replicas is completely up and running is the load-balancer configuration changed to switch the load to the new version. A benefit of this approach is that there’s always only one version of the application running, reducing the complexity of handling multiple concurrent versions.

# UE Resources
[Kent C. Dodds Deployment pipeline breakdown](https://kentcdodds.com/blog/how-i-built-a-modern-website-in-2021)
