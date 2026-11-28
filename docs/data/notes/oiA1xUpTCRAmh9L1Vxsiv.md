
A cold start for serverless functions is directly related to the size of the function itself. The reason why cold starts exist is because the function is stored as a zip file, and it needs to be unzipped before it is mounted. Therefore, to have shorter cold starts we must write smaller functions.
- Therefore, each serverless function in Node.js should have its own `package.json`

# Tools
begin.com
