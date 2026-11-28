
Cognito is an identity platform for web and mobile apps. It is...
- a user directory 
- an authentication server
- an authorization service for OAuth 2.0 access tokens and AWS credentials.

Cognito supports MFA for users.

For integrating Cognito with web/mobile apps, AWS recommends using [[Amplify|aws.svc.amplify]]

The two main features of Cognito are User Pools and Identity Pools.

## User pools
A user pool is a user directory, allowing us to create, manage and authenticate users.

Flow:
1. User accesses web application and accesses sign-in page
2. The request to sign in is sent to Cognito, who presents challenges to the user (e.g. email/password)
3. The client sends back the challenge response to Cognito, who provides a token, thereby signing in the user.

![](/assets/images/2024-04-09-10-45-03.png)

When a user on our platform signs up on our website, the client calls Cognito user pool API to create a new user. When that user later signs in, the client again calls the Cognito user pool API to authenticate and authorize them.

## Identity pools
Identity Pools provide credentials that authorize and monitor API requests to AWS services (e.g. Dynamo, S3), allowing us to authorize authenticated/anonymous users to access those AWS resources.
- the identity pool issues AWS credentials for our app to server resources to users.
- Identity pools use both role-based and attribute-based access control to manage your users’ authorization to access your AWS resources.

* * *
- [Auth Flow Configurations](https://stackoverflow.com/questions/54238306/what-the-settings-mean-in-aws-cognito-user-pool-app-client)
