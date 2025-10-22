# VaCT

Virtualizacne a Cloudove Technologie

[AWS Academy](https://awsacademy.instructure.com/courses/138173/)

## Cloud Concepts Overview

> AWS M1

**Cloud computing**

- On-demand service (compute power, database, storage, applications, ...)
- Pay-as-you-go
- Via internet

Enables you to stop thinking of infrastructure as hardware, and let's you think of it as software  
It's more flexible, less upfront cost, can change quickly, don't have to think about maintenance, less time consuming

Cloud service models

- IaaS - Infrastructure as a Service - most control over IT resources - AWS EC2 Virtual Servers
- PaaS - Platform as a Service - DB
- SaaS - Software as a Service - MS office in browser, email, dropbox - least control over IT resources

Cloud Computing Deployment Models

- Cloud - whole app deployed in provider's cloud
- Hybrid - connecting cloud to existing infrastructure
- On-premises (private cloud) - just private hardware, our infrastructure

Advantages of cloud computing

- Trade capital expenses for variable expenses
  - Capital expenses - you need to pay for whole datacenter, whenever you will use 100% of it or not
  - Variable expenses - you only need to pay for what you use
  - You will have it (almost) instantly, no need to wait for days/weeks
  - ?
    - Capex - Capital Expenses - kapitalove vydavky - kupim a mam to - server, priestory, ... (v cloude v podstate neexistuje)
    - Opex - Operational Expenses - plati sa priebezne - zamestnanci
- Higher economy of scale - multiple users pays for cloud, not only you - lower pay-as-you-go prices
- Stop guessing capacity
  - Underestimating/overestimating
- Increase speed - spin up virtual machine in cloud in minutes, instead of waiting for servers to ship (day/weeks/months)
  - Go global in minutes - deployment in multiple regions around the world
- Stop spending of maintenance

### Introduction to Amazon Web Services

Web Service is any piece of software that makes itself available over the internet and uses standardized format (XML, JSON) for the request and the response of an API interaction

**AWS** provides wide set of cloud-based products  
On-demand access to compute, storage, network, database and more  
Offers flexibility - configuration, on-demand update, auto scalability, ability to shut it down  
You for what you use, when you use it  
Services work together like building blocks

Categories

- Compute services (orange color) - Amazon EC2, AWS Lambda, AWS Elastik Beanstalk, Amazon Lightsail, ...
- Storage services (green color) - Amazon S3, Amazon S3 Glacier, Amazon EFS, ...
- Security services (red color) - AWS IAM, Amazon Cognito, AWS Shield, AWS KMS, ...
- Database services (blue color) - Amazon RDS, Amazon DynamoDB, Amazon Redshift, Amazon Aurora, ...
- Networking services (purple color) - Amazon VPC, Amazon Route 53, Amazon Cloudfront, Elastic Load Balancing, ...
- Management and Governance services (pink color) - AWS Trusted Advisor, AWS Cloudwatch, AWS CloudTrail, ...
- AWS Cost Management Services (green color) - AWS Cost & Usage Report, AWS Budgets, AWS Cost Explorer, ...

Ways to manage/interact with AWS

- AWS Management Console - Web GUI
- AWS CLI
- SDKs - Access through code

AWS Cloud Adoption Framework (AWS CAF)

- Moving to AWS Cloud
- Provides guidance and best practices to help organizations build successful cloud
- Organized into 6 areas of focus - called _perspectives_
- First 3 focus on people/business capabilities, last 3 focus on technical capabilities
- Perspectives
  - Business
  - People
  - Governance
  - Platform
  - Security
  - Operations
- Each perspective consists of _capabilities_

## AWS Global Infrastructure Overview

> AWS M3

AWS **Region** is a geographical area  
List of [AWS Regions](https://infrastructure.aws)  
AWS Region consists of two or more **Availability zones**, typically 3  
Availability zone consists of one or more **data centers**

AWS Regions are isolated, to achieve fault tolerance  
When you have data in one Region, it's not automatically replicated to other regions  
Some regions have restricted access (China, government)  
Not all AWS Services are available in all Regions

Example - Region _eu-west-1_, availability zones _eu-west-1a_, _eu-west-1b_, _eu-west-1c_

Regions are usually several kilometers apart, while Availability Zones are typically within 100km of each other  
Availability Zones are interconnected with high speed dedicated network, allowing apps/data to be simultaneously replicated on more zones for redundancy (tornadoes, floods, ...)

Customer can choose availability Zone, and it's recommended to choose more for resilience  
Customer can not choose data center  
Data centers usually consists of 50-80 thousand physical servers

Another step is/are **Points of presence**  
Points of presence include **Edge locations**, **Regional Edge caches**

Amazon Route 53 - DNS

AWS Infrastructure features

- Elasticity and flexibility - resources dynamically adjust to meet requirements
- Fault-tolerance - redundancy
- High availability

### AWS Services

#### AWS Storage services

Green color

Examples

- Amazon Simple Service Storage (S3)
- Amazon Elastic Block Store (EBS)
- Amazon Elastic File System (EFS)
- Amazon Simple Service Storage Glacier - long term archiving, low cost,

#### AWS Compute services

Orange color

Examples

- Amazon Elastic Compute Cloud (EC2) - virtual machines
- Amazon Elastic Compute Cloud Auto Scaling
- Amazon Elastic Container Service (ECS) - Docker
- Amazon Elastic Kubernetes Service (EKS) - Kubernetes
- AWS Elastic Beanstalk - Web applications
- AWS Lambda - run code? only paying when code is running
- AWS Fargate - something with containers also

#### AWS Database services

Blue color

Examples

- Amazon Relational Database Service (RDS)
- Amazon Aurora - DB by Amazon, compatible with MySQL and PostgreSQL
- Amazon Redshift
- Amazon DynamoDB - NoSQL

#### AWS Networking services

Networking and Content Delivery
Purple color

Examples

- Amazon Virtual Private Cloud (VPC)
- Elastic Load Balancing
  -Amazon CloudFront - Content Delivery Network (CDN) - delivers data, videos, apps, APIs
- AWS Tranzit Gateway
- Amazon Route 53 - DNS
- AWS Direct Connect -
- AWS VPN

#### Security services

Security, Identity and Compliance category
Pink color

Examples

- AWS Identity and Access Management (IAM) - manage access to AWS securely, can create users and groups, permissions
- AWS Organizations - restrict what services and actions are allowed in your accounts
- Amazon Cognito - controls to sing-up, sign-in, access and control web and apps
- AWS Artifact - on-demand access to AWS Security and more
- AWS Key Management Service (KMS) - create and manage keys, control encryption
- AWS Shield - DDoS protection

#### Cost Management service category

Green color

Examples

- AWS Cost and Usage Report
- AWS Budgets - create custom budgets, notify when exceeded
- AWS Cost Explorer - visualize expenses

#### Management and Governance services

Pink color

Examples

- AWS Management Console - WebUI access
- AWS Config - track resources and changes
- Amazon CloudWatch - monitor resources and applications
- AWS Auto Scaling - scale mutliple resources
- AWS CLI
- AWS Trusted Advisor - helps you optimize performance and security
- AWS Well-Architected Tool - help in reviewing and improving workloads
- AWS CloudTrail - tracks user activity and API usage

## Cloud Economics and Billing

> AWS M2

Three fundamental drivers of cost

- Compute - CPU, GPU, ... per hour/minute/second, can be different in different regions
- Storage - typically per GB per time
- Data transfer - typically only paying for outbound, typically per GB

**Pay for what you use** - no upfront expenses

**Pay less when you reserve**

- EC2 instances
- On Demand
- Pay more upfront to pay less overall
- NURI - No Upfront Reserved Instance - smaller discount
- PURI - Partial Upfront Reserved Instance - lower discount
- AURI - All Upfront Reserved Instance - largest discount

**Pay less when you use more** - the more GB you use, the less the price is per GB

**Pay less when AWS grows** - the price of AWS never grew, it lowered 75 times between 2006 and 2019

Custom pricing - if none of provided plans are good for you

AWS Free Tier - multiple services free for 1 year, be careful

Services with no charge

- Amazon VPC - network
- Elastic Beanstalk\* - web server
- Auto Scaling\*
- AWS CloudFormation\* - creating new instances?
- AWS Identity and Access Management (IAM) - v podstate AAA
- ...and more...

\* There might be charges associated with other AWS services that are used with these services - for example Auto Scaling is free, but you will pay for new instances

### Total Cost of Ownership (TCO)

The financial estimate to help identify direct and indirect costs of a system

It consists of:

- Server Costs - server, rack, PDU, switches, OS, virtualization licenses
- Storage Costs - disks, RAID, administration
- Network Costs - LAN switches, load balancers, administration
- IT Labor Costs - administration
- +space, power, cooling

[AWS Pricing Calculator](https://calculator.aws/#/)

### AWS Organizations

Free account management service  
Consolidates multiple regular AWS accounts into an organization  
For large organizations

Benefits

- Centrally managed access policies across multiple AWS accounts
- Controlled access to AWS services
- Automated AWS account creation and management
- Consolidated billing across multiple AWS accounts

Hierarchic design  
Root -> Organizational Unit (OU) -> AWS Account (actual user, program with API, ... with access rights = policies)  
OU can have either AWS accounts as childs, or also another OUs  
Everything can have at least one parent
If you apply a policy to one OU, it applies to all child OUs/AWS accounts

Accessing via Console, AWS CLI, SDKs or HTTPS Query API

### AWS Billing and Cost management

Free tools

AWS Cost and Usage Report  
AWS Billing Dashboard  
AWS Budgets  
AWS Cost Explorer

### AWS Technical support

Support plans

- Basic support - free, basically no support
- Developer plan - only low severity (24h) and normal severity (12h)
- Business plan - +high (4h) and urgent (1h)
- Enterprise - +critical (15min)

AWS Trusted Advisor - gives hints what to do for the service to be better/cheaper/more redundant/...

## AWS Cloud Security

> AWS M4

### AWS Shared Responsibility Model

Who is responsible for what

- Amazon - security _of_ cloud - responsible for SW and HW
- You - security _in_ cloud - responsible for client-side data encryption, firewall config, access management, ...

### AWS Identity Access Management (IAM)

Allows you to control access to compute, storage, database and application services in the AWS cloud

Essential components

## Networking and Content Delivery

> AWS M5

### Amazon Virtual Private Cloud (VPC)

Siet  
Ekvivalent jednej LAN  
Logically isolated section of the AWS Cloud  
Inside one Region, isolated from Availability Zones  
IPv4 and IPv6 support  
Security Groups, ACLs  
Dedicated to AWS Account

VPC is in one Region only, can span over multiple Availability Zones
Consists of subnets - subnet belongs to a Availability Zone  
Subnets are classified as public or private, not the same as public/private in normal networks - private does not have _direct_ access to outside network

Each VPC must have assigned IPv4 CIDR block from private range, masks `/16` to `/28`  
Subnets cannot overlap  
_There is no possibility to change the address range after it's created_  
IPv6 is also supported, but not so used yet

There are some reserved addresses - for example in `10.0.0.0/24` range

- `10.0.0.0` - Network address
- `10.0.0.1` - Internal communication - basically gateway
- `10.0.0.2` - DNS
- `10.0.0.3` - Future use
- `10.0.0.255` - Broadcast

Public IPv4 address can be assigned manually  
Always through NAT - usually `1:1` = Elastic \_  
Bonded with AWS Account  
Additional cost

**Elastic Network Interface**  
Like external NIC  
You are always connected through this, not directly  
You can attach/detach/reattach it to/from an instance to redirect network traffic

Each subnet has its own routing table  
You have exactly one routing table in a subnet  
It contains at least one route - local - cannot be deleted

Elastic IP - Static Public IP

### Networking

Internet Gateway

- Router that has NAT
- To make a subnet public, attach VPC to GW
- Add a default route to access non-local networks

NAT Gateway

- NAT, that I can manage
- Is different from Internet Gateway
- Like another point between GW and VPC

VPC Sharing

- Sharing one or more subnets with other AWS Account in the same Organization (not between different Organizations)

VPC Peering

- Connects two VPCs under the same Account
- Can connect two networks in different regions
- As if they were in the same network
- Only two VPCs
- IPs cannot overlap

Site-to-Site VPN Connection

- Virtual GW instead of Internet GW
- Permanent VPN between Routers - Virtual GW and your home/company router

Direct Connect (DX)

- Dedicated private network connection
- Physically connect AWS with you
- Glass fiber
- Uses `dot1q` VLAN
- There are like 10 of these in Central Europe

VPC Endpoints

- Connect something else (S3 bucket) from Amazon to your VPC
- S3 can be in different region
- Connect S3 to your Elastic Network Interface via Endpoint
- Two types
  - Gateway
  - Interface

Transit Gateway

- Something like Peering, but for multiple VPCs - like a star topology

### VPC Security

Security Groups

- Virtual Firewall
- Works on _Instance level_, not on whole Subnet
- Like give your EC2 a security group
- Can go up to L4 - IP addresses and Ports
- Inbound, Outbound - from the view of Instance
- Default - deny all inbound (except initiated from inside), allow all outbound
- Stateful
- You can only permit, you cannot deny - everything is denied by default
- You can have multiple of these on one instance

Network ACLs

- Standard ACL (as on Cisco)
- Workds at _Subnet level_
- Inbound/Outbound
- Stateless
- Default - permit any
- Evaluated in order
- Return traffic has to be explicitly allowed
- One subnet can has only one ACL

### Amazon Route 53

DNS

Traffic Flow Manipulation

- Simple Routing
- Weighted Round Robin Routing - assign weights to specify the frequency
- Latency Routing - give me closest (in terms of latency)
- Geolocation routing - route traffic based on location of your users
- Geoproximity routing - route traffic based on location of your resources (server, service, ...)
- Failover Routing - use backup if main fails
  - You have to configure backup
  - Based on - request interval, treshold, ...
- Multivalue Answer Routing - give multiple answers, client chooses

### Amazon CloudFront

CDN - Content Delivery Network  
Videos, Photos, ... - Netflix for example  
Globally distributed system of caching servers  
Buffer = Point of Presence  
As close to customer as possible  
Edge locations - connect between Amazon and the world  
Pay only for outbound  
Charged for the number of HTTP(S) requests + transfer

## Storage

> AWS M7

Zelena farba

### Amazon EBS

Elastic Block Store  
Persistent block storage volumes for use with EC2 instances  
Just like a drive on PC/laptop, or as USB drive  
Logically as directly connected  
Persistent - data remains after power off  
Automatically replicated within Availability Zone  
High availability and durability

Create individual storage volumes, attach to one instance

Use as

- Boot volume
- Data storage
- Database
-

Types

- SSD
  - General Purpose (`gp2`)
  - Provisioned IOPS (IO per second)
- HDD
  - Throughput-Optimized
  - Cold

Max `16TiB`

Features

- Snapshots
- Encryption
- Elasticity

Pricing

- Independent from instance
- Per month
- Inbound is free, Outbound across Regions is paid

Block vs Object storage

- Block - classic drive, file system, you can change one block
- Object - when you want to change something, you change the whole - similarly to `zip` file

### Amazon S3

Simple Storage Service  
Zeleny kyblik  
Object-level storage  
Data is stored as objects in buckets  
Single object max `5TB`  
Durability - 11 9s = 99.999999999% of time  
Availability - 4 9s = 99.99% = ~52 minutes per year = ~8 seconds per day

Types

- S3 Standard
- S3 Standard Infrequent Access - 3 9s availability
- S3 Intelligent-Tiering - changes type (\[in]frequent) based on number of request
- S3 One Zone-Infrequent Access
- S3 Glacier - archives, takes long time to access
- S3 Glacier Deep Archive - takes even longer

URLs - two types

Use cases

- Backup and storage
- Application hosting
- Media hosting

Pay for

- GB per month
- Transfer out
- HTTP requests - PUT, COPY, POST, LIST, GET

Lifecycle policies example

- S3 Standard
- S3 Standard IA - after 30 days
- S3 Glacier - after 60 days
- Delete after 365 days

### Amazon EFS

Elastic File System  
Network mapped drive - like Samba (but specifically NFS)  
Access from multiple machines

### Amazon S3 Glacier

Zamrznuty S3 kyblik  
Long-term data archives  
Extremely long cost  
Durability 11 9s

Retrieval options

- Standard = 3-5 hours
- Bulk = 5-12 hours
- Expedited = 1-5 minutes

Use cases

- Archiving of like everything

Using

- RESTful APIs
- Java or .NET SDKs
-

Control access with IAM, Encryption with AES-265, Glacier manages your keys
