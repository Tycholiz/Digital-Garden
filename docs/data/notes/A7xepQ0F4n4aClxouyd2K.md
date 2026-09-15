
Module Federation allows multiple webpack builds to work together.
- From the runtime environment's perspective, modules from multiple different builds will behave like a huge connected module graph.
- From the developer's perspective, we will be able to import modules from specific remote builds, and use them with minimal restriction.

### Motivation
Imagine we want to implement a micro frontend [[general.arch.microservice.micro-frontend]], which would have these qualities:
- It should take multiple separate builds to form a single application.
- These separate builds should not have dependencies between each other, so they can be developed and deployed individually

There is a distinction between local and remote modules
- Local modules are normal modules which are part of the current build.
- Remote modules are modules that are not part of the current build and loaded from a so-called container at the runtime.
	- Loading remote modules is considered async (due to the network call involved). When using a remote module these asynchronous operations will be placed in the next chunk of loading operations that is between the remote module and the entrypoint. Chunk loading is necessary.

A container is created through a container entry, which exposes asynchronous access to the specific modules. The exposed access is separated into two steps:
1. loading the module (asynchronous)
	- done during the chunk loading
2. evaluating the module (synchronous).
	- done during the module evaluation interleaved with other local and remote modules
		- This way, evaluation order is unaffected by converting a module from local to remote or the other way around.

### High-level Overview
Each build acts as a container and also consumes other builds as containers. This way each build is able to access any other exposed module by loading it from its container.

### Goals of the system
- It should be possible to expose and use any module type that webpack supports.
- Chunk loading should load everything needed in parallel (web: single round-trip to server).
- Control from consumer to container
	- Overriding modules is a one-directional operation.
	- Sibling containers cannot override each other's modules.
- Concept should be environment-independent.
	- Usable in web, Node.js, etc.
- Relative and absolute request in shared:
	- Will always be provided, even if not used.
	- Will resolve relative to `config.context`.
	- Does not use a `requiredVersion` by default.
- Module requests in shared:
	- Are only provided when they are used.
	- Will match all used equal module requests in your build.
	- Will provide all matching modules.
	- Will extract `requiredVersion` from package.json at this position in the graph.
	- Could provide and consume multiple different versions when you have nested node_modules.
- Module requests with trailing `/` in shared will match all module requests with this prefix.

### Use cases
- separate builds per page
	- Each page of a Single Page Application is exposed from container build in a separate build. This way each page can be separately deployed
- Components library as container
	- Many applications share a common components library which could be built as a container with each component exposed. Each application consumes components from the components library container.
	- Changes to the components library can be separately deployed without the need to re-deploy all applications.

# E Resources
[Webpack docs](https://webpack.js.org/concepts/module-federation/)
