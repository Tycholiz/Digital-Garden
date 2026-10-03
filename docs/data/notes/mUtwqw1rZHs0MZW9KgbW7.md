
The order that we list our stages defines the execution order for jobs:
- Jobs in the same stage run in parallel.
- Jobs in the next stage run after the jobs from the previous stage complete successfully.

If stages is not defined in the .gitlab-ci.yml file, the default pipeline stages are:
- `.pre`
- `build`
- `test`
- `deploy`
- `.post`

If a job does not specify a stage, the job is assigned the `test` stage.

To make a job start earlier and ignore the stage order, use the `needs` keyword.

