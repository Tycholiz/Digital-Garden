
Bundling is process of taking all of your Javascript code (including `node_modules`) and reducing it down to a single file.
- the remaining bundle is as small as possible.

Bundling is normally a concept not found in [[Node.js|js.node]] code, since the size Node.js code on a server doesn't really matter.
- however, there are use-cases for bundling Node.js code. For instance, if we are building multiple Lambdas in a single project using [[serverless-framework]], there is a benefit to bundling, since smaller Lambdas are downloaded from [[aws.svc.S3]] faster, reducing cold start times.
  - in this case, use ESBuild

Think of bundling as a [[MapReduce|general.patterns.map-reduce]] operation that maps over all source files and "reduces" them into a bundle.

Before ES modules were available in browsers, developers had no native mechanism for authoring JavaScript in a modularized fashion. This is why we are all familiar with the concept of "bundling": using tools that crawl, process and concatenate our source modules into files that can run in the browser.

### Tree-shaking
The bundler looks through the code and uses only the imported modules (third-party and your own), thereby elminimating all the modules that aren't a part of the project.

## UE Resources
- [Building a JS bundler](https://cpojer.net/posts/building-a-javascript-bundler)
