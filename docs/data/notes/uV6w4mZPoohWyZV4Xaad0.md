
### Blue-green deployments
A blue-green deployment is one without any downtime. In contrast to rolling updates, a blue-green deployment works by starting a cluster of replicas running the new version while all the old replicas are still serving all the live requests. Only when the new set of replicas is completely up and running is the load-balancer configuration changed to switch the load to the new version. A benefit of this approach is that there’s always only one version of the application running, reducing the complexity of handling multiple concurrent versions.

# Trends
[GitOps](https://www.atlassian.com/git/tutorials/gitops)

# UE Resources
[Kent C. Dodds Deployment pipeline breakdown](https://kentcdodds.com/blog/how-i-built-a-modern-website-in-2021)
