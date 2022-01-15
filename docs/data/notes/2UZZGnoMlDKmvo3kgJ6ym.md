
Virtual Private Cloud (VPC) is a service that allows us control over the virtual networking environment
- gives control over resource placement, connectivity, security etc.

You can use Amazon VPC to create a private network for resources such as databases, cache instances, or internal services.

VPC is a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define.
- You have complete control over your virtual networking environment, including selection of your own IP address ranges, creation of subnets, and configuration of route tables and network gateways.
- The network config of the VPC is highly configurable
	- ex. you can create a public-facing subnet for your web servers that have access to the Internet, and place your backend systems such as databases or application servers in a private-facing subnet with no Internet access.

Process:
1. create your VPC
2. add resources to it, such as EC2 and RDS instances
3. define how your VPCs communicate with each other across accounts, Availability Zones (AZs), or Regions.

VPC comprises a variety of objects:
- *A Virtual Private Cloud*: A logically isolated virtual network in the AWS cloud. You define a VPCs IP address space from ranges you select.
- *Subnet*: A segment of a VPC’s IP address range where you can place groups of isolated resources.
- *Internet Gateway*: The Amazon VPC side of a connection to the public Internet.
- *NAT Gateway*: A highly available, managed [[NAT|network.lan.router.NAT]] service for your resources in a private subnet to access the Internet.
- *Virtual private gateway*: The Amazon VPC side of a VPN connection.
- *Peering Connection*: A peering connection enables you to route traffic via private IP addresses between two peered VPCs.
- *VPC Endpoints*: Enables private connectivity to services hosted in AWS, from within your VPC without using an Internet Gateway, [[VPN|network.VPN]], [[NAT|network.lan.router.NAT]] devices, or firewall proxies.
- *Egress-only Internet Gateway*: A stateful gateway to provide egress only access for IPv6 traffic from the VPC to the Internet.

VPC endpoints enable you to privately connect your VPC to services hosted on AWS without requiring an Internet gateway, a NAT device, VPN, or firewall proxies
- Endpoints are horizontally scalable
- VPC offers two different types of endpoints: gateway type endpoints and interface type endpoints.
	- Gateway type endpoints are available only for AWS services including S3 and DynamoDB.
	- Interface type endpoints provide private connectivity to services powered by PrivateLink.

Multiple VPCs within a single region can communicate with each other
![](/assets/images/2021-11-23-12-55-44.png)

VPCs usually don't communicate, but we can open a few ports if they really need to. the tendency is to be selective

## Concepts
VPC networks includes the following network elements:

- *Elastic network interface* – elastic network interface is a logical networking component in a VPC that represents a virtual network card.
- *Subnet* – A range of IP addresses in your VPC. You can add AWS resources to a specified subnet. Use a public subnet for resources that must connect to the internet, and a private subnet for resources that don't connect to the internet.
- *Security group* – use security groups to control access to the AWS resources in each subnet.
- *Access control list (ACL)* – use a network ACL to provide additional security in a subnet. The default subnet ACL allows all inbound and outbound traffic.
- *Route table* – contains a set of routes that AWS uses to direct the network traffic for your VPC. You can explicitly associate a subnet with a particular route table. By default, the subnet is associated with the main route table.
- *Route* – each route in a route table specifies a range of IP addresses and the destination where Lambda sends the traffic for that range. The route also specifies a target, which is the gateway, network interface, or connection through which to send the traffic.
- *NAT gateway* – An AWS Network Address Translation (NAT) service that controls access from a private VPC private subnet to the Internet.
- *VPC endpoints* – You can use an Amazon VPC endpoint to create private connectivity to services hosted in AWS, without requiring access over the internet or through a NAT device, VPN connection, or AWS Direct Connect connection. For more information, see AWS PrivateLink and VPC endpoints.
