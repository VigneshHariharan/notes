# AWS Services used

## Objective

- Understand the each component of BFD services

## Agenda

- AWS Networking Basics
- AWS ELB & ECS

---

## AWS Networking

- VPC
- Public Subnets, Private Subnets
- Internet Gateway, NAT Gateway
- Security Group

These concepts can be easily understood by learning a little bit about IP protocol

---

## Basics of Internet protocol (IP)

What is Internet Protocol?

- In OSI Model, In Networking layer, packets are transferred from one node to another.
- Two things are required for this to happen
  - A way to address nodes
  - Route packets from one router to another

- IP Protocol is responsible for addressing the nodes
- IP Address in IPV4 has 32 bit address process which has a format of a.b.c.d
- a,b,c,d each has 1 byte which is 8 bits, total of 4 byte is 32 bits

---

### NAT

- IP Address needs to be unique for every hosts since it is not possible due to constraint of address spaces, Network Address Translation (NAT) was introduced.
- NAT - Network Address Translation which assigns a router or any machine to acts as intermediary between private and public networks.
- Another way to think about it is NAT translates private IP to a public IP 
- Basically a group of hosts will making connections through NAT so that in public internet all the requests will be coming from a single IP

---

### Subnets

What is it?

- Subnet means a network inside a network
- Subnetting will making routing faster, it can be done by knowing network and host portion of an IP Address
- There are 2 ways to do this
  - CIDR (AWS uses this)
  - Subnet Mask

---

### Classless Inter domain Routing (CIDR)

- In CIDR, /x prefix is used to explicitly say how many bits are used for network portion and host portion
- Format: a.b.c.d/x, x will determine how many bits network portion will occupy from the start
- Example 1: 10.0.2.0/24 means 10.0.2.x is the network portion first 24 bits, and the last .0 is the host portion which means IP address of hosts within the subnet will always be 10.0.2.x and x will be between 0 - 255 which is 8 bit.
- Example 2: 10.0.0.0/16 means 10.0.x.x is the network portion first 16 bits are used for network and remaining 16 is used for hosts.
- /x can be 0 - 32 bits
- But in AWS limit is 16 - 28 bits

---

## What is a VPC?

- Isolated network, where every resource created within AWS except global services must belong to a VPC
- VPC is region specific, for each region we need to have a VPC, By default AWS provides a VPC for every region.
- A VPC is isolated by CIDR blocks
- AWS recommendation for CIDR block is 10.0.0.0/16
- Public IPV4 address cost money, Public IP address needs to be unique outside, private only needs to be unique within VPC

---

### Subnets in AWS

- In AWS, subnet provides further isolation from VPC, it is availability zone specific
- Availability zones (AZ) are like data centers or part of data centers where the AWS services hardware placed in. They are name like us-east-1a, ap-south-1b.
- Each Availability zone can have multiple subnets
- When creating resources in AWS, we can specify subnets to deploy service in multiple availability zones which will make the services highly available.

### Public Subnets

- a subnets need to be connected to a Internet gateway in order for the subnet to be public.
- The subnet can be connected to gateways through route table.

---

### Private Subnets

- A subnet is a private subnet when it is connected to NAT gateway.
- NAT Gateway allows outbound calls to internet but restricts inbound calls from internet


### Route Tables

- Set of rules that contains destination and target where all the routing in the VPC is defined
- 0.0.0.0/0 is public internet when that condition comes in , internet gateway can be in target
- Destination can be specified to subnets

---

### Security Groups

- Granular level control for a resource, like if we need to RDS to be only accessible by one EC2 instance
- Can control both inbound & outbound with type (TCP, HTTP), Protocol, Port Range, Source
- Everything is denied, the things that are allowed need to be specified
- Requests are stateful they are bidirectional which means if I get an inbound request, it is assumed that the outbound request is also allowed, For example IP 10.0.1.12 if added in inbound rules, outbound will also be allowed for it

---

## Elastic Load balancer (ELB)

- Load balancer accepts requests from clients then distributes traffic across multiple targets in different availability zones
- There are different kinds of load balancers:
  - Application Load balancer (layer 7)
  - Network Load balancer (layer 4)
- It also monitors targets via health checks, when a target is unhealthy requests are distributed in remaining targets

---


## Elastic Load balancer (ELB) (Continued)

- When AZ is enabled, there will be a load balancer node for every AZ
- Round robin algorithm is used by default: Basically all the targets will receive the same number of requests
- Load Balancer can be internet facing or internal, the difference is DNS server, in private DNS will serve it form awsdomain.com so the IP address won't be resolved in public
- Service Discovery is the mechanism where load balancer decides where servers are available which instances your requests should go to.

---

## Elastic Container Service (ECS)

### What is containerization?

- Bundle up your software with it's dependencies so it can run on any hardware but must share same OS kernel
- Container - application is in execution state
- Image - Packaged Software

### What is ECS?

- Fully Managed container orchestration service
  - ECS will create new containers and delete containers when there is a problem with it
  - It also reverts back to your last working container version, if there is a problem with deployment
  - It also scales up the number of containers, allocates the specified resource provided

---

### How it affects resource allocation?

Resource allocation issue comes when scaling applications

There are 2 kinds of scaling: Vertical Scaling, Horizontal Scaling

- Vertical Scaling means improving capacity of one node and supposed to serve request faster
- Horizontal scaling is scaling number of number of nodes available and support serve more requests

- Traditionally with EC2 instances, Vertical scaling will mean increase the RAM, memory, storage of a single VM.
- Now there are 2 conditions to handle, the EC2 instance should handle peak load and regular traffic as well.


---


### How it affects resource allocation? (Continued)

- To handle both peak load and regular traffic, EC2 instance will need to be scaled up to handle peak load capacity, But on regular traffic the resources won't be fully utilized.
- Here resources mean CPU, Memory, GPU, Network Bandwidth, Storage
- Example: One node server utilizes 30% of CPU on regular load, but utilization on peak load is 80%
- To solve this issue, 2 instances of server can be running in same EC2 instance to increase resource utilization, but when there is higher load received on both instances, both of them will be competing for resources
- This is where containerization solves resource allocation issue, ECS can specify how much resource a container needs, so the hardware can be utilized efficiently which will reduce the costs.

---

### What is Fargate?

- Fargate is one of the infrastructure providers for ECS
- ECS can run containers in EC2 machines as well, but in EC2 you need to choose what kind of image, hardware capacity, and manually do any security updates
- But Fargate Service creates MicroVM for exactly the resource specified like only one 2 CPU's with 6 GB memory
- Fargate also spreads out containers in multiple availability zones which means if there are 3 containers, all of the them will be in different availability zones based on the subnets provided

---

### How would you deploy a server?

Task - instantion of a task definition
Task Definition - Configuration which contains resource usage of CPU, Memory, Storage with whether it should run on Fargate or EC2

- Create a docker or podman image and deploy in ECR
- Create a Task definition in ECS
- Create a cluster and within the cluster create a service
- Service will be used for Task management for load balancing, auto scaling, deployment.

### How to access the server deployed?

The ECS service can be set as a target for a load balancer. The load balancer will have a DNS entry which can be used to make a request to ECS Service deployed.

---

Resources learned from:

- [VPC](https://youtu.be/3FumWkHSusY?feature=shared)
- [ELB](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
- [Service Discovery](https://youtu.be/3UDJvF0CMkQ?feature=shared)
- [Service Discovery Issue](https://github.com/aws/containers-roadmap/issues/343)
- [ECS-Scalability](https://containersonaws.com/presentations/amazon-ecs-scaling-best-practices/)