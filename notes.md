# AWS (Infrastucture)

1. **`Region`** - It consists of AZ's
2. **`Availability Zones`** - Each AZ is made up of one or more discreet data center which make up a region. Separated to each other but with 100 km range. (Usually 3, Min 2, Max 6)
3. **`Edge Location` (Point of Presence)** - 
    * CDN for user as close as possible for low latency
    * AWS has 216 poin of presence (205 edge location & 11 regional caches)

# IAM

1. IAM - Identity & Access Management
2. IAM is a global service
3. First account created in AWS is the **root** user
4. **Users** are individual people using your AWS account
5. **User Group** is a way of organizing multiple users under same group
6. **Policy** is a json document attached to a group specifying what actions the group members can perform
7. AWS follows the concept of **least privileged policy**

# IAM security policy

1. **Password Policy**
    * Minimum length
    * Specific Character
    * Password Reuse
    * Changing own password
    * Password Expiration
2. **MFA** - Multi Factor Authentication
    * Virual MFA Device - e.g. Google Authenticator, Authy
    * U2F-Universal Second Factor Security Key - e.g. Yubikey by Yubico
    * Hardware Key Fob MFA device - e.g. Gemalto

# Accessing AWS

1. AWS Management Console - Using username and password (plus MFA)
2. AWS CLI - Using access keys (Access Key)
3. AWS SDK - Using access keys (Access Key)
4. AWS CloudShell

# IAM Roles

1. IAM role gives permission to AWS services created by us to access our AWS account
2. IAM roles are intended to be used by AWS services and not by users
3. A IAM role can be created for the following - 
    * AWS services
    * Another AWS account
    * Web Identity
    * SAML 2.0 federation
4. Common Roles - 
    * EC2 instance role
    * Lambda function role
    * CloudFormation role

# IAM Security Tools

1. **Credential Reports** (Account Level)
    * To view credentials report for all users in your account
2. **Access Advisor** (User Level)
    * Access Advisor shows the services that a user can access and when those services were last accessed. 
    * Review this data to remove unused permissions.

> # EC2 - Elastic Compute Cloud

* EC2 is used to provide **`IAAS`** in AWS 

# EC2 User Data (Bootstrapping)

* Using user data, a user can run a script when the ec2 instance is booted for the first time
* User data require super user previledge so we need to log in using root user

# EC2 Instances

* There are many instance type available for EC2
* There 7 categories of instances
    1. **General Purpose**
        * Balance between --> Compute • Memory • Networking
    2. **Compute Optimized**
        * Compute bound applications that benefit from high performance processors
    3. **Memory Optimized**
        * Fast performance for workloads that process large data sets in memory
    4. **Accelerated Computing**
        * Accelerated computing instances use hardware accelerators, or co-processors, to perform functions, such as floating point number calculations, graphics processing
    5. **Storage Optimized**
        * Storage optimized instances are designed for workloads that require high, sequential read and write access to very large data sets on local storage
    6. **Instance Feature**
    7. **Measuring Instance Performance**

* For free tier, we will be using **t2.micro**
    * **t** - instance class
    * **2** - generation
    * **micro** - size within the instance class

# Creating an EC2 instance
1. Choose AMI - Amazon Machine Image
2. Choose Instance Type
3. Configure Instance
4. Add Storage
5. Add Tags
6. Configure Security Group

# Security Group

* Provide n/w security in AWS (Firewall)
* They control how traffic is allowed into or out of our EC2 Instances
* Security Group are locked to a region

# Classic Ports

* 22 = SSH
* 21 = FTP
* 22 = SFTP
* 80 = HTTP
* 443 = HTTPS
* 3389 = RDP

# SSH into EC2

* Following ways available:
    * **Putty :** PPK file
    * **OpenSSH :** PEM file   

        ```bash
        ssh -i <path-to-pem-file> ec2-user@<public-ip-of-ec2-instance>
        ```
    * **EC2 instance connect :** Web interface
    * _Session Manager_
    * _EC2 Serial Console_

# Attaching IAM Roles in EC2 instance

* As told before, IAM roles can be attached to only AWS service and EC2 is a AWS service
* So we can create an IAM role with some policy attached (permissions) and attach that to our EC2 instance

# EC2 Instances Purchasing Options

* **On-Demand Instances**
* **Reserved**: (min: 1 yr, max: 3yr)
    * **Reserved Instances**
    * **Convertible Reserved Instances**
    * **Scheduled Reserved Instances**
* **Spot Instances**
* **Dedicated Hosts**
* **Dedicated Instances**

# Building AMI from EC2 instance

1. **Image from EC2 instance**
2. **EC2 Image Builder**
    * Used to automate the creation of Virtual Machines or container images
    * Automate the creation, maintain, validate and test EC2 AMIs
    * Can be run on a schedule (weekly, whenever packages are updated, etc…)
    * Free service (only pay for the underlying resources)

