
# Fargate
- serverless compute engine for containers
- adds about 20% in price, but removes a lot of the admin overhead
- removes the need to provision and manage servers
	- don't have to worry about scaling, patching, securing, and managing servers
- Fargate automates how much computing power you need and will scale up/down automatically
- with Fargate, you only interact with your containers
![](/assets/images/2021-03-08-21-25-27.png)

Fargate isn't typically used for long running stuff, as it can get quite expensive quite fast.

### Differences with ECS
With a virtual machine, someone still has to manage the hardware, but with EC2 that someone is AWS and you never even see the hardware.

With ECS on EC2, someone still has to manage the instances, but with ECS on Fargate that someone is AWS and you never even see the EC2 instances.

ECS has a “launch type” of either EC2 (if you want to manage the instances yourself) or Fargate (if you want AWS to manage the instances).