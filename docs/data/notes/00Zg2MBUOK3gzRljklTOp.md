
A job is an atomic unit that is picked up by the runner.
- If we were acting as the runner, we would simply run a script, like `npm run test` or `npm run lint`. These are 2 different jobs, and they are picked up by the runner.
- jobs are executed in the environment of the runner.

Multiple jobs in the same stage are executed in parallel

Jobs are defined at the top-level of the `gitlab-ci.yml` file.
- you might have jobs called Lint, Test, Deploy Preview, etc

What is important is that each job is run independently from each other.

with rules, we can modify the order that jobs within a stage will be run.

Jobs prepended with a `.` will be hidden, and will not be processed by Gitlab CI/CD.
- You can use hidden jobs as templates for reusable configuration with `extends` keyword or YAML anchors.

### Running jobs conditionally (`rules`)
```yml
default-job:
  script:
    - yarn test
  rules:
    - if: $CI_COMMIT_BRANCH
```

### Groups of jobs
```yml
build ruby 1/3:
  stage: build
  script:
    - echo "ruby1"

build ruby 2/3:
  stage: build
  script:
    - echo "ruby2"

build ruby 3/3:
  stage: build
  script:
    - echo "ruby3"
```

Each job can run in a separate isolated [[Docker container|docker.containers]]
- [docs](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html)

### Resource Group
Allows us to make sure only one job per resource group is working at a time
- ex. consider 2 different pipelines of the same repo that both deploy to the same place. Of course, deployments should happen sequentially, so we put both of those jobs in the same resource_group, guaranteeing that they will run one after the other.

A job can be specified as part of a `resource_group` which ensures a job is mutually exclusive across different pipelines for the same project.
- if multiple jobs that belong to the same resource group are queued simultaneously, only one of the jobs starts. The other jobs wait until the resource_group is free.
- ex. `resource_group=prod` puts a limitation that only one job in the `prod` resource group may run at a time.
- Resource groups behave similar to semaphores in other programming languages.

### Service
When we use services, we specify 2 keywords:
- services
- images

A job can define a Docker image

* * *

You can’t use these keywords as job names:
- image
- services
- stages
- types
- before_script
- after_script
- variables
- cache
- include
