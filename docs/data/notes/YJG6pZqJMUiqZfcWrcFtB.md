
Pipelines are the top-level component of continuous integration, delivery, and deployment.

A pipeline is a set of instructions (ie. [[gitlab.CI-CD.jobs]]) that are executed in an order that we define

A merge request can have many pipelines, and each pipeline must belong to one merge request.

Normally a pipeline is created when you push to a branch on Gitlab

Pipelines comprise:
- Jobs, which define what to do. For example, jobs that compile or test code.
- Stages, which define when to run the jobs. For example, stages that run tests after stages that compile the code.

If all jobs in a stage succeed, the pipeline moves on to the next stage.
If any job in a stage fails, the next stage is not (usually) executed and the pipeline ends early.

A typical pipeline might consist of four stages, executed in the following order:
- A build stage, with a job called compile.
- A test stage, with two jobs called test1 and test2.
- A staging stage, with a job called deploy-to-stage.
- A production stage, with a job called deploy-to-prod.

## Types of Pipeline
Pipelines come in various configurations
### Basic pipelines 
run everything in each stage concurrently, followed by the next stage. Run the pipeline each time changes are pushed to a branch.

### Directed Acyclic Graph Pipeline (DAG) 
pipelines are based on relationships between jobs and can run more quickly than basic pipelines.
    - we achieve this with the `needs` keyword, which allows us to be explicit about the job's dependencies (ex. a post-install stage *needs* an install stage to have succeeded first)

### Multi-project pipelines 
combine pipelines for different projects together.

### Parent-Child pipelines 
break down complex pipelines into one parent pipeline that can trigger multiple child sub-pipelines, which all run in the same project and with the same SHA. This pipeline architecture is commonly used for mono-repos.

### Pipelines for Merge Requests 
Run only when commits associated with an MR are pushed (rather than for every commit).
- Merge Request jobs run in a separate pipeline from Commit jobs.
- These pipelines are labeled as `detached` in the UI

Pipelines for merge requests can run when you:
- Create a new merge request.
- Commit changes to the source branch for the merge request.
- Select the Run pipeline button from the Pipelines tab in the merge request.

Can be configured with `rules` or `only/except`

### Pipelines for Merged Results 
merge request pipelines that act as though the changes from the source branch have already been merged into the target branch.

### Merge Trains 
Merge trains use pipelines for merged results to queue merges one after the other.
