# AWS Services used

Objective:

- Understand the components of BFD services

TOC:

- Basics of IP Protocol
- VPC - Virtual Private Cloud
- Subnet - Public & Private Subnets, Route table
- Security Groups
- Elastic Load balancing -
  - what is a load balancer?
  - how does the load balancing happen? -> Which method is used?
  - Why we choose application load balancer?
- Elastic Container Service
- Elastic Cache

---

## Basics of Internet protocol - 1

IP Protocol is responsible for 2 things

- Addresses
- Routing IP Packets

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

NACL

- Same type of inbound and outbound rules but we can specify allow deny rules which makes it stateless
- It also has priority ordering

---

VPC Endpoints
if we have something like S3 that needs to be connected by a private resource, we can have a s3 endpoint so the routing is within AWS network

Other features of VPC

- VPC Peering - Connect different VPC - Account independant
- AWS Private Link - Different mechanism to expose specific resource instead of everything
- CLient VPN - Logs u into the VPC can be accessed as within the network after login

---

Elastic Load balancer

- Load balancer distributes traffic across EC2 instances

REsource: <https://youtu.be/3FumWkHSusY?feature=shared>
