# aws-solution-architect-associate
## Table of Contents
 - [AWS Cloud](#aws-cloud)
 - [AWS IAM](#aws-iam)
 - [EC2 Basics](#ec2-basics)
 - [EC2 Associate](#ec2-associate)
 - [EC2 Instance Storage](#ec2-instance-storage)
 - [High Availability and Scalability](#high-availability-and-scalability)
 - [RDS Aurora and ElastiCache](#rds-aurora-and-elasticache)
 - [Route53](#route53)
 - [Classic Solutions Architecture](#classic-solutions-architecture)
 - [S3](#s3)
 - [S3 Advanced](#s3-advanced)
 - [S3 Security](#s3-security)
 - [CloudFront and Global Accelerator](#cloudfront-and-global-accelerator)
 - [AWS Storage Extras](#aws-storage-extras)
 - [AWS Integration and Messaging](#aws-integration-and-messaging)
 - [Containers on AWS](#containers-on-aws)
 - [Serverless Overview](#serverless-overview)
 - [Serverless Architecture](#serverless-architecture)
 - [Databases in AWS](#databases-in-aws)
 - [Data and Analytics](#data-and-analytics)
 - [Machine Learning](#machine-learning)
 - [AWS Monitoring Audit and Performance](#aws-monitoring-audit-and-performance)
 - [AWS IAM Advanced](#aws-iam-advanced)
 - [AWS Security and Encryption](#aws-security-and-encryption)
 - [AWS VPC](#aws-vpc)
 - [Disaster Recovery and Migration](#disaster-recovery-and-migration)
 - [More Solutions Architecture](#more-solutions-architecture)
 - [Other services](#other-services)
 - [White Papers and Architectures](#white-papers-and-architectures)

## AWS Cloud
 - AWS Global Infrastructure:
   - AWS Regions: clusters of data centers (Most AWS services are region-scoped)
   - AWS Availability Zones
   - AWS Data Centers
   - AWS Edge Locations /Points of Presence
 - How to choose an AWS Region?
   - Compliance with data governance and legal requirements: data never leaves a region without your explicit permission
   - Proximity to customers: reduced latency
   - Available services within a Region: new services and new features aren’t available in every Region
   - Pricing: pricing varies from region to region and is transparent on the service pricing page
 - AWS Availability Zones: (min-3, max-6 per region, each AZ has one or more discrete data centers (separated))
 - AWS Points of Presence (Edge Locations): Content is delivered to end users with lower latency
 - services:
   - Global: IAM, Route53, Cloudfront, WAF
   - Region(most services): EC2(IaaS), Elastic Beanstalk(PaaS), Lambda(FaaS), Rekognition(SaaS)

## AWS IAM
 - Users and Groups: Root Account, Users and Groups(only contain users)
 - IAM-Permissions: JSON files called policies -- apply the privilege principle, don't give more permissions than a user needs.
 - IAM policies inheritance: users who belong to multiple groups can inherit multiple permissions
 - IAM policies structure: version(required), id(optional), statement(required)
   - statement: Sid(statement id--optional),Effect(allow/deny), Principal(to whom this policy applied to), Action(list of allow/deny), Resource(list of resources the actions applied to), Condition(conditions for when this policy takes effect)
 - IAM password policy
   - strong password = high security
   - password policy:
     - min length
     - specific char types
     - allow all iam users to change their passwords
     - password expiration
     - prevent password re-use
 - Multi-Factor Authentication - MFA: Password + MFA = Successful Login
 - How can users access AWS?
   - AWS Console (password + MFA)
   - AWS CLI(access key): alternative to AWS Console
   - AWS SDK(access key)
   - access key id & access key secret (don't share access keys)
 - IAM Roles for Services: an IAM role contains IAM permissions will be assigned to certain AWS services to perform some actions
 - IAM Security Tools:
   - IAM Credentials Report (account-level): a report that lists all your account's users and the status of their various credentials
   - IAM Access Advisor (user-level): Access advisor shows the service permissions granted to a user and when those services were last accessed. You can use this information to revise your policies.
## EC2 Basics
 - EC2 = Elastic Compute Cloud = Infrastructure as a Service
 - EC2: renting virtual machine (EC2), storing data on virtual drives(EBS), distributing traffic across machines(ELB), Scaling (ASG)
 - EC2 sizing & configuration options:
   - OS: Linux, Windows, MacOS
   - CPU & RAM
   - storage: instance store(hardware), network-attached (EBS&EFS)
   - Network card: speed of the card, public IP
   - Firewall rules: security group
   - Bootstrap script (configure at first launch): EC2 User Data
     - launching commands when an EC2 instance starts (only first start, run once)
     - install software, updates, download files from internet or something else
     - user data script runs with root user
 - EC2 instance type:
   - naming convention: m5.2xlarge (m instance class, 5 generation, 2xlarge size within the instance class)
   - general purpose: great for a diversity of workloads such as web servers or code repositories, which strike the balance between compute, memory, and network
   - compute optimized: great for compute-intensive tasks that require high-performance CPUs: Batch processing, media transcoding, gaming server, machine learning.
   - memory optimized: great for processing large data sets in memory: real-time processing big unstructured data, BI, high-performance database
   - storage optimized: great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage: Data warehousing applications, High frequency online transaction processing (OLTP) systems.
 - Security Groups(stateful, NACL--stateless):  fundamental of network security in AWS,  control how traffic is allowed into or out of our EC2 Instances.
   - only contain rules
   - Security groups rules can reference by IP or by security group
     - ports
     - IP ranges - IPV4 / IPV6
     - inbound / outbound
   - can be attached to multiple instances
   - locked down to region/VPC
   - live outside of EC2
   - good to maintain a separated security group for SSH
 - Classic Ports: 22-SSH, 21-FTP, 22-Sftp, 80-HTTP, 443-HTTPS, 3389-RDP (Remote Desktop Protocol) – log into a Windows instance
 - SSH: one of the most important function. It allows you to control a remote machine, all using the command line.
 - EC2 Instance Connect: no need for access key file, open within a browser, a temporary key will be used (only works with aws linux2), and make sure to open port 22
 - EC2 Instances Purchasing Options:
   - On-Demand Instances – short-term and un-interrupted workload, predictable pricing, pay by second and for what you use
   - Reserved (1 & 3 years): 
     - Reserved Instances – long workloads, save up to 72% compared to on-demand , instance's scope -- regional or zonal, long-term and steady workloads, such as database
     - Convertible Reserved Instances – long workloads with flexible instances, save up to 66% compared to on-demand (can change instance type, family, OS, scope and tenancy)
   - Savings Plans (1 & 3 years): save up to 72%, locked down to instance family and region – a commitment to an amount of usage, long workload
   - Spot Instances: save up to 90%, most cost-effient – short workloads, cheap, can lose instances (less reliable), great for workloads that are resilient to failure(such as batch job, data analysis, any distributed job, image processing, etc), but not for critical jobs
   - Dedicated Hosts – book an entire physical server, control instance placement, allows to address compliance requirements and use existing server-bound software licenses. Most expensive, on-demand/reserved(1 or 3 yrs)
   - Dedicated Instances – no other customers will share your hardware, but may share hardware with other instances in the same account. The hardware is dedicated to you, no control over instance placement.
   - Capacity Reservations – reserve on-demand instances capacity in a **specific AZ** for any duration
 - Which purchasing option is right for me?
   - On-demand: coming and staying in resort whenever we like, we pay the full price
   - Reserved: like planning ahead and if we plan to stay for a long time, we may get a good discount.
   - Savings Plans: pay a certain amount per hour for certain period and stay in any room type (e.g., King, Suite, Sea View, …)
   - Spot instances: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time
   - Dedicated Hosts: We book an entire building of the resort
   - Capacity Reservations: you book a room for a period with full price even you don’t stay in it
 - EC2 Spot Instance Requests: define **max spot price** , when **current spot price** > **max spot price**, can choose to **stop** or **terminate** with 2 min grace period. Or **Spot Block** to **block** the instances during a specified time frame with interruptions.
 - How to terminate Spot Instances? you must first cancel a **spot request**, and then terminate the associated spot instances.
 - Spot Fleets: a set of spot instances + (optional)on-demand instances
   - trying to meet the target capacity with price constraints(define launch pools)
   - strategies to allocate spot instances: lowestPrice, diversified, capacityOptimized, priceCapacityOptimized(recommended)
   - Spot Fleets allow us to automatically request Spot Instances with the lowest price
## EC2 Associate
## EC2 Instance Storage
## High Availability and Scalability
## RDS Aurora and ElastiCache
## Route53
## Classic Solutions Architecture
## S3
## S3 Advanced
## S3 Security
## CloudFront and Global Accelerator
## AWS Storage Extras
## AWS Integration and Messaging
## Containers on AWS
## Serverless Overview
## Serverless Architecture
## Databases in AWS
## Data and Analytics
## Machine Learning
## AWS Monitoring Audit and Performance
## AWS IAM Advanced
## AWS Security and Encryption
## AWS VPC
## Disaster Recovery and Migration
## More Solutions Architecture
## Other services
## White Papers and Architectures