# EC2 Instance Storage

> ## EBS Volume (Elastic Block Store)

* An EBS volume **is a network drive** you can attach to your instances while they run
* It allows your instances to persist data, even after their termination
* They can only be mounted to **one instance at a time (at the CCP level)**
* They are **bound to a specific availability zone**
* Analogy: Think of them as a “network USB stick”
* Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or
Magnetic per month

* ### EBS Snapshots
    * Make a backup (snapshot) of your EBS volume at a point in time
    * Not necessary to detach volume to do snapshot, but recommended
    * Can copy snapshots across AZ or Region

> ## EC2 Instance Store
* EC2 Instance Store are physical drives on your EC2 instance
* If you need a high-performance hardware disk, use EC2 Instance Store
* Better I/O performance
* EC2 Instance Store lose their storage if they’re stopped (ephemeral)
* Good for buffer / cache / scratch data / temporary content
* Risk of data loss if hardware fails
* Backups and Replication are your responsibility

> ## EFS (Elastic File System)
* Managed NFS (network file system) that can be mounted on 100s of EC2
* EFS **works with Linux EC2** instances in **multi-AZ**
* Highly available, scalable, **expensive (3x gp2)**, **pay per use**, no capacity planning

> ## EFS Infrequent Access (EFS-IA)
* Storage class that is cost-optimized for files not accessed every day
* Up to **92% lower cost compared to EFS Standard**
* EFS will automatically move your files to EFS-IA based on the last time they were accessed
* Enable EFS-IA with a **Lifecycle Policy**
* E.g., move files that are not accessed for 60
days to EFS-IA
* Transparent to the applications accessing EFS

> ## Amazon FSx for Windows File Server
* A fully managed, highly reliable, and scalable Windows native shared file system
* Built on Windows File Server
* Supports **SMB protocol** & **Windows NTFS**
* Integrated with Microsoft Active Directory
* Can be accessed from AWS or your on-premise infrastructure

> ## Amazon FSx for Lustre
* A fully managed, high-performance, scalable file storage for **High Performance Computing (HPC)**
* The name Lustre is derived from “Linux” and “cluster”
* Machine Learning, Analytics, Video Processing, Financial Modeling, …
* Scales up to 100s GB/s, millions of IOPS, sub-ms latencies

# Load Balancing

## Scalability
1. > **Veritical Scaling**
    * Upgrade instance to a higher capacity instance
2. > **Horizontal Scaling = Elasticity**
    * Deploy application on multiple instances
    * To achieve this in AWS:
        * Loab Balancer
        * Auto Scaling Group
## High Avialability
* Deploying application on multiple Avalability Zones
* To achieve this in AWS:
    * Loab Balancer on multi AZ's
    * Auto Scaling Group

# ELB - Elastic Load Balancing

1. > **Application Load Balancer (ALB)**
    * For HTTP & HTTPS (Layer 7)
2. > **Network Load Balancer (NLB)**
    * For TCP, UDP, TLS (Layer 4)
3. > **Gateway Load Balancer (GLB)**
    * IP

# ASG - Auto Scaling Group
* Load balancer help out in diverting traffic to all EC2 instances in your *target group*
* But in case of Load Balancer, we need to create a target group and all ec2 instances in advance
* ASG helps in automatically creatinf our EC2 instances based on demand 

# Scaling Strategies

* > **Manual Scaling**: Update the size of an ASG manually
* > **Dynamic Scaling**: Respond to changing demand
    * > Simple / Step Scaling
        * When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
        * When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
    * > Target Tracking Scaling
        * Example: I want the average ASG CPU to stay at around 40%
    * > Scheduled Scaling
        * Anticipate a scaling based on known usage patterns
        * Example: increase the min. capacity to 10 at 5 pm on Fridays
* > **Predictive Scaling**
    * Uses Machine Learning to predict future traffic ahead of time
    * Automatically provisions the right number of EC2 instances in advance


> # S3 - Simple Storage Service

* S3 is often reffered as Infinite Scalling
* **Files == Objects**
* **Directories == Buckets**
* Buckets must have a globally unique name (across all regions all accounts)
* Buckets are defined at the region level
* S3 looks like a global service but buckets are **created in a region**
* Objects have a **Key**
* The key is the FULL path:
    * s3://my-bucket/my_file.txt
    * s3://my-bucket/my_folder1/another_folder/my_file.txt
* The key is composed of prefix + object name
    * s3://my-bucket/my_folder1/another_folder/my_file.txt
* There’s no concept of “directories” within buckets

* Object values are the content of the body:
    * **Max Object Size is 5TB** (5000GB)
    * **If uploading more than 5GB, must use “multi-part upload”**

## S3 Security
---
* User based
    * **IAM policies** - which API calls should be allowed for a specific user from IAM console
* Resource Based
    * **Bucket Policies** - bucket wide rules from the S3 console - allows across account
        * Object Access Control List (ACL) – finer grain
        * Bucket Access Control List (ACL) – less common

