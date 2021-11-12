
A CRD defines a new resource type, and tells Kubernetes about it
- Once a new resource type is added, new instances of that resource may be created.

Handling CRD changes is up to us as the developer, though a common pattern is to create a custom controller that watches for new CRD instances, and responds accordingly.
