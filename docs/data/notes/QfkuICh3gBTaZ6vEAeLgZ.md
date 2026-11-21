
An executor is essentially the environment where the job will be executed.

Each [[runner|gitlab.CI-CD.runner]] will define at least one executor. 

if you need Node.js to run a job, this needs to be installed on the machine where the runner is installed.
- Since everything needed is already available on runtime, the jobs are executed very fast.

Gitlab allows us to use the following different executors:
- Shell
    - run the jobs directly on the machine where the runner has been installed.
    - use this when you need a native-run environment (ex. you need to ensure that your software runs on a specific OS or hardware).
    - since we are just running in a shell, there will be left-overs from previous jobs. This also means that it's harder to manage dependencies (e.g. if we need node 12 for one job and node 14 for another)
- SSH
- VirtualBox
- Parallels
- Docker
    - gives us a clean environment for each job. Therefore, this should be the default executor we use.
    - however, there is added overhead since we need to pull a Docker image for each job.
- Docker Machine
- Kubernetes
    - use this if we already have kubernetes set up.
    - the GitLab Runner will run the jobs on a Kubernetes cluster. Therefore, each job will have its own pod.
    - this executor can help us scale our build infrastructure.

# Resources
https://medium.com/devops-with-valentine/a-brief-guide-to-gitlab-ci-runners-and-executors-a81b9b8bf24e
https://docs.gitlab.com/runner/executors/