## Adding Bucket Policy
---
* Disable block public access setting
* Add a new policy using the policy generator tool
* 
    ```json
    Example:
    {
        "Version": "2012-10-17",
        "Id": "Policy1631099539396",
        "Statement": [
            {
                "Sid": "Stmt1631099502552",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::daipayan-bucket/*"
            }
        ]
    }
    ```
## S3 Websites
---
* S3 can host static websites and have them accessible on the www
* The website URL will be:
    * \<bucket-name>.s3-website-\<AWS-region>.amazonaws.com
    * \<bucket-name>.s3-website.\<AWS-region>.amazonaws.com


## S3 - Versioning
---
* You can version your files in Amazon S3
* It is enabled at the bucket level
* Same key overwrite will increment the “version”: 1, 2, 3….
* It is best practice to version your buckets
* Protect against unintended deletes (ability to restore a version)
* Easy roll back to previous version
* Notes:
    * Any file that is not versioned prior to enabling versioning will have version **“null”**
    * Suspending versioning does not delete the previous versions

## S3 Access Logs
---
* This features allows S3 to save logs onto another S3 bucket

## S3 Replication (CRR & SRR)
---
* **CRR = Cross Region Replication**
* **SRR = Same Region Replication**
* **Must enable versioning** in source and destination
* Buckets can be in different accounts
* Must give proper IAM permissions to S3
* CRR - Use cases: compliance, lower latency access, replication across accounts
* SRR – Use cases: log aggregation, live replication between production and test accounts


## S3 Storage Classes
---
1. > **Amazon S3 Standard - General Purpose**
    * 99.99% Availability
    * Used for frequently accessed data
    * Low latency and high throughput
    * Sustain 2 concurrent facility failures
    * Use Cases: Big Data analytics, mobile & gaming applications, content
    distribution…
2. > **Amazon S3 Standard - Infrequent Access (IA)**
    * Suitable for data that is less frequently accessed, but requires rapid access when needed
    * 99.9% Availability
    * Lower cost compared to Amazon S3 Standard, but retrieval fee
    * Sustain 2 concurrent facility failures
    * Use Cases: As a data store for disaster recovery, backups…
3. > **Amazon S3 Intelligent Tiering**
    * 99.9% Availability
    * Same low latency and high throughput performance of S3 Standard
    * Cost-optimized by automatically moving objects between two access
    tiers based on changing access patterns:
        * Frequent access
        * Infrequent access
    * Resilient against events that impact an entire Availability Zone
4. > **Amazon S3 One Zone - Infrequent Access (IA)**
    * Same as IA but data is stored in a single AZ
    * 99.5% Availability
    * Low latency and high throughput performance
    * Lower cost compared to S3-IA (by 20%)
    * Use Cases: Storing secondary backup copies of on-premise data, or
    storing data you can recreate
5. > **Amazon Glacier**
    * Low cost object storage (in GB/month) meant for archiving / backup
    * Data is retained for the longer term (years)
    * Cost & Retrieval:
        * Expedited (1 to 5 minutes)
        * Standard (3 to 5 hours)
        * Bulk (5 to 12 hours)
6. > **Amazon Glacier Deep Archive**
    * Cost & Retrieval:
        * Standard (12 hours)
    * Bulk (48 hours)
7. > **~~Amazon S3 Reduced Redundancy Storage (deprecated - omitted)~~**

## S3 – Moving between storage classes
---
* It is possible to move objects between multiple storage classes
* This movement is made automated using **lifecycle configuration**

## S3 Object Lock & Glacier Vault Lock
---
* > **S3 Object Lock**
    * Adopt a **WORM** (Write Once Read Many) model
    * _Block an object version deletion for a specified amount of time_
* > **Glacier Vault Lock**
    * Adopt a **WORM** (Write Once Read Many) model
    * _Lock the policy for future edits (can no longer be changed)_
    * Helpful for compliance and data retention

## AWS Snow Family
---
1. > **Snowcone**
    * **8 TBs of usable storage**
    * AWS DataSync pre-installed
2. > **Snowball Edge**
    * > Snowball Edge Storage Optimized
        * 52 vCPUs, 208 GiB of RAM
        * Optional GPU (useful for video processing or machine learning)
        * **39.5 TB usable storage**
    * > Snowball Edge Compute Optimized
        * Up to 40 vCPUs, 80 GiB of RAM
        * **80 TB usable storage**
        * Object storage clustering available
3. > **Snowmobile**
    * **100 PB of capacity**
    * Better than Snowball if you transfer more than 10 PB

* Data migration:
    * Snowcone 
    * Snowball Edge 
    * Snowmobile 
* Edge computing:
    * Snowcone 
    * Snowball Edge

## AWS OpsHub
---
AWS OpsHub (a software you install on your computer / laptop) to manage your Snow Family Device

