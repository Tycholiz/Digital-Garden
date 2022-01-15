
A layer is a `.zip` file archive that can contain additional code or data.
- it may contain libraries (e.g. node modules), a custom runtime, data, or configuration files.

Lambda layers provide a convenient way to package libraries and other dependencies that you can use with your Lambda functions.

Using layers reduces the size of uploaded deployment archives, making it faster to deploy the code.

Layers also promote code sharing and separation of responsibilities

### Using Layers
When you create a layer, you must bundle all its content into a .zip file archive. You upload the .zip file archive to your layer from Amazon Simple Storage Service (Amazon S3) or your local machine. Lambda extracts the layer contents into the /opt directory when setting up the execution environment for the function.
