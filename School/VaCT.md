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
- Amazon CloudFront - Content Delivery Network (CDN) - delivers data, videos, apps, APIs
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
