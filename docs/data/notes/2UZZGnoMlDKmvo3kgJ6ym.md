
## What is it?
Virtual Private Cloud (VPC) is a service that allows us control over the virtual networking environment
- gives control over resource placement, connectivity, security etc.

You can use Amazon VPC to create a private network for resources such as databases, cache instances, or internal services.

VPC is a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define.
- You have complete control over your virtual networking environment, including selection of your own IP address ranges, creation of subnets, and configuration of route tables and network gateways.
- The network config of the VPC is highly configurable
	- ex. you can create a public-facing subnet for your web servers that have access to the Internet, and place your backend systems such as databases or application servers in a private-facing subnet with no Internet access.

### Why does it exist?
a VPC is analogous to your home local network.
- Similar to how the devices in your home need a local IP address so the router knows where to direct traffic, your resources need to exist in a VPC so it can receive traffic.
- If you want the LAN (VPC) to talk to the internet, you can add a NAT Gateway (just like your home router).
	- The rest of the internet can only respond to packets from your computers. The Internet can't initiate a connection to your computers -- because they can't even get "your" IP address (it's un-routable, remember). They can only talk to the NAT gateway, and that only routes existing connections from the LAN.
- If you want your computers to respond to the Internet, you can "poke holes in the firewall" using various technologies such as ALB, ELB, routing, peering, etc.

Everything attached to the internet needs to be in a network. If you attached network cables directly from all the compute devices to a huge router then you'd have a huge open network with minimal control over what is allowed to talk to what. Not ideal at all if one device gets compromised and then can hack everything else right?

VPC separates the giant network of the AWS data centers into smaller logically isolated networks and lets you build out rules about what networked resources you want to be able to talk to what other networked resources.

Things in a VPC can't talk over the network to anything else outside of the VPC or anything else in another VPC unless you explicitly allow it. This makes VPC "secure by default". It starts out completely isolated from the global internet and isolated from everything else inside of AWS. Then you build out the set of minimal network access rules and routing rules you'd like to have in your network.

As a side note, most of the time if you are having trouble understanding AWS networking its because of this "secure by default" aspect. And if your networking isn't working then 99% chance it's a missing or misconfigured security group rule, or missing networking route that means that the VPC isn't allowing this networking connection until you explicitly allow it.

## Why use it?
Compare it to your own home network, where devices within the network can communicate with each other without too many security barriers in between. In addition to that, each device has access to the internet via the [[NAT|network.lan.router.nat]], but internet traffic can't come directly in.
- if you want something externally visible, you put it in a DMZ or public subnet.

In a microservices architecture, imagine you had a [[Serverless|serverless-framework]] repo with some [[lambdas|aws.svc.lambda]]. If those lambdas needed to communicate with any of our other services (assuming they too were in the same VPC), we would need to put them in the VPC.
- In Serverless Framework in particular, this is just a config option in the `serverless.yml` file.
- Lambdas are the type of thing that should be left out of the VPC unless they absolutely need to be in it. There is a cost to putting them in the VPC. They cost more $ and have slower cold start times. They also carry more potential for connectivity issues.
	- ex. since Elasticache (AWS Redis) must run in a VPC, if our lambda needed to connect to it, the lambda would also need to exist in the same VPC.

VPC endpoints enable you to privately connect your VPC to services hosted on AWS without requiring an Internet gateway, a NAT device, VPN, or firewall proxies
- Endpoints are horizontally scalable
- VPC offers two different types of endpoints: gateway type endpoints and interface type endpoints.
	- Gateway type endpoints are available only for AWS services including S3 and DynamoDB.
	- Interface type endpoints provide private connectivity to services powered by PrivateLink.

Multiple VPCs within a single region can communicate with each other
![](/assets/images/2021-11-23-12-55-44.png)

Different VPCs usually don't communicate with each other, but we can open a few ports if they really need to.

## How does it work?
Process:
1. create your VPC
2. add resources to it, such as EC2 and RDS instances
3. define how your VPCs communicate with each other across accounts, Availability Zones (AZs), or Regions.

AWS VPC comprises a variety of objects:
- *A Virtual Private Cloud*: A logically isolated virtual network in the AWS cloud. You define a VPCs IP address space from ranges you select.
- *Subnet*: A segment of a VPC’s IP address range where you can place groups of isolated resources.
- *Internet Gateway*: The Amazon VPC side of a connection to the public Internet.
- *NAT Gateway*: A highly available, managed [[NAT|network.lan.router.NAT]] service for your resources in a private subnet to access the Internet.
- *Virtual private gateway*: The Amazon VPC side of a [[VPN|network.vpn]] connection.
- *Peering Connection*: A peering connection enables you to route traffic via private IP addresses between two peered VPCs.
- *VPC Endpoints*: Enables private connectivity to services hosted in AWS, from within your VPC without using an Internet Gateway, [[VPN|network.VPN]], [[NAT|network.lan.router.NAT]] devices, or firewall proxies.
- *Egress-only Internet Gateway*: A stateful gateway to provide egress only access for IPv6 traffic from the VPC to the Internet.

## Concepts
<!-- - note: is this just repeated from above? -->
VPC networks includes the following network elements:

- *Elastic network interface* – elastic network interface is a logical networking component in a VPC that represents a virtual network card.
- *Subnet* – A range of IP addresses in your VPC. You can add AWS resources to a specified subnet. Use a public subnet for resources that must connect to the internet, and a private subnet for resources that don't connect to the internet.
- *Security group* – use security groups to control access to the AWS resources in each subnet.
- *Access control list (ACL)* – use a network ACL to provide additional security in a subnet. The default subnet ACL allows all inbound and outbound traffic.
- *Route table* – contains a set of routes that AWS uses to direct the network traffic for your VPC. You can explicitly associate a subnet with a particular route table. By default, the subnet is associated with the main route table.
- *Route* – each route in a route table specifies a range of IP addresses and the destination where Lambda sends the traffic for that range. The route also specifies a target, which is the gateway, network interface, or connection through which to send the traffic.
- *NAT gateway* – An AWS Network Address Translation (NAT) service that controls access from a private VPC private subnet to the Internet.
- *VPC endpoints* – You can use an Amazon VPC endpoint to create private connectivity to services hosted in AWS, without requiring access over the internet or through a NAT device, VPN connection, or AWS Direct Connect connection. For more information, see AWS PrivateLink and VPC endpoints.
