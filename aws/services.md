# AWS Services used

Objective:

- Understand the each component of BFD services

TOC:

- Basics of IP Protocol
- VPC - Virtual Private Cloud
- Subnet - Public & Private Subnets, Route table
- Security Groups
- Elastic Load balancing 
- Elastic Container Service

---

## Basics of Internet protocol - 1

IP Protocol is responsible for 2 things

- Address Space
- Routing 

IP Address in IPV4 has a.b.c.d/x format example, 10.0.0.0/16 , a,b,c,d each has 1 byte which is 8 bits, total of 4 byte is 32 bits
when /x comes in 0 - 32 , In AWS the limit is 16 - 28
The /x differentiates the network and host, here host are the servers. It is also called a subnet
All the host in the network will have the same network portion
Since there is a limitation on address spaces, NAT - Network Address Translation which assigns a router or any machine to acts as intermediary between private and public networks. Basically a group of hosts will making connections through NAT so that in public internet all the requests will be coming from a single IP

---

## Basics of Internet Protocol - 2

Subnet masks
To determine whether an IP is in the range of a particular subnet, Subnet masks are used. So we need to know the network portion of a network
TO determine this bits are used like 255.255.255.0 which means first 3 are network portion last one is a host portion, basically can 255.255.252.0 which means there can 1024 hosts within the network

CIDR - Classless Inter domain Routing
Class means that a.b.c.d in mask instead /x prefix is used to explicity say how many bits are used - modernly used
10.0.2.0/24

Default Gateways
If a host is in a subnet and wants to talk outside a gateway is required, A gateway will have it's own IP address. The host should know the gateway's IP Address
If a host wants to talk to another host in same subnet MAC addresses can be used

---

## What is a VPC?

- isolated network, where every resource created within AWS except global services must belong to a VPC
- A VPC is isolated by CIDR blocks

Address Spacing & Routing:
IP Address space or CIDR block, AWS recommendation is 10.0.0.0/16
Public IPV4 address cost money
Public IP address needs to be unique outside, private only needs to be unique within VPC

---

## Route Tables

Set of rules that contains destination and target
0.0.0.0/0 is public internet when that condition comes in , internet gateway can be in target
Destination can be specified to subnets
If an resource wants to get some data outside internet, which is to get inbound calls, it needs to get connected to NAT Gateway
NAT Gateway cost money

---

## Security Groups

- Granular level control for a resource, like if we need to be only by accessible by one resource
- Can control both inbound & outbound with type (TCP, HTTP), Protocol, Port Range, Source
- Everything is denied, the things that are allowed need to be specified
- Requests are stateful they are bidirectional which means if I get an inbound request, it is assumed that the outbound request is also allowed, For example IP 10.0.1.12 if added in inbound rules, outbound will also be allowed for it

---

VPC Endpoints
if we have something like S3 that needs to be connected by a private resource, we can have a s3 endpoint so the routing is within AWS network

Other features of VPC

- VPC Peering - Connect different VPC - Account independent
- AWS Private Link - Different mechanism to expose specific resource instead of everything
- CLient VPN - Logs u into the VPC can be accessed as within the network after login

---

Elastic Load balancer

- Load balancer accepts requests from Clients then distributes traffic across multiple targets in different availability zones
- It also monitors targets via health checks, when a target is unhealthy requests are distributed in remaining targets
- When AZ is enabled, there will be a load balancer node for every AZ
- Round robin algorithm is used by default: Basically all the targets will receive the same number of requests
- Load Balancer can be internet facing or internal, the difference is DNS server, in private DNS will serve it form awsdomain.com so the IP address won't be resolved in public

Service Discovery is the mechanism where load balancer decides where servers are available which instances your requests should go to.

---

Elastic Container Service (ECS)

What is containerization?

- Bundle up your software with it's dependencies so it can run on any hardware but must share same OS kernel
- Container - application is in execution state
- Image - Packaged Software

What is ECS?

- Fully Managed container orchestration service
  - ECS will create new containers and delete containers when there is a problem with it
  - It also reverts back to your last working container version, if there is a problem with deployment
  - It also scales up the number of containers, allocates the specified resource provided

---

How it affects resource allocation?

Resource allocation issue comes when scaling applications

There are 2 kinds of scaling: Vertical Scaling, Horizontal Scaling

- Vertical Scaling means improving capacity of one node and supposed to serve request faster
- Horizontal scaling is scaling number of number of nodes available and support serve more requests

- Traditionally with EC2 instances, Vertical scaling will mean increase the RAM, memory, storage of a single VM.
- Now there are 2 conditions to handle, the EC2 instance should handle peak load and regular traffic as well.

- To handle both peak load and regular traffic, EC2 instance will need to be scaled up to handle peak load capacity, But on regular traffic the resources won't be fully utilized.
- Here resources mean CPU, Memory, GPU, Network Bandwidth, Storage
- Example: One node server utilizes 30% of CPU on regular load, but utilization on peak load is 80%
- To solve this issue, 2 instances of server can be running in same EC2 instance to increase resource utilization, but when there is higher load received on both instances, both of them will be competing for resources
- This is where containerization solves resource allocation issue, ECS can specify how much resource a container needs, so the hardware can be utilized efficiently which will reduce the costs.


What is Fargate?

- Fargate is one of the infrastructure providers for ECS
- ECS can run containers in EC2 machines as well, but in EC2 you need to choose what kind of image, hardware capacity, and manually do any security updates
- But Fargate Service creates MicroVM for exactly the resource specified like only one 2 CPU's with 6 GB memory
- Fargate also spreads out containers in multiple availability zones which means if there are 3 containers, all of the them will be in different availability zones based on the subnets provided

How would you deploy a server?

Task - instantion of a task definition
Task Definition - Configuration which contains resource usage of CPU, Memory, Storage with whether it should run on Fargate or EC2

- Create a docker or podman image and deploy in ECR
- Create a Task definition in ECS
- Create a cluster and within the cluster create a service
- Service will be used for Task management for load balancing, auto scaling, deployment.

How to access the server deployed?

The ECS service can be set as a target for a load balancer. The load balancer will have a DNS entry which can be used to make a request to ECS Service deployed.

---

Resource:
[VPC](https://youtu.be/3FumWkHSusY?feature=shared)
ELB - https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html

[Service Discovery](https://youtu.be/3UDJvF0CMkQ?feature=shared)
[Service Discovery Issue](https://github.com/aws/containers-roadmap/issues/343)

[ECS-Scalability](https://containersonaws.com/presentations/amazon-ecs-scaling-best-practices/)