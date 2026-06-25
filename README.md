# Build-Basic-VPC-with-Traffic-Flow-and-Security-on-AWS



## Introducing Today's Project!

In this project, I will demonstrate building a basic VPC with subnets and internet gateway, as well as utilise VPC traffic Flow and security tools. I'm doing this project to learn the fundementals of cloud networking, learn how subnet divides up network space and how an Internet Gateway opens up communication to the public web.

### What is Amazon VPC?

Amazon VPC is a logically isolated section of the AWS cloud where you can launch your computing resources, such as databases and virtual servers. It acts as your own private data center in the cloud, giving you complete control over your network configurations, IP addresses, subnets, and routing.

In today's project, I used Amazon VPC to launch a virtual private netwrk on AWS with public subnet and connected the VPC to an internet gateway.

### Personal reflection

One thing I didn't expect in this project was that AWS proves a default VPC with subsets and internet gateway as pre-existing resources on AWS can not run without a default VPC connection.

---

## Virtual Private Clouds (VPCs)

### What I did in this step

In this step, I will acces the VPC cosnsule in AWS and create a VPC.

### How VPCs work

VPCs provides you with your own private cloud on AWS. It as your own private, isolated city inside that AWS country. Here is how it helps you:
Security and Isolation: Your VPC is completely isolated from other users' networks. Your resources are private by default.
Custom Layouts: You get to draw the boundaries! You can divide your city into neighborhoods called subnets.
Traffic Control: You decide who can enter and leave. For example, you can connect your VPC to the public internet using an Internet Gateway, or keep it entirely locked down.

### Why there is a default VPC in AWS accounts

There was already a default VPC in my account ever since my AWS account was created. This is because some of AWS services, such as Amazon EC2, can not run withuot a network. If AWS did not give you a default VPC, you would have to learn how to manually build a network, configure subnets, and attach an Internet Gateway before you could launch your very first server!

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-vpc_2facf927)

### Defining IPv4 CIDR blocks

To set up my VPC, I had to define an IPv4 CIDR block, which is a way to assign a whole block of IP addresses. It is shown by the number followed by the '/' at the end of  IPv4 address.  It defines how many bits of the 32-bit IPv4 address are fixed for the network, leaving the remaining bits to be assigned to individual devices.

---

## Subnets

### What I did in this step

In this step, I will create a public subnets so I can start planning where different resources will live and operate.

### Creating and configuring subnets

Subnets are used to cluster resources within a Virtual Private Cloud (VPC) based on network security and traffic flow  There are already subnets existing in my account, one for every Avaliablility Zone

### Public vs private subnets

The difference between public and private subnets are public subnets are used to aloow reousrces inside the subnet to communicate with external network, while a private subnet is used for internal resoucrces that do not need to communicate to other netwrks. For a subnet to be considered public, it has to be connected to an internet gateway.

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-vpc_157c4219)

### Auto-assigning public IPv4 addresses

Once I created my subnet, I enabled auto-assign public IPv4 option. This setting makes sure any EC2 instance launched in that subnet would instantly get a public IPv4 address so that I do not have to create on manually.

---

## Internet gateways

### What I did in this step

In this step, I will connect my VPC to an internet gateway.

### Setting up internet gateways

Internet gateways are the bridge that connects between the subnet and the internet.

Attaching an internet gateway to a VPC means resources in the VPC can now access the internet. If I missed this step, an application hosted inside this VPC would not be avalilable to users on the internet.

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-vpc_4ae90410)

---

## VPC Traffic Flow and Security


### What is Amazon VPC?

Amazon Virtual Private Cloud (Amazon VPC) is a logically isolated section of the AWS cloud where you can launch your computing resources. It acts as your own private virtual network, giving you complete control over IP address ranges, subnets, routing tables, and network gateways.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to create route tables, create network ACL to control traffic incoming and outging from my subnet and setup security groups to protect EC2 instance living inthe subnet.


### Duration to complete

This part took me 30 minutes to complete.

---

## Route tables

Route tables a set of rules (called routes) that determine where network traffic from your subnet or gateway is directed. 

Routes tables are needed to make a subnet public because subnets cannot access the outside world by default. A route table directs traffic by specifying rules, acting as a map that tells data packets where to go. To make a subnet public, you must configure a route table that directs all internet-bound traffic (0.0.0.0/0) to an Internet Gateway.

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-security_0a07b191)

---

## Route destination and target

tha destination is the final IP address range the traffic is intended for, while the target is the next step—or "next hop"—where the router should send that traffic to get it closer to its goal.

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0 and a target of igw-0769cb0eccd72a85c

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-security_0a07b191)

---

## Security groups

Security groups are set of rules that controls that traffic coming in or out of a VPC.

### Inbound vs Outbound rules

Inbound rules are set of rules that control incomming traffic. I configured an inbound rule that allow all allows any IP address to access your resource. I have done this by setting inbount rule source to 0.0.0.0/0. This is a risky choice, but for the purpose of this project, I will allowall traffic to flow in from HTTP requests.

Outbound rules are rules that control the traffic leaving the VPC. By default, my security group's outbound rule is set to allow all outbound traffic

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-security_92b0b0b4)

---

## Network ACLs

Network ACLs act as a secondary, stateless virtual firewall for your subnets. They evaluate and filter traffic entering or leaving a subnet based on a numbered, sequential set of allow and deny rules

### Security groups vs. network ACLs

The difference between a security group and a network ACL is that Network ACLs are used to set broad traffic rules that apply to an entire subnet. For example, blocking incoming traffic from a particular range of IP addresses or denying all outbound traffic to certain ports.
Security groups allow for more granular control, managing access to individual resource. You can specify which ports and protocols are allowed for each connected resource.

---

## Default vs Custom Network ACLs

### Similar to security groups, network ACLs use inbound and outbound rules

By default, a network ACL's inbound and outbound rules will allow all inbound and outbound traffic by default.

In contrast, a custom ACL’s inbound and outbound rules are automatically set to allow all inbound traffic with rule number 100. This is default number cloud practioners use incase they need to add an additional rule that needs to be evaluated first, which will then be given a smaller nymber.

![Image](http://nextwork.ai/charmed_brown_innocent_seal/uploads/aws-networks-security_4faeb056)


