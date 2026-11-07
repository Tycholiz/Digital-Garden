
## Types of Variables
There are 4 types of environment variables we can define, differentiated by *where* they are defined:
- in the `gitlab-ci.yml` file
- project (repo)-level CI/CD variables. Done in the project settings of the UI.
- [[group|gitlab#groups]]-level CI/CD variables. Done in the project settings of the UI.
- instance (ie. self-hosted) CI/CD variables

* * *

### Predefined Variables
Gitlab provides predefined environment variables [(docs)](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)

Example variables
- current stage, current job, if and how the pipeline was triggered, commit details (message, author, branch name, SHA)
- AWS variables like AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION

note: some variables are available in some pipeline types but not others (ie. some may be available for branch pipelines, but not merge request pipelines)
- merge request pipelines get their own set of variables.

### Using variables
There are two places defined variables can be used. On the:
- GitLab side, in .gitlab-ci.yml.
- The GitLab Runner side, in config.toml.

[docs](https://docs.gitlab.com/ee/ci/variables/where_variables_can_be_used.html)

#### Expansion mechanisms
Variables can be expanded (ie. resolved) via 3 different methods
- GitLab
- GitLab Runner
- Execution shell environment

Variables used in `.gitlab-ci.yml` can be expanded with `$variable`, or `${variable}` or `%variable%`
- Each form is handled in the same way, no matter which OS/shell handles the job, because the expansion is done in GitLab before any runner gets the job.

GitLab expands job variable values recursively before sending them to the runner. This means the following works:
```yml
- BUILD_ROOT_DIR: '${CI_BUILDS_DIR}'
- OUT_PATH: '${BUILD_ROOT_DIR}/out'
- PACKAGE_PATH: '${OUT_PATH}/pkg'
```
