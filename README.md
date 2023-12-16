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
