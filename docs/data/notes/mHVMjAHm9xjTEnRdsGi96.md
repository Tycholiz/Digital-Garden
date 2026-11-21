
## What is it?
A layer is a `.zip` file archive that can contain additional code or data.
- it may contain libraries (e.g. node modules), a custom runtime, data, or configuration files.
- think of it in much same way as how a [[docker image|docker.image]] is built using layers, or how [[Kustomize|k8s.tools.kustomize]] is used to reduce redunant code between configurations. The rationale is the same.

## Why use it?
A key benefit of Lambda layers is that the layers stay warm (ie. initialized, eliminating the need for a cold start)
- the potential for this as a performance enhancement is huge, because if our Lambda uses many third-party dependencies, they can be aggregated into a layer (or many layers) and they won't have to be downloaded on cold starts.

Lambda layers provide a convenient way to package libraries and other dependencies that you can use with your Lambda functions.

Using layers reduces the size of uploaded deployment archives, making it faster to deploy the code.

Layers also promote code sharing and separation of responsibilities

## How does it work?
- When you add a layer to a function, Lambda extracts the layer contents into the /opt directory in your function’s execution environment

### Versioning
When you create a new layer, Lambda creates a new layer version with a version number of 1. Each time you publish an update to the layer, Lambda increments the version number and creates a new layer version.
- Every layer version is identified by a unique ARN. Therefore, when adding a layer to the function, you must specify the exact layer version you want to use.

### Using Layers
the general process of creating and using layers involves these three steps:
1. Package your layer content into a `.zip` file
2. Create the layer in Lambda AWS console (or via Lambda API).
3. Add the layer to your function(s).

When you create a layer, you must bundle all its content into a .zip file archive. You upload the .zip file archive to your layer from Amazon Simple Storage Service (Amazon S3) or your local machine. Lambda extracts the layer contents into the /opt directory when setting up the execution environment for the function.
