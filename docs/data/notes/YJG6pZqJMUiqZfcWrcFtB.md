
Pipelines are the top-level component of continuous integration, delivery, and deployment.

A pipeline is a set of instructions (ie. [[gitlab.CI-CD.jobs]]) that are executed in an order that we define

A merge request can have many pipelines, and each pipeline must belong to one merge request.

Normally a pipeline is created when you push to a branch on Gitlab

Pipelines comprise:
- Jobs, which define what to do. For example, jobs that compile or test code.
- Stages, which define when to run the jobs. For example, stages that run tests after stages that compile the code.

If all jobs in a stage succeed, the pipeline moves on to the next stage.

If any job in a stage fails, the next stage is not executed and the pipeline ends early.
- we can set a job to `allow_failure: true` if we want to override this behaviour.

A typical pipeline might consist of four stages, executed in the following order:
- A test stage, with two jobs called unittest and lint.
- A build stage, with a job called compile.
- A staging stage, with a job called deploy-to-stage.
- A production stage, with a job called deploy-to-prod.

## Types of Pipeline
Pipelines come in various configurations

### Basic pipelines (ie. Branch Pipelines)
run everything in each stage concurrently, followed by the next stage. Run the pipeline each time changes are pushed to a branch.

Basic pipelines have some characteristics:
- they run when you push a new commit to a branch.
- they have access to some predefined variables.
- they have access to protected variables and protected runners.

### Merge Request Pipelines
Run only when commits associated with an MR are pushed (rather than for every commit).
- Merge Request jobs run in a separate pipeline from Commit jobs.

Pipelines for merge requests run when you:
- Create a new merge request.
- Commit changes to the source branch for the merge request.
- Select the Run pipeline button from the Pipelines tab in the merge request.

This means that if you use a squash-based workflow, jobs in the MR pipelines (ie. those associated with each commit to that MR) will have run on a different commit than the commit that actually gets merged into the `main` branch.
- remember that in a squash-based workflow, all the commits that went into that MR will not make its way into the repo's history. Consider that if we have a job that depends on a git commit SHA (e.g. a `contract-test` job that records the publication of the contract by the commit SHA), we cannot perform this job in the MR pipeline alone, because the contract testing framework (here, [[Pact|testing.method.contract.pact]]) will only be aware of the commit SHA-related contracts that are associated with those commits that will never make its way into the main commit history line.

Merge Request Pipelines don't run by default; they *must* be configured with `rules` or `only/except` (old method)

Jobs of a merge request pipeline only run on the contents of the source branch, ignoring the target branch.

Merge Request Pipelines also have access to additional [pre-defined variables](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html#predefined-variables-for-merge-request-pipelines) 

Merge Request Pipelines display the `merge request` tag:

![](/assets/images/2022-08-25-09-54-07.png)

### Git Tag Pipelines
- note: "git tag pipeline" is not an official type of pipeline

A pipeline that runs once a git tag has been created
![](/assets/images/2022-11-16-04-26-39.png)

```yml
.git_tag_trigger:
  only:
    variables:
      - $CI_COMMIT_TAG
```

### Merged Requests Pipeline
- note the merge**d** request**s**

merge request pipelines that act as though the changes from the source branch have already been merged into the target branch.

### Merge Trains 
Merge trains use pipelines for merged results to queue merges one after the other.

### Directed Acyclic Graph Pipeline (DAG) 
pipelines are based on relationships between jobs and can run more quickly than basic pipelines.
- we achieve this with the `needs` keyword, which allows us to be explicit about the job's dependencies 
    - ex. a post-install stage *needs* an install stage to have succeeded first

### Multi-project pipelines 
combine pipelines for different projects together.

### Parent-Child pipelines 
break down complex pipelines into one parent pipeline that can trigger multiple child sub-pipelines, which all run in the same project and with the same SHA. This pipeline architecture is commonly used for mono-repos.
