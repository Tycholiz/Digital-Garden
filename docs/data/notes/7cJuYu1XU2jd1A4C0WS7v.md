
When using a service, requests to one of the exposed ports will be picked up by Kube DNS and forwarded to one of the pods.
- ex. as a user, I can access the exposed port (ie. **27017**:34172) through any node of the cluster. Kube DNS is the component that picks up those requests and forwards them on.