## AWS Storage Gateway
---
* Bridge between on-premise data and cloud data in S3
* Hybrid storage service to allow onpremises to seamlessly use the AWS cloud

# AWS Database Services

1. > ## RDS - Relational Database Service `(SQL) `     
2. > ## ElastiCache `(in-memory)`
3. > ## DynamoDB `(NoSQL-Serverless)`
4. > ## DynamoDB Accelerator - DAX `(in-memory)`
5. > ## Redshift `(PostgreSQL-OLAP)`
6. > ## EMR - Elastic MapReduce `(Hadoop Cluster)`
7. > ## Athena `(SQL on S3 - Serverless)`
8. > ## QuickSight `(Interactive Dashboard - Serverless)`
9. > ## DocumentDB `(MongoDB)`
10. > ## Neptune `(Graph Database)`
11. > ## QLDB - Quantum Ledger Database `(Centralized Ledger)`
12. > ## Amazon Managed Blockchain  `(De-Centralized Ledger)`
13. > ## AWS Glue `(ETL - Extract Transform & Load - Serverless)`
14. > ## DMS – Database Migration Service


## RDS - Relational Database Service
---
* Managed DB service for DB use SQL as a query language
* Storage backed by **EBS**
* Examples:
    * Postgres
    * MySQL
    * MariaDB
    * Oracle
    * Microsoft SQL Server
    * > **Aurora (AWS Proprietary database)**
        * Aurora is a proprietary technology from AWS (not open sourced)
        * **PostgreSQL** and **MySQL** are both **supported** as Aurora DB
        * Aurora is “**`AWS cloud optimized`**” and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
        * Aurora storage automatically grows in **`increments of 10GB, up to 64 TB`**
        * **`Aurora costs more than RDS (20% more)`** – but is more efficient
        * Not in the free tier

## RDS Deployments
---
1. > **Read Replicas**
    * Scale the read workload of your DB
    * Can create up to 5 Read Replicas
    * Data is only written to the main DB
2. > **Multi-AZ**
    * Failover in case of AZ outage (high availability)
    * Data is only read/written to the main database
    * **`Can only have 1 other AZ as failover`**
3. > **Multi-Region (Read Replicas)**
    * Disaster recovery in case of region issue
    * Local performance for global reads
    * Replication cost

## ElastiCache
---
* ElastiCache is to get managed Redis or Memcached

## DynamoDB
---
* Fully Managed Highly available with **replication across 3 AZ**
* NoSQL database
* Scales to massive workloads, distributed **`“serverless”`** database
* Millions of requests per seconds, trillions of row, 100s of TB of storage
* Fast and consistent in performance
* Single-digit millisecond latency – low latency retrieval
* Integrated with IAM for security, authorization and administration
* Low cost and auto scaling capabilities

## DynamoDB Accelerator - DAX
---
* Fully Managed in-memory cache for DynamoDB
* 10x performance improvement – singledigit
millisecond latency to microseconds latency – when accessing your DynamoDB tables
* Secure, highly scalable & highly available
* Difference with ElastiCache at the CCP level: **DAX is only used for DynamoDB**, while **ElastiCache can be used for other
databases**

## Redshift
---
* Redshift is based on **PostgreSQL**, but it’s **`not used for OLTP`**
* It’s **`OLAP – online analytical processing`** (analytics and data warehousing)
* Load data once every hour, not every second
* 10x better performance than other data warehouses, scale to PBs of data
* **Columnar** storage of data (instead of row based)
* Massively Parallel Query Execution (MPP), highly available
* Pay as you go based on the instances provisioned
* Has a SQL interface for performing the queries
* BI tools such as AWS Quicksight or Tableau integrate with it

## Amazon EMR - Elastic MapReduce
---
* EMR helps creating **`Hadoop clusters`** `(Big Data)` to analyze and process
vast amount of data
* The clusters can be made of hundreds of EC2 instances
* Also supports **Apache Spark, HBase, Presto, Flink…**
* EMR takes care of all the provisioning and configuration
* Auto-scaling and integrated with Spot instances
* Use cases: data processing, machine learning, web indexing, big data…

## Athena
---
* Fully **`serverless`** database with SQL capabilities
* Used to **`query data in S3`** & output **`results back to S3`**
* Pay per query
* Secured through IAM
* Use Case: one-time SQL queries, serverless queries on S3, log analytics

## Amazon QuickSight
---
* **`Serverless` machine learning-powered** business intelligence service to
create **interactive dashboards**
* Fast, automatically scalable, embeddable, with per-session pricing
* Use cases:
    * Business analytics
    * Building visualizations
    * Perform ad-hoc analysis
    * Get business insights using data
    * Integrated with RDS, Aurora, Athena, Redshift, S3…

## DocumentDB
---
* **“`AWS-implementation” of MongoDB`** (which is a NoSQL database)
* Fully Managed, highly available with replication across 3 AZ
* Aurora storage automatically grows in increments of 10GB, up to 64 TB.
* Automatically scales to workloads with millions of requests per seconds

