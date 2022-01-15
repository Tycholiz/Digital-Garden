
A cold start for serverless functions is directly related to the size of the function itself. The reason why cold starts exist is because the function is stored as a zip file, and it needs to be unzipped before it is mounted. Therefore, the solution to having shorter cold starts is to have smaller functions

Each function should have its own package.json

# Tools
begin.com
