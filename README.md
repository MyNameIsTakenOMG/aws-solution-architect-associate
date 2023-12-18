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
 - Private vs public IP (IPV4)
   - public IP: machine can be identified on the internet, meaning it's unique across the internet and can be geo-located easily.
   - private IP: machine can only be identified on a private network, meaning it's unique across the private network, connecting to public internet via NAT and internet gateway. Besides, only a specified range of IPs can be used as private IPs
 - Elastic IP: a fixed public IP for your EC2 instance, can only be attached to one instance at a time.
   - can only have up to 5 Elastic IPs in your account (ask aws to increase)
   - can quickly remap the address to another instance when another was failed
   - Overall, try to avoid using Elastic IP: 1)often reflect poor architectural decisions. 2)instead, use a random public IP with a registered DNS name. 3)or using a load balancer.
 - by default, each EC2 instance has a public IP and a private IP
 - Placement Groups:
   - strategies: 1)Cluster—clusters instances into a low-latency group in a single Availability Zone. 2)Spread—spreads instances across underlying hardware (max 7 instances per group per AZ). 3)Partition—spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
 - Elastic Network Interfaces (ENI): Logical component in a VPC that represents a virtual network card. Can be created independently and attach them on the fly on EC2 instances for failover. ENI is bound to specific AZ.
   - Primary private IPv4, one or more secondary IPv4
   - One Elastic IP (IPv4) per private IPv4
   - One Public IPv4
   - One or more security groups
   - A MAC address
 - EC2 Hibernate: 1)the RAM state is written into root EBS(must be encrypted). 2)long-running processing, services that take time to initialize
   - the instance RAM size must be less then **150GB**
   - cannot be in the state of hibernate more than **60 days**
   - available for **on-demand, spot** instance
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
 - Rekognition: face detection, content moderation(Set a **Minimum Confidence Threshold** for items that will be flagged. Flag sensitive content for manual review in
Amazon Augmented AI (A2I)), labeling, celebrity recognition -- detect objects, people, text, and scenes in images or videos
 - Transcribe:  convert speech to text (ex: subtitles). 1)Uses a deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately. 2)Automatically remove Personally Identifiable Information (PII) using Redaction. 3)Supports Automatic Language Identification for multi-lingual audio.
 - Polly: text to lifelike speech using deep learning. **Lexicon(Customize the pronunciation of words with Pronunciation lexicons) &  Speech Synthesis Markup Language (SSML, more customization)**
 - Translate: translations,  localize content
 - Lex: build conversational bots – chatbots
 - Connect:  cloud-based virtual contact center
 - Comprehend: natural language processing(NLP). Analyzing text to find insights and relationships. **Amazon Comprehend Medical**: detects and returns useful information in
unstructured clinical text. Uses NLP to detect **Protected Health Information (PHI)** – DetectPHI API.
 - SageMaker: machine learning for every developer and data scientist, easy-to-train models.
 - Forecast: build highly accurate forecasts -- feeding data and then training the model, last using the model to predict
 - Kendra: ML-powered search engine -- extract and index contents from documents of different formats, and then make the results indexable.
 - Personalize: real-time personalized recommendations, integrates into existing websites, apps, SMS, email marketing system
 - Textract: detect text and data in any scanned documents of any type
