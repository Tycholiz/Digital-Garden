
Infrastructure as code (IaC) is the practice of keeping all infrastructure configuration stored as code.
- The actual desired state may or may not be not stored as code (e.g., number of replicas or pods).

IaC encourages and promotes declarative system administration tools over custom imperative solutions, over the primitive way of doing it using loose imperative scripts that were hard to keep track of and difficult to understand.
- This led to the emergence of technologies like Docker Containers, Ansible, Terraform, and Kubernetes, which utilize static declarative configuration files.

IaC is the opposite of click ops.

IaC allows developers to test infra changes very early on in the development cycle, instead of waiting until the last step to deploy to prod, hoping everything works, we just deploy to staging, which should be as close as possible to production (staging should essentially be prod without the users)

### Examples
- AWS Cloudformation
- Azure Resource Manager

Cloud Agnostic:
- Chef
- Puppet
- [[Terraform|terraform]]
- Serverless
- Pulumi
