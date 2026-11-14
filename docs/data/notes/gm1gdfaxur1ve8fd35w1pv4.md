
## Deployment Strategies
### Branching Model
The idea here is to have a 1:1 mapping between branches and environments (we might have branches `main`, `staging` and `dev`).

Merging code into any of these environment branches will automatically trigger the [[CD|deploy.CI-CD]] and deploy the code to the corresponding environment

### Manual Deployment Workflow
The idea with this approach is to merge code into the `main` branch once it has been validated. From there, our CD pipeline is set up to automatically deploy to our development environment. However, we will also have additional manual jobs that will allow us to deploy to each additional environment. Typically, we manually deploy to QA first so that testing can be done, followed by staging and then finially production.
- in order to isolate each CD job into its own environment, we typically would create multiple stages of our CD (e.g. `dev`, `staging-us-1`, `staging-us-1`, `prod-us-1` etc.), and make the latter stages dependent on the earlier stages. That way, we cannot deploy to production unless we've already deployed to staging.

In addition to the manual deploy jobs, we can also have optional `rollback` jobs for each environment stage that allow us to revert to the most recent deployment.