## Amazon Neptune
---
* Fully managed **`Graph database`**
* A popular graph dataset would be a social network
* Highly available across **3 AZ**, with up to **15 read replicas**
* Build and run applications working with highly connected
datasets – optimized for these complex and hard queries
* Can store up to billions of relations and query the graph with
milliseconds latency
* Highly available with replications across multiple AZs
* Great for knowledge graphs (Wikipedia), fraud detection,
recommendation engines, social networking

## QLDB - Quantum Ledger Database
---
* A **`ledger`** is a book recording **`financial transactions`**
* Fully Managed, Serverless, High available, Replication across 3 AZ
* Used to review history of all the changes made to your application data over time
* **`Immutable system`**: no entry can be removed or modified, cryptographically verifiable
* 2-3x better performance than common ledger blockchain frameworks, manipulate data using SQL
* Difference with Amazon Managed Blockchain: **no decentralization component**, in accordance with financial regulation rules

## Amazon Managed Blockchain
---
* Amazon Managed Blockchain is a managed service to:
    * Join public blockchain networks 
    * or Create your own scalable private network
* Compatible with the frameworks **`Hyperledger Fabric`** & **`Ethereum`**

## AWS Glue
---
* Managed **`extract`, `transform`, and `load` (ETL)** service
* Useful to prepare and transform data for analytics
* Fully **`serverless`** service
* Glue Data Catalog: catalog of datasets
* can be used by Athena, Redshift, EMR

## DMS – Database Migration Service
---
* Quickly and securely **`migrate databases`** to AWS, resilient, self healing
* The source database remains available during the migration
* Supports:
    * **`Homogeneous migrations`**: e.g., Oracle to Oracle
    * **`Heterogeneous migrations`**: e.g., Microsoft SQL Server to Aurora

# Other Compute Section

* > ## ECS
* > ## Fargate
* > ## ECR
* > ## AWS Lambda
* > ## Amazon API Gateway
* > ## AWS Batch
* > ## AWS Light Sail


## ECS
---
* ECS = Elastic Container Service
* Launch Docker containers on AWS
* You **`must provision & maintain the infrastructure`** (the EC2 instances)
* AWS takes care of starting / stopping containers
* Has integrations with the Application Load Balancer


## Fargate
---
* Launch Docker containers on AWS
* You **`do not provision the infrastructure`** (no EC2 instances to manage) – simpler!
* **`Serverless`** offering
* AWS just runs containers for you based on the CPU / RAM you need


## ECR
---
* ECR = Elastic Container Registry
* Private **`Docker Registry`** on AWS
* This is where you store your Docker images so they can be run by ECS or Fargate


## AWS Lambda
---
* **`Virtual functions`** – no servers to manage (**`serverless`**)!
* Limited by time - short executions
* Scaling is automated!
* Integrated with the whole AWS suite of services
* **`Event-Driven`**: functions get invoked by AWS when needed
* Integrated with many programming languages
* Easy monitoring through AWS CloudWatch
* Easy to get more resources per functions (up to 10GB of RAM!)
* Increasing RAM will also improve CPU and network!
* > **Lambda Container Image**
    * The container image **`must implement the Lambda Runtime API`**
    * **`ECS`** / **`Fargate`** is preferred for running **`arbitrary Docker images`**
* **`Pay per call or per duration`**


## Amazon API Gateway
---
* Fully managed service for developers to **easily `create`, `publish`, `maintain`, `monitor`, and `secure` `APIs`**
* **`Serverless`** and scalable
* Supports RESTful APIs and WebSocket APIs
* Support for security, user authentication, API throttling, API keys, monitoring...


## AWS Batch
---
* Fully managed batch processing at any scale
* Efficiently run 100,000s of computing batch jobs on AWS
* A “batch” job is a job with a start and an end (opposed to continuous)
* Batch **`will dynamically launch EC2 instances or Spot Instances`**
* AWS Batch provisions the right amount of compute / memory
* You submit or schedule batch jobs and AWS Batch does the rest!
* **`Batch jobs are defined as Docker images and run on ECS`**
* Helpful for cost optimizations and focusing less on the infrastructure


## AWS Light Sail
---
* Virtual servers, storage, databases, and networking
* **`Simpler alternative to using EC2, RDS, ELB, EBS, Route 53…`**
* Great for people with **`little cloud experience!`**
* Can setup notifications and monitoring of your Lightsail resources
* Use cases:
    * Simple web applications (has templates for LAMP, Nginx, MEAN, Node.js…)
    * Websites (templates for WordPress, Magento, Plesk, Joomla)
    * Dev / Test environment
* Has high availability but no auto-scaling, limited AWS integrations

