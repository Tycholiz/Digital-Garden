
Identity federation is a system of trust between two parties for the purpose of authenticating users.
- the SP trusts the IdP to authenticate users and relies on the information provided by the IdP about them.

Federated identities are users that can access secure AWS account resources with external identities. 
- External identities can come from a corporate identity store (such as LDAP or Windows Active Directory) or from a third party (such as Login in with Amazon, Facebook, or Google). 
- Federated identities do not sign in with the AWS Management Console or AWS access portal. The type of external identity in use determines how federated identities sign in.

Identity Federation is a way that we can manage [[federated identity|security.federated-identity]] in AWS.

The problem Identity Federation solves is, "how do we grapple with the fact that we have multitudes of different employees, contractors

- an identity provider (IdP) is responsible for user authentication.
- a service provider (SP) controls access to resources.
	- an SP might be a service or application

After authenticating a user, the IdP sends the SP a message, called an assertion, containing the user's sign-in name and other attributes that the SP needs to establish a session with the user and to determine the scope of resource access that the SP should grant.

Federation is a common approach to building access control systems which manage users centrally within a central IdP and govern their access to multiple applications and services acting as SPs.

AWS SSO and [[AWS|aws.terms.IAM]] are two tools from AWS that allow us to [[federate|general.terms.federated]] our workforce into AWS accounts and business applications.
