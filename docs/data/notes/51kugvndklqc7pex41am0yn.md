
Git submodules solve the problem: "what if we need to use another project from within our project repo?"
- ex. we have a module (e.g. a React UI library) that we want to use in multiple projects

One reason that we'd include a submodule package instead of just including it as an npm package it is that in the latter case, we need to run `npm install` in order to get the latest changes.