## What’s serverless?
---
* A serverless service is a service where user **`doesn't have to maintain a EC2 instance`**
* Examples of serverless services we have seen till now:
    * DynamoDB
    * Athena
    * Amazon QuickSight
    * AWS Glue
    * Fargate
    * Lambda
    * Amazon API Gateway


# Deploying & Managing Infrastructure at Scale

* > ## CloudFormation
* > ## AWS CDK - Cloud Development Kit
* > ## Elastic Beanstalk
* > ## AWS CodeDeploy
---
* > ## AWS CodeCommit
* > ## AWS CodeBuild
* > ## AWS CodePipeline
* > ## AWS Cloud9
---
* > ## AWS Systems Manager (SSM)
* > ## AWS OpsWorks

## CloudFormation
---
* CloudFormation is a **`declarative way of outlining your AWS Infrastructure`**, for any resources (most of them are supported).
* For example, within a CloudFormation template, you say:
    * I want a security group
    * I want two EC2 instances using this security group
    * I want an S3 bucket
    * I want a load balancer (ELB) in front of these machines
* Then CloudFormation creates those for you, in the right order, with the
exact configuration that you specify
* **`Infrastructure as code`**
    * No resources are manually created, which is excellent for control
    * Changes to the infrastructure are reviewed through code
* Cost
    * Each resources within the stack is tagged with an identifier so you can easily see how
    much a stack costs you
    * You can estimate the costs of your resources using the CloudFormation template
    * Savings strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely
* **`CloudFormation Stack Designer`** is visual representation of all resources that will be creted for a cloudformation template


## AWS CDK - Cloud Development Kit
---
* Define your **`cloud infrastructure using a familiar programming language`**:
    * JavaScript/TypeScript, Python, Java, and .NET
* The code is “compiled” into a CloudFormation template (JSON/YAML)
* You can therefore **`deploy infrastructure and application runtime code together`**
    * Great for Lambda functions
    * Great for Docker containers in ECS / EKS


## Elastic Beanstalk
---
* Elastic Beanstalk is a **`developer centric view of deploying
an application on AWS`**
* It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, etc…
* But it’s all in one view that’s easy to make sense of!
* We still have full control over the configuration
* **`Beanstalk = Platform as a Service (PaaS)`**
* Beanstalk is free but you pay for the underlying instances


## AWS CodeDeploy
---
* We want to deploy our application automatically
* Works with EC2 Instances
* Works with On-Premises Servers
* **`Hybrid service`**
* Servers / Instances must be **`provisioned`** and **`configured`** ahead of time with the **`CodeDeploy Agent`**

## AWS CodeCommit
---
* AWS offering for **`code repository`**, like GitHub

## AWS CodeBuild
---
* **`Compiles`** source code, run **`tests`**, and **`produces packages`** that are ready to be deployed

## AWS CodePipeline
---
* AWS **`CI-CD offering`**, like Jenkins

## AWS CodeArtifact
---
* **`Artifact Management System`**, like Artifactory
    * Store & Retrieve code dependencies

## AWS CodeStar
---
* **`Unified UI`** to easily manage software development activities in one place

## AWS Cloud9
---
* **`Online IDE`** for coding and debugging code

## AWS Systems Manager (SSM)
---
* Helps you **`manage your EC2 and On-Premises systems at scale`**
* Another **`hybrid`** AWS service

## AWS OpsWorks
---
* To perform **`server configuration`** using **`Chef`** & **`Puppet`**, 
* It’s an **`alternative to AWS SSM`** & hence an **`hybrid`** AWS service


# Global Applications in AWS

## Route 53
---
* **`Global DNS`**
* Great to route users to the closest deployment with least latency
* Great for disaster recovery strategies
* In AWS, the most common records are:
    * www.google.com => 12.34.56.78 == **`A record`** (IPv4)
    * www.google.com => 2001:0db8:85a3:0000:0000:8a2e:0370:7334 == **`AAAA IPv6`**
    * search.google.com => www.google.com == **`CNAME`**: hostname to hostname
    * example.com => AWS resource == **`Alias`** (ex: ELB, CloudFront, S3, RDS, etc…)

## Route 53 Routing Policies
---

* > **`SIMPLE ROUTING POLICY`** (No Health Check)
* > **`WEIGHTED ROUTING POLICY`** (Health Check) 
* > **`LATENCY ROUTING POLICY`** (Health Check)
* > **`FAILOVER ROUTING POLICY`** (Health Check)

## CloudFront
---
* **`Global CDN`** - Content Delivery Network
* Replicate part of your application to AWS Edge Locations – decrease latency
* Cache common requests – improved user experience and decreased latency
* **`DDoS protection`** (because worldwide), integration with Shield, **`AWS WAF (Web Application Firewall)`**
* **`CloudFront Origin`**: Services from which CloudFront can cache:
    1. S3 bucket (Enhanced security with CloudFront **`Origin Access Identity (OAI)`**)
    2. Custom Origin (HTTP):
        * Application Load Balancer
        * EC2 instance
        * S3 website
        * Any HTTP backend you want

