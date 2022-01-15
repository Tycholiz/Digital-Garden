
### Root user
Upon creating a new AWS account, we begin with a single sign-in identity that has complete access to all AWS services and resources in the account. This is the root user.

## IAM Identity
An IAM identity provides access to an AWS account.

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

Use an IAM user when...
- You created an AWS account and you're the only person who works in your account.
	- In this case, create an IAM user for yourself and use the credentials for that user when you work with AWS.
- Other people in your user group need to work in your AWS account, and your user group is using no other identity mechanism.
	- In this case, create IAM users for the individuals who need access to your AWS resources, assign appropriate permissions to each user, and give each user his or her own credentials.

### User Group
A user group is a collection of IAM users managed as a unit, allowing us to specify permissions for a collection of users
- ex. you could have a user group called `Admins` and give that user group the types of permissions that administrators typically need

### Role
An IAM role is very similar to a user, in that it is an identity with permission policies that determine what the identity can and cannot do in AWS.
- However, a role does not have any credentials (password or access keys) associated with it.

A role is intended to be used by anyone who needs it, and therefore it is not necessarily associated with a single person.
- that is, an IAM user can assume a role to temporarily take on different permissions for a specific task.

Use an IAM role when...
- You're creating an application that runs on an [[aws.svc.EC2]] instance, and it makes requests to AWS.
- You're creating an app that runs on a mobile phone and that makes requests to AWS.
- Users in your company are authenticated in your corporate network and want to be able to use AWS without having to sign in again—that is, you want to allow users to federate into AWS.
	- In this case, don't create IAM users. Configure a federation relationship between your enterprise identity system and AWS.

# E Resources
[AWS Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html)
