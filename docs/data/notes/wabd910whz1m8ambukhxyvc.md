
### Root user
Upon creating a new AWS account, we begin with a single sign-in identity that has complete access to all AWS services and resources in the account. This is the root user.
- Never use the root account for anything real; use it to set up your account and payment method and contact methods and most importantly, your IAM 'real' users. Then secure it with MFA and never login again until you have to do something that requires root access.
- Never ever create API keys or access credentials for root user. Those are literally the keys to your entire kingdom and losing them will be catastrophic

### AWS Organization
AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage.