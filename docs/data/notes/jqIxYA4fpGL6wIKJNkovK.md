
Artifacts come in 2 flavors: Job Artifacts and Pipeline Artifacts

## Job Artifacts
Jobs can output an archive of files and directories. This output is known as a job artifact.

Often our [[jobs|gitlab.CI-CD.jobs]] generate files which are used by subsequent jobs/stages.
- ex. our deploy stage might generate libraries and files which are then used to deploy the application. These files are collectively known as the artifact
    - this can give us insight into how something was deployed

Artifacts can be uploaded and browsed by using the `artifact` property on the job.
- similar to a [[cache|gitlab.CI-CD.cache]], it is then available to other jobs (though they must be in the same pipeline).

You may want to create artifacts only for tagged releases to avoid filling the build server storage with temporary build artifacts. For example, use rules to create artifacts only for tags:

```yml
default-job:
  script:
    - yarn test
  rules:
    - if: $CI_COMMIT_BRANCH

release-job:
  script:
    - yarn package -U
  artifacts:
    name: "$CI_COMMIT_REF_NAME"
    paths:
      - target/*.war
  rules:
    - if: $CI_COMMIT_TAG
```

You can use wildcards for directories too. For example, if you want to get all the files inside the directories that end with xyz:
```yml
job:
  artifacts:
    paths:
      - path/*xyz/*
```

Save all Git untracked files and files in `binaries/`:
```yml
artifacts:
  untracked: true
  paths:
    - binaries/
```

Imagine we are using [[Serverless Framework|serverless-framework]] to create 3 [[lambdas|aws.svc.lambda]]. Building this will result in us having 3 different directories, each with its own `node_modules/`. We may want to create this artifact (which is in effect a `dist/` folder with 3 subdirectories).

## How to view + download Artifacts
[docs](https://docs.gitlab.com/ee/ci/pipelines/job_artifacts.html#download-job-artifacts)

## Pipeline Artifacts
Pipeline artifacts are files created by GitLab after a pipeline finishes. 
- Pipeline artifacts are different to job artifacts because they are not explicitly managed by `.gitlab-ci.yml` definitions.

ex. Pipeline artifacts are used by the [test coverage visualization feature](https://docs.gitlab.com/ee/user/project/merge_requests/test_coverage_visualization.html) to collect coverage information. It uses the artifacts: reports CI/CD keyword.

The latest pipeline artifacts are kept forever. Once a new one comes in, old ones will live on for 7 days after creation date.
