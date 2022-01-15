
In a sense, an EC2 instance can be thought of as an atomic building block of AWS compute.

An instance of EC2 (Elastic Cloud Compute) is a VM on an AWS server
The nice thing about EC2 is that the computer you get will be very similar to the computer you use to develop your software. If you can run your software on your computer, you can almost certainly run it on EC2 without any changes.
- This is one of EC2’s main advantages compared to other types of compute platforms (such as [[Lambda|aws.svc.lambda]] ): you don’t have to adapt your application to your host.

With EC2 you get an OS, a file system, access to the server’s hardware, etc.

Elastic refers to the fact that it can scale up/down as needed automatically

EC2 has dozens of options you will probably never need, and thus it comes with sensible defaults.
-  This is the result of the highly varied workloads and use cases serviced by EC2

Selecting an instance type will be the most consequential decision when provisioning an EC2 instance.
- There are 256+, and can be narrowed to a few categories, defined by what they're optimized for:
	- CPU
	- Memory
	- Network
	- Storage

If you were building your own server, there would be an infinite number of ways to configure it, but with EC2 you get to pick an instance type from its catalog.
- This is the tradeoff you make as opposed to getting you build your own server, but most likely it shouldn't even be a concern.

### Security
the defaults are sensible, but we may have to modify:
- The security group
	- You can think of security groups as individual firewalls for your instances. With security groups, you can control what goes in and out of your instances.
- The VPC ACL.
	- You can think of VPC ACL as a network firewall. With the VPC ACL, you can control what goes in and out of your network.

### Scaling
The auto part of Auto Scaling allows us to automatically add/remove EC2 instances (therefore horizontal scaling). In theory it works, but is often impractical.
The main premise of Auto Scaling is that once you decide how much headroom you want, you’ll be able to make that headroom a constant size

Unless the following 2 are true, then it's probably not worth messing around with auto-scaling:
- Are your EC2 costs high enough that any reduction in usage will be materially significant?
- If your EC2 bill were to go down by 30%—would that be a big deal for your business?

#### How many EC2 instances to run?
Capacity Headroom - we need to have a buffer between the expected peak demand and the maximum capacity that our system can handle.

Pricing
- only pay for the number of seconds your instance is running.
