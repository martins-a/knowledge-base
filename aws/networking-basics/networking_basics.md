# AWS Networking

- VPC: private network to isolate resources - "fence" around resource
- Subnets: A smaller network inside your VPC - normally you have a private subnet and a public subnet [check image]
- You *must* have a subnet to create resources inside
- Route table: In AWS, a Route Table determines where network traffic should go inside a VPC. [check image]

## Creating a VPC

- Create VPC -> name it -> set cidr -> create vpc

## Creating a Subnet

- Create subnet -> select the VPC -> select availibility zone -> CIDR block


_tip_ : normally people create public and private subnets in different availability zones

## Adding a EC2 instance to the network

- Launch a EC2 instance -> instance type -> key pair -> select VPC + subnet -> Create a security group -> 

_tip_: In AWS, an EC2 Key Pair is used to securely access your EC2 instance, typically through SSH (for Linux instances).

_tip_: a Security Group acts as a virtual firewall that controls which network traffic can enter or leave your resources, such as EC2 instances

### CIDR Blocks

- For example, if you add the following: 
	-> 10.0.0.0/24 - you can only change the last block 
	-> 10.0.0.0/16 - you can change the last 2 blocks
	-> 10.0.0.0/8 - you can change the last of 3 blocks
	-> 10.0.0.0/32 - In CIDR (Classless Inter-Domain Routing) notation, /32 means that all 32 bits of an IPv4 address are used to identify the network, leaving 0 bits for hosts.
	
#### What is the purpose of /32?

	- Restricting access to a specific IP (Security)
	- For example, suppose you have an EC2 instance and want to allow SSH access only from your office's public IP.
		
		```
		resource "aws_security_group_rule" "ssh" {
		  type              = "ingress"
		  from_port         = 22
		  to_port           = 22
		  protocol          = "tcp"
		  cidr_blocks       = ["203.0.113.10/32"]
		  security_group_id = aws_security_group.example.id
		}
		```

## How to a make a subnet public?

- You need a gateway
- A gateway connects you to another network
- internet gateway is one type of gateway
- attach to a vpc
- give the subnet a route to connect to the gateway
- add a route table and bind 0.0.0.0/0 to the igtw

### Adding the route table

- VPC -> Route tables -> select VPC -> create it
- Edit subnet associations -> Bind route table to subnet
- Edit route and bind 0.0.0.0/0 to the internet gateway

_tip_: main route table is automatically created
_tip_: any subnet that don't have a subnet association, defaults to the main route table
_tip_: good pratice is to have a specific route table for public and private subnet
_tip_: 0.0.0.0/0 : In AWS Route Tables, 0.0.0.0/0 means all IPv4 addresses - It is commonly used as the default route

# Reference

- https://www.youtube.com/watch?v=2doSoMN2xvI
- https://youtu.be/2doSoMN2xvI?t=1080