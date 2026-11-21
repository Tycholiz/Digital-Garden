
ElasticBeanstalk is a PaaS that enables you to deploy and scale applications onto Amazon EC2. You retain ownership and full control over the underlying EC2 instances.

Purpose is to deploy + scale web apps/services built with common web languages (Javascript, PHP etc) onto webservers like Nginx/Apache
- Elastic beanstalk hosts all the services needed for deployment/scaling

ElasticBeanstalk uses [[aws.svc.cloud-formation]] under the hood to manage and provision resources for your application.

ElasticBeanstalk Handles things like:
- Provisioning EC2 instances and scaling up/down as necessary
- loadbalancing
- updating platform with latest patches
- app health monitoring
- Web server (Nginx)

Also allows us to reimplement portions of the code as containerized services, allowing us to achieve a more [[distributed|deploy.distributed]] architectural design

Allows to retain control over the resources powering your application, so you can decide on how much you want to manage.

To get an app working through ElasticBeanstalk, we essentially...
1. upload source code bundle to Elastic Beanstalk console
2. Elastic Beanstalk returns a new URL for the webapp

* * *

Alternatives: Azure App Service, Google Cloud App Engine, DigitalOcean App Platform
