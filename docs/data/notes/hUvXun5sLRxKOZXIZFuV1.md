
## IAM Identity
An IAM identity provides access to the resources under management by an AWS account.

An IAM identity represents a user, and can be authenticated and then authorized to perform actions in AWS.

Each IAM identity can be associated with one or more policies.
- Policies determine what actions a user, role, or member of a user group can perform, on which AWS resources, and under what conditions.

### User
An IAM user is an entity that you create in AWS.
- The IAM user uniquely represents the person or service who uses the IAM user to interact with AWS.

A primary use for IAM users is to give people the ability to sign in to the AWS Management Console for interactive tasks and to make programmatic requests to AWS services using the API or CLI.

A user in AWS consists of a name, a password, and up to 2 access keys used to access the API/CLI.

When you create an IAM user, you grant it permissions by making it a member of a user group that has appropriate permission policies attached
- you can also directly attaching policies to the user, though this is less recommended.

When you create an IAM user, you can choose to allow console or programmatic access. 
- if console access is allowed, the IAM user can sign in to the console using a user name and password. 
- if programmatic access is allowed, the user can use access keys to work with the CLI or API.

#### Use an IAM user when...
- You created an AWS account and you're the only person who works in your account.
	- In this case, create an IAM user for yourself and use the credentials for that user when you work with AWS.
- Other people in your user group need to work in your AWS account, and your user group is using no other identity mechanism.
	- In this case, create IAM users for the individuals who need access to your AWS resources, assign appropriate permissions to each user, and give each user his or her own credentials.

### User Group
A user group is a collection of IAM users managed as a unit, allowing us to specify permissions for a collection of users
- ex. you could have a user group called `Admins` and give that user group the types of permissions that administrators typically need

### Role
A role is a collection of policies which can be applied to a user / other service.

A role can only be added at time of instance and user creation though. 
- A role cannot be added after the fact (though permissions for that role can be)

An IAM role is very similar to a user, in that it is an identity with permission policies that determine what the identity can and cannot do in AWS.
- However, a role does not have any credentials (password or access keys) associated with it.

Once a role is given, it can interact with AWS via a CLI or SDK and does not need to authenticate or provide credentials (which is all done behind the scenes)

A user can also assume a role, either from the same AWS account (via federated login with identity provider) or a different AWS account (via `AssumeRole`)

A role is intended to be used by anyone who needs it, and therefore it is not necessarily associated with a single person.
- that is, an IAM user can assume a role to temporarily take on different permissions for a specific task.

If we didn't have roles, the client (potentially itself a AWS service) accessing another AWS service would need to hold the access key and secret key of a user, and those credentials would be used to access the AWS resource

#### Use an IAM role when...
- You're creating an application that runs on an [[aws.svc.EC2]] instance, and it makes requests to AWS.
- You're creating an app that runs on a mobile phone and that makes requests to AWS.
- Users in your company are authenticated in your corporate network and want to be able to use AWS without having to sign in again—that is, you want to allow users to federate into AWS.
	- In this case, don't create IAM users. Configure a federation relationship between your enterprise identity system and AWS.

### Policy
A policy is a whitelist of permissions for an [action](https://docs.aws.amazon.com/IAM/latest/APIReference/API_Operations.html). We then take that policy and attach it to an IAM identity (ie. user, user group or role)
- ex. `AmazonGlacierFullAccess`, `AmazonGlacierReadOnlyAccess`, `AWSLambdaRole`
- it does not matter if the resource is accessed from the AWS Management Console, the AWS CLI, or the AWS API.
- e.g. actions: `secretsmanager:GetSecretValue`

When an *IAM principal* (user or role) makes a request, AWS will evaluate the policy and determine if the request is allowed or not.

Policies are stored as JSON documents, like so:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "lambda:InvokeFunction"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

AWS supports six types of policies ([docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#access_policy-types)): 
- identity-based policies
- resource-based policies
- permissions boundaries
- Organizations SCPs
- access control lists (ACLs)
- session policies

Policies may be AWS managed, meaning they are created by AWS, or they may be Customer managed, meaning they are created by the AWS root user. In the latter case, we create the JSON policy ourselves, defining the permissions that it would grant when attached to an entity.

#### Identity-based Policies
Identity-based policies are JSON permissions policy documents that control what actions an identity (users, groups of users, and roles) can perform, on which resources, and under what conditions.

Identity-based policies can be either:
- Managed - standalone policies that can be attached to multiple users, groups and roles of the AWS account. They can either be managed by AWS or by the account holder.
- Inline - policies that get added directly to a single user, group or role (ie. one-to-one relationship).

#### Resource-based Policies
JSON policy documents that we attach to a resource (e.g. S3 bucket)
- this grants the specified principal (ie. user/role) permission to perform specific actions on that resource and defines under what conditions this applies

## Gotchas
- IAM role names and policy names exist at a global namespace (IAM namespace), so if we had [[lambdas|aws.svc.lambda]] in both us-east-1 and us-west-2 and wanted to create IAM policies for each, we'd have to name the policies/roles differently (e.g. add a region suffix).

### AWS IAM vs AWS IAM Identity Center
These are 2 different AWS console applications

*AWS IAM* manages access to AWS services and resources within an AWS account securely for entities (e.g. IAM users)

*AWS Identity Center* manages access to all AWS services and resources within an AWS organization, as well as access to other cloud applications, e.g., Salesforce.
- ideal for managing multiple AWS accounts
- This service is the successor to *AWS Single Sign-on*

# E Resources
[AWS Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html)
