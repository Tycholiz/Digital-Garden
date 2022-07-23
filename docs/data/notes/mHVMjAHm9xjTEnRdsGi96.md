
A layer is a `.zip` file archive that can contain additional code or data.
- it may contain libraries (e.g. node modules), a custom runtime, data, or configuration files.
- think of it in much same way as how a [[docker image|docker.image]] is built using layers, or how [[Kustomize|k8s.tools.kustomize]] is used to reduce redunant code between configurations. The rationale is the same.

A key benefit of Lambda layers is that the layers stay warm (ie. initialized, eliminating the need for a cold start)
- the potential for this as a performance enhancement is huge, because if our Lambda uses many third-party dependencies, they can be aggregated into a layer (or many layers) and they won't have to be downloaded on cold starts.

Lambda layers provide a convenient way to package libraries and other dependencies that you can use with your Lambda functions.

Using layers reduces the size of uploaded deployment archives, making it faster to deploy the code.

Layers also promote code sharing and separation of responsibilities

### Using Layers
When you create a layer, you must bundle all its content into a .zip file archive. You upload the .zip file archive to your layer from Amazon Simple Storage Service (Amazon S3) or your local machine. Lambda extracts the layer contents into the /opt directory when setting up the execution environment for the function.
