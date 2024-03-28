
## Continuous Integration (CI)
a CI system will automatically build code, run linters, run tests etc., and provide immediate feedback on the result of those actions.
- a CI can also automate other parts of your development process by managing deployments and notifications.
- "continuous" means that it happens with each "checkin" of the code (updating code on the host)

When you instruct a build to be made, the CI runner clones the github repo into a brand-new virtual environment, then runs the tests and creates the build.
- if any of those tasks fail, the build is considered broken
- if all tasks succeed, the build passes and Travis CI can deploy the code

### Examples
- [[Github Actions|github.actions]]
- Travis CI
- Jenkins
- Circle CI

## Continuous Deployment
This process is about taking all of the commits we make during the development process, and deploying them to the live (production) application automatically.
- As part of this process, we subject our code to a multitude of tests (unit, e2e etc.), which is CI.

## Continuous Delivery
One step past, whereby we end up in a state where a deployment can be triggered, albeit manually.
- more realistically might be called "continuous deliverables".
- the idea here is that it empowers the decision makers to be able to control deployments. The code then becomes something like a deliverable to a boss' desk, rather than you just going ahead and deploying it whenever you merge into main

Continuous Delivery is about making sure that every change a developer makes can make it through the pipeline that includes subjecting our code to a series of tests 
- does it compile? does it pass unit tests? does it pass E2E? etc

It is slightly broader than Continuous Deployment, in the sense that not all changes that go through the pipeline need to necessarily make it into production.
- In this way, you could think of Continuous Delivery as baking in the business-level ability to trigger deployments. Putting code into production is a business-decision, not a technical one. The executives of a company may decide "we only want to deploy new releases every 2 weeks, so that the amount of change that user's experience isn't so jarring". The point is that from a technical standpoint, we can do this— we just decide not to for business reasons.
    - This above explanation shows that in order to do Continuous Delivery, we have to already be doing Continuous Deployment

### Why use CD (both forms)?
- reduced deployment risk
    - with smaller deployments, it's easier to spot when/where something goes wrong
        - anal: same reason why smaller commits are preferable
- user feedback
    - with incremental and automated change, it's easier to get more reliable feedback from users, rather than having to depend on what they tell you they want.
