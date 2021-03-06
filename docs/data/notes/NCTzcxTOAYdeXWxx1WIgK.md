
A Github Action is installed as soon as we have a `.github/workflows` directory committed to our repo on Github.

![](/assets/images/2021-04-15-21-56-11.png)

## Terminology

### Workflow
![[github.actions.workflow]]

### Job
![[github.actions.job]]

### Step
![[github.actions.step]]

### Action
![[github.actions.action]]

### Runner
![[github.actions.runner]]

* * *

### Secrets
`GITHUB_TOKEN` is passed to the runner when a workflow is triggered.
- Aside from this, no other secrets are passed by default, and must be configured manually.

To provide an action with a secret (either as input or as an env variable), use the `secrets` context.
- This allows us to access secrets we've created in our repo.
- [more info](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions)

Secrets should not be passed between processes via the command line, as these commands might be visible to other users with the `ps` command.
- This is why we use `STDIN` or env variables to pass secrets.

# UE Resources
[Actions V2 features](https://jasonet.co/posts/new-features-of-github-actions/)