## AWS Monitoring Audit and Performance
 - Amazon CloudWatch Metrics: metrics for every service in AWS. Metrics belong to namespaces. Dimension is an attribute of a metric(up to 30). Metrics have timestamps. Custom metrics can be created.
 - CloudWatch Metric Streams: Continually stream CloudWatch metrics to a destination of your choice, with **near-real-time** delivery and low latency (Kinesis Data Firehose, or 3rd party services, such as datadog). Optionally, **filter metrics** to only stream a subset of them.
 - CloudWatch Logs: Log groups representing an application. log stream(instance in apps or containers). destinations: s3, opensearch, lambda, kinesis data streams, kinesis data firehose. Encrypted by default, can be encrypted using custom key.
 - CloudWatch logs sources: SDK, CloudWatch Logs Agent, CloudWatch Unified Agent, Elastic Beanstalk, ECS, AWS Lambda, VPC Flow Logs, API Gateway, CloudTrail, Route53.
 - CloudWatch Logs Insights: Search and analyze log data stored in CloudWatch Logs. Provides a purpose-built query language. Can query multiple Log Groups in different AWS accounts. It’s a query engine, not a real-time engine.
 - CloudWatch Logs – S3 Export: take up to 12 hrs. API call(CreateExportTask). Not near-real-time or real-time… use Logs Subscriptions instead.
 - CloudWatch Logs Subscriptions:  get real-time logs from cloudwatch, and send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda. **Subscription Filter(filter logs)**.
 - CloudWatch Logs AggregationMulti-Account & Multi-Region: [account1/region1...] -> [Subscription Filter...] -> Kinesis Data Streams -> Kinesis Data Firehose -> S3.
 - CloudWatch Logs Subscriptions: Cross-Account Subscription – send log events to resources in a different AWS account (KDS, KDF) (**in this case, we need modify the destination cloudwatch service's access policy to accept the logs from the original cloudwatch service. meanwhile, we need a IAM role to allow kinesis data streams to collect logs**)
 - CloudWatch Logs for EC2: we need **cloudwatch logs agent** running on the EC2 instances with proper IAM permissions.
 - CloudWatch Logs Agent & Unified Agent: logs agent -- old version, can only send logs to cloudwatch. unified agent -- collect and send logs as well as additional system-level metrics such as RAM, CPU. Centralized configuration using SSM Parameter Store.
 - CloudWatch Unified Agent – Metrics : CPU, RAM, DISK metrics, Netstat, processes, swap space(free, used)
 - CloudWatch Alarms: used to trigger notifications for any metrics
   - Various options (sampling, %, max, min, etc…)
   - Alarm States: OK, INSUFFICIENT_DATA, ALARM
   - Period: Length of time in seconds to evaluate the metric (High resolution custom metrics: 10 sec, 30 sec or multiples of 60 sec)
   - target: EC2, ASG, SNS
 - CloudWatch Alarms – Composite Alarms: CloudWatch Alarms are on a single metric. Composite Alarms are monitoring the states of multiple other alarms. AND and OR conditions. Helpful to reduce “alarm noise” by creating complex composite alarms.
 - EC2 Instance Recovery:
   - status checks: instance status and system status
 - Good to know: 1)Alarms can be created based on CloudWatch Logs Metrics Filters. 2)to test alarms and notifications, we can use CLI to manually change alarm state.
 - Amazon EventBridge:
   - schedule: cron jobs
   - event pattern: Event rules to react to a service doing something
   - trigger lambda, SNS/SQS, ...
   - can also use event filter
   - Event buses can be accessed by other AWS accounts using Resource-based Policies
   - You can archive events (all/filter) sent to an event bus (indefinitely or set period)
   - Ability to replay archived events
 - Amazon EventBridge – Schema Registry
   - EventBridge can analyze the events in your bus and infer the schema
   - The Schema Registry allows you to generate code for your application, that will know in advance how data is structured in the event bus
   - Schema can be versioned
 - Amazon EventBridge – Resource-based Policy
   - Manage permissions for a specific Event Bus
   - Example: allow/deny events from another AWS account or AWS region
 - CloudWatch Container Insights
   - Collect, aggregate, summarize metrics and logs from containers
   - containers in ECS, EKS, K8S on EC2, Fargate
   - In Amazon EKS and Kubernetes, CloudWatch Insights is using a containerized version of the CloudWatch Agent to discover containers
 - CloudWatch Lambda Insights
   - Monitoring and troubleshooting solution for serverless applications running on AWS Lambda
   - collect system-level metrics, such as CPU, diagnostic information, such as cold start
   - Lambda Insights is provided as a Lambda Layer
 - CloudWatch Contributor Insights
   - Analyze log data and create time series that display contributor data. (See metrics about the top-N contributors)
   - For example, you can find bad hosts, identify the heaviest network users, or find the URLs that generate the most errors.
   - You can build your rules from scratch, or you can also use sample rules that AWS has created – leverages your CloudWatch Logs
 - CloudWatch Application Insights (powered by sageMaker)
   - Provides automated dashboards that show potential problems with monitored applications, to help isolate ongoing issues
   - Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter
 - AWS CloudTrail
   - Provides governance, compliance and audit for your AWS Account
   - CloudTrail is enabled by default!
   - Get an history of events / API calls made within your AWS Account. Can put logs from CloudTrail into CloudWatch Logs or S3
   - A trail can be applied to All Regions (default) or a single Region.
   - If a resource is deleted in AWS, investigate CloudTrail first!
 - CloudTrail Events
   - Management Events: By default, trails are configured to log management events.
     - Can separate Read Events (that don’t modify resources) from Write Events (that may modify resources)
     - Operations that are performed on resources in your AWS account
   - Data Events: By default, data events are not logged (because high volume operations)
   - CloudTrail Insights Events: Enable CloudTrail Insights to detect unusual activity in your account
     - CloudTrail Insights analyzes normal management events to create a baseline
     - And then continuously analyzes write events to detect unusual patterns, send insight events to S3, eventBridge, or cloudtrail console
 - CloudTrail Events Retention: 90 days in cloudtrail, or send to S3 and use Athena for long-term storage
 - CloudTrail + EventBridge
 - AWS Config:  a per-region service
   - Can be aggregated across regions and accounts
   - Helps with auditing and recording compliance of your AWS resources. Helps record configurations and changes over time
   - You can receive alerts (SNS notifications) for any changes
   - Possibility of storing the configuration data into S3 (analyzed by Athena)
 - Config Rules
   - Can use AWS managed config rules (over 75)
   - Can make custom config rules (must be defined in AWS Lambda)
   - Rules can be evaluated / triggered: for each configuration change or at regular intervals
   - AWS Config Rules does not prevent actions from happening (no deny)
   - pricing: no free tier
 - Config Rules – Remediations
   - Automate remediation of non-compliant resources using SSM Automation Documents
   - Use AWS-Managed Automation Documents or create custom Automation Documents
   - You can set Remediation Retries if the resource is still non-compliant after auto-remediation
 - Config Rules – Notifications
   - Use EventBridge to trigger notifications when AWS resources are non-compliant
   - Ability to send configuration changes and compliance state notifications to SNS (all events – use SNS Filtering or filter at client-side)
 - CloudWatch vs CloudTrail vs Config
   - CloudWatch:
     - Performance monitoring (metrics, CPU, network, etc…) & dashboards
     - Events & Alerting
     - Log Aggregation & Analysis
   - CloudTrail:
     - Record API calls made within your Account by everyone
     - Can define trails for specific resources
     - Global Service
   - Config
     - Record configuration changes
     - Evaluate resources against compliance rules
     - Get timeline of changes and compliance
 - For an Elastic Load Balancer (Example)
   - CloudWatch:
     - Monitoring Incoming connections metric
     - Visualize error codes as % over time
     - Make a dashboard to get an idea of your load balancer performance
   - CloudTrail:
     - Track who made any changes to the Load Balancer with API calls
   - Config:
     - Track security group rules for the Load Balancer
     - Track configuration changes for the Load Balancer
     - Ensure an SSL certificate is always assigned to the Load Balancer (compliance)
## AWS IAM Advanced
## AWS Security and Encryption
## AWS VPC
## Disaster Recovery and Migration
## More Solutions Architecture
## Other services
## White Papers and Architectures