# S3 Transfer Acceleration
* **`Accelerate`** global **`uploads`** & **`downloads`** into **`Amazon S3`**


## AWS Global Accelerator
---
* Improve global application **`availability`** and **`performance`** using the **`AWS global network`**
* **`2 Anycast IP`** are created for your application and traffic is sent through Edge Locations

# AWS Outposts

* **`Hybrid Cloud`**: businesses that keep an onpremises
infrastructure alongside a cloud infrastructure
* Therefore, two ways of dealing with IT systems:
    * One for the AWS cloud (using the AWS console, CLI, and AWS APIs)
    * One for their on-premises infrastructure 
* AWS Outposts are “server racks” that offers the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud
* AWS will setup and manage “Outposts Racks” within your on-premises infrastructure and you can start leveraging AWS services on-premises 
* You are **`responsible`** for the Outposts Rack **`physical security`**


# AWS WaveLength

* **`WaveLength Zones`** are infrastructure deployments embedded within the telecommunications providers datacenters at the **`edge of the 5G networks`**
* Brings AWS services to the edge of the 5G networks
* Example: EC2, EBS, VPC…
* Ultra-low latency applications through 5G networks 
* Traffic doesn’t leave the Communication Service Provider’s (CSP) network
* High-bandwidth and secure connection to the parent AWS Region
* No additional charges or service agreements
* Use cases: Smart Cities, ML-assisted diagnostics, Connected Vehicles, Interactive Live Video Streams, AR/VR, Real-time Gaming, …

# Cloud Integration

# Amazon SQS – Simple Queue Service

* Oldest AWS offering (over 10 years old)
* Fully managed service (~**`serverless`**), use to decouple applications
* Scales from 1 message per second to 10,000s per second
* Default retention of messages: 4 days, maximum of 14 days
* No limit to how many messages can be in the queue
* Messages are **`deleted`** after they’re read by consumers
* Low latency (<10 ms on publish and receive)
* Consumers share the work to read messages & scale horizontally

# Amazon SNS - Simple Notification Service

* The “**`event publishers`**” only sends message to one **`SNS topic`**
* As many “**`event subscribers`**” as we want to listen to the **`SNS topic notifications`**
* Each subscriber to the topic will get all the messages
* Up to 10,000,000 subscriptions per topic, 100,000 topics limit
* SNS Subscribers can be:
    * HTTP / HTTPS (with delivery retries – how many times)
    * Emails, SMS messages, Mobile Notifications
    * SQS queues (fan-out pattern), Lambda Functions (write-your-own integration)

# Amazon Kinesis
* For the exam: **`Kinesis = real-time big data streaming`**
* Managed service to collect, process, and analyze real-time streaming
data at any scale
* Too detailed for the Cloud Practitioner exam but good to know:
    * Kinesis Data Streams: low latency streaming to ingest data at scale from hundreds of thousands of sources
    * Kinesis Data Firehose: load streams into S3, Redshift, ElasticSearch, etc…
    * Kinesis Data Analytics: perform real-time analytics on streams using SQL
    * Kinesis Video Streams: monitor real-time video streams for analytics or ML

# Amazon MQ
* SQS, SNS are “cloud-native” services, and they’re using proprietary protocols from AWS.
* Traditional applications running from on-premise may use open protocols
such as: **`MQTT`**, **`AMQP`**, **`STOMP`**, **`Openwire`**, **`WSS`**
* When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ
* **`Amazon MQ = managed Apache ActiveMQ`**
* Amazon MQ doesn’t “scale” as much as SQS / SNS
* Amazon MQ runs on a dedicated machine (not serverless)
* Amazon MQ has both queue feature (~SQS) and topic features (~SNS)

# Cloud Monitoring

1. **`CloudWatch`**:
    * **`Metrics`**: monitor the performance of AWS services and billing metrics
    * **`Alarms`**: automate notification, perform EC2 action, notify to SNS based on metric
    * **`Logs`**: collect log files from EC2 instances, servers, Lambda functions…
    * **`Events`** (or **`EventBridge`**): react to events in AWS, or trigger a rule on a schedule
2. **`CloudTrail`**: audit API calls made within your AWS account
3. **`CloudTrail Insights`**: automated analysis of your CloudTrail Events
4. **`X-Ray`**: trace requests made through your distributed applications
5. **`Service Health Dashboard`**: status of all AWS services across all regions
6. **`Personal Health Dashboard`**: AWS events that impact your infrastructure
7. **`Amazon CodeGuru`**: automated code reviews and application performance recommendations


