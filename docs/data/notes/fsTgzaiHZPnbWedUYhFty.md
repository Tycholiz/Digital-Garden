
# Elastic Beanstalk
Purpose is to deploy + scale web apps/services built with common web languages (Javascript, PHP etc) onto webservers like Nginx/Apache
- Elastic beanstalk hosts all the services needed for deployment/scaling

Elastic Beanstalk enables you to deploy and scale applications onto Amazon EC2. You retain ownership and full control over the underlying EC2 instances.

Handles things like:
- Provisioning services
- loadbalancing
- scaling infra
- updating platform with latest patches
- app health monitoring

Also allows us to reimplement portions of the code as containerized services, allowing us to achieve a more [[distributed|deploy.distributed]] architectural design

Allows to retain control over the resources powering your application, so you can decide on how much you want to manage.

Alternatives: Azure App Service, Google Cloud App Engine

### Process
1. upload source code bundle to Elastic Beanstalk console
2. Elastic Beanstalk return a new URL for the webapp