## CloudWatch Metrics
---
* Metric is a variable to monitor (**`CPUUtilization`**, **`NetworkIn…`**)
* Metrics have timestamps
* Can create **`CloudWatch dashboards`** of metrics
* **`CloudWatch Billing`** metric (**`only us-east-1 region`**)
* Important Metrics
    * **`EC2 instances`**: CPU Utilization, Status Checks, Network (not RAM)
        * Default metrics every 5 minutes
        * Option for Detailed Monitoring ($$$): metrics every 1 minute
    * **`EBS volumes`**: Disk Read/Writes
    * **`S3 buckets`**: BucketSizeBytes, NumberOfObjects, AllRequests
    * **`Billing`**: Total Estimated Charge (only in us-east-1)
    * **`Service Limits`**: how much you’ve been using a service API
    * **`Custom metrics`**: push your own metrics

## CloudWatch Alarms
---
* Alarms are used to trigger notifications for any metric
* **`Alarms actions`**: 
    * Auto Scaling: increase or decrease EC2 instances “desired” count
    * EC2 Actions: stop, terminate, reboot or recover an EC2 instance
    * SNS notifications: send a notification into an SNS topic

## CloudWatch Logs
---
* CloudWatch Logs **`can collect log`** from:
    * **`Elastic Beanstalk`**: collection of logs from application
    * **`ECS`**: collection from containers
    * **`AWS Lambda`**: collection from function logs
    * **`CloudTrail`** based on filter
    * **`CloudWatch log agents`**: on EC2 machines or on-premises servers (**`hybrid`**)
    * **`Route53`**: Log DNS queries
* Enables real-time monitoring of logs
* Adjustable CloudWatch Logs retention

## CloudWatch Events
---

* Type of events: 
    * **`Schedule`**: Cron jobs (scheduled scripts)
    * **`Event Pattern`**: Event rules to react to a service doing something
* **`Target`**: Trigger Lambda functions, send SQS/SNS messages…


## Amazon EventBridge
---

* EventBridge is the **`next evolution of CloudWatch Events`**
* Default event bus: generated by AWS services (CloudWatch Events)
* **`Partner event bus`**: receive events from SaaS service or applications
(Zendesk, DataDog, Segment, Auth0…)
* **`Custom Event buses`**: for your own applications
* **`Schema Registry`**: model event schema
* EventBridge has a different name to mark the new capabilities
* The CloudWatch Events name will be replaced with EventBridge

## CloudTrail
---
* Provides **`governance`**, **`compliance`** and **`audit`** for your AWS Account
* **`CloudTrail is enabled by default!`**
* **`Get an history of events / API calls`** made within your AWS Account by:
    * Console
    * SDK
    * CLI
    * AWS Services
* Can put logs from CloudTrail into CloudWatch Logs or S3
* A trail can be applied to All Regions (default) or a single Region.
* If a resource is deleted in AWS, investigate CloudTrail first!


## CloudTrail Events
---
1. **`Management Events`**:
    * Operations that are performed on resources in your AWS account
    * Examples:
        * Configuring security (IAM AttachRolePolicy)
        * Configuring rules for routing data (Amazon EC2 CreateSubnet)
        * Setting up logging (AWS CloudTrail CreateTrail)
        * By default, trails are configured to log management events.
        * Can separate Read Events (that don’t modify resources) from Write Events (that may modify resources)
2. **`Data Events`**:
    * By default, data events are not logged (because high volume operations)
    * Amazon S3 object-level activity (ex: GetObject, DeleteObject, PutObject): can separate Read and Write Events
    * AWS Lambda function execution activity (the Invoke API)
3. **`CloudTrail Insights Events`**:
    * Enable CloudTrail Insights to **`detect unusual activity`** in your account:
        * inaccurate resource provisioning
        * hitting service limits
        * Bursts of AWS IAM actions
        * Gaps in periodic maintenance activity
    * CloudTrail Events Retention
        * Events are stored for **`90 days in CloudTrail`**
        * To keep events **`beyond`** this period, log them to **`S3 and use Athena`**

## AWS X-Ray
---
* Log formats differ across applications and log analysis is hard.
* Debugging: one big monolith “easy”, distributed services “hard”
* No common views of your entire architecture


## Amazon CodeGuru
---
* An **`ML-powered service`** for automated code **`reviews`** and application **`performance recommendations`**
* Provides two functionalities
    * **`CodeGuru Reviewer`**: automated code reviews for static code analysis (development)
    * **`CodeGuru Profiler`**: visibility/recommendations about application performance during runtime (production)


## AWS Status - Service Health Dashboard
---
* Shows **`all regions, all services health`**
* Shows historical information for each day
* Has an RSS feed you can subscribe to

## AWS Personal Health Dashboard
---
* AWS Personal Health Dashboard provides alerts and remediation
guidance when AWS is experiencing events that may impact you.
* While the Service Health Dashboard displays the general status of
AWS services, Personal Health Dashboard gives you a **`personalized
view into the performance and availability of the AWS services
underlying your AWS resources`**.
* The dashboard displays relevant and timely information to help you
manage events in progress and provides proactive notification to
help you plan for scheduled activities.

