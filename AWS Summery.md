# 1
Business - People - Operations - Governance - Security - Platform
# 2
**On-Demand** = Offers flexibility to start and stop instances as needed without upfront payments or long-term contracts.
![[Pasted image 20260626030040.png]]
**Elastic Block Store (EBS) Volumes** =  provides **persistent block-level storage** for virtual machines (EC2 instances). It allows instances to persist data even after termination
Think of EBS like a **virtual hard disk** that you attach to a server in the cloud.
- You launch an EC2 instance
- Then you attach an EBS volume
- The instance reads/writes data to it like a normal disk
**Amazon Machine Image (AMI)** = customization of an EC2 instance
**EC2 Image Builder** = is a service from Amazon Web Services that lets you: **Automatically build, update, test, and distribute AMIs**
**EFS - Elastic File System** = provides a **shared file system** for multiple servers in Amazon EC2.
 **EFS Infrequent Access (EFS-IA)** = Is a storage class optimized for cost, designed for files not accessed daily
# 4
**Subnets** = allow you to partition your network inside your VPC (Availability Zone resource)
**Internet Gateways =** enable internet connectivity for instances in a VPC.
**Public Subnets =** are configured with a route to the Internet Gateway for external access.
**NAT Gateways (managed by AWS) and NAT Instances (managed by the user)** provide internet access to instances in Private Subnets while keeping them private.
- **NACL (Network Access Control List):**
    - Acts as a firewall controlling traffic to and from a subnet.
    - Supports both ALLOW and DENY rules.
    - Attached at the subnet level.
- **Security Groups:**
    - A firewall that controls traffic to and from an Elastic Network Interface (ENI) or an EC2 instance.
    - Only ALLOW rules are supported.
 **VPC Peering** = allows two VPCs to connect privately using AWS' network
**Transit Gateway** = Provides transitive peering between thousands of VPCs and on-premises networks using a hub-and-spoke (star) connection.
 **Site-to-Site VPN:** = Connects an **on-premises VPN to AWS.**
 **Direct Connect** = Establishes a **physical connection between on-premises and AWS.**
 **AWS Client VPN (using OpenVPN):** = Allows you to connect from your computer to your private network in AWS and on-premises.

# 5
**AWS OpsHub** = a software installed on your computer or laptop, can be used to **manage Snow Family devices.**
**AWS Storage Gateway** = provides a seamless way to connect on-premises environments with S3 storage
# 6
**Amazon RDS** = **Managed** database service employing SQL for queries
**Amazon Aurora** = Amazon Aurora is a global-scale relational database service built for the cloud with full MySQL and PostgreSQL compatibility.
- claiming a **5x improvement over MySQL on RDS** and **over 3x the performance of Postgres on RDS.** it also costs 20% more than RDS.
**Amazon Aurora Serverless** = is a version of Aurora where AWS automatically manages the database capacity for you.
**Amazon DynamoDB** = Automated key/value database provisioning and dynamic scaling in response to actual usage.
- Designed for extremely high scalability, low latency, and high availability.
**Amazon DocumentDB** = with MongoDB compatibility)
**DynamoDB Accelerator - DAX** = Fully managed in-memory cache for DynamoDB.
**Amazon Redshift** = It is designed for **Online Analytical Processing (OLAP) - suitable for analytics and data warehousing.**
- not used for Online Transaction Processing (OLTP)
**Amazon Redshift Serverless** = Automatically provisions and scales the underlying capacity of the data warehouse.
**Amazon EMR (Elastic MapReduce)** = A cloud **big data platform for processing massive amounts of data.**
**Amazon Athena** = Serverless query service to analyze data stored in Amazon S3.
**Amazon QuickSight** = Serverless **machine learning-powered business intelligence service for creating interactive dashboards.**
**Amazon Neptune** = Fully managed **graph database service.**
**Amazon TimeStream** = It is designed specifically for data that changes over time and is collected continuously.
**Amazon Quantum Ledger Database (Amazon QLDB)** =  designed to record financial transactions in a transparent, immutable, and cryptographically verifiable manner. One trusted authority keeps a tamper-proof history of all data changes.
- Used to **review the history of all changes made to your application data over time.**
- Immutable system: no entry can be removed or modified, ensuring data integrity.
**Amazon Managed BlockChain** = Enables the **development of applications where multiple parties can execute transactions without requiring a trusted, central authority.** Many organizations trust a shared system without a central owner.
![[Pasted image 20260625155233.png]]
**AWS Glue** = Managed database to **extract, transform, and load (ETL) service.**
**DMS – Database Migration Service** = Quickly and securely **migrates databases to AWS, ensuring resilience and self-healing capabilities.**
**AWS Schema Conversion Tool** = It helps convert your database structure (**schema**) and some database code automatically.
# 7
1. **AWS Management Console (protected by password + MFA)**
2. **AWS Command Line Interface (CLI):** protected by access keys
3. **AWS Software Developer Kit (SDK) - for code:** protected by access keys
Access Keys are generated via the **AWS Management Console.**
- The Access Key ID serves a similar purpose as a username.
- The Secret Access Key serves a similar purpose as a password.

**IAM Access Advisor (User-Level):** Displays the **service permissions granted to an IAM user.**
 **IAM Credentials Report (Account-Level):** Provides a comprehensive list of **all users in your AWS account along with the status of their various credentials.**

**AWS Secrets Manager** -> designed for **storing secrets securely. **Integrates seamlessly with **Amazon RDS (MySQL, PostgreSQL, Aurora) for enhanced security.**, Ensures encryption of secrets using KMS (Key Management Service).
**Amazon GuardDuty =** An **intelligent threat discovery solution** to safeguard your AWS Account.
- Utilizes Machine Learning algorithms, anomaly detection techniques, and third-party data sources.
**Amazon Inspector** = Automated Security Assessments streamline security evaluations across various AWS resources.
**AWS Config** = facilitates **auditing and maintaining compliance** for AWS resources. (What changed?)
**AWS CloudTrail** = Provides **governance, compliance, and auditing capabilities** for your AWS account. (Who did it?)
**AWS Macie** = Amazon Macie is a fully managed data security and privacy service **leveraging machine learning and pattern matching to identify and safeguard sensitive data within AWS.**

**Amazon Security Hub** = serves as a centralized security management tool, enabling the oversight of security across multiple AWS accounts and automating security checks.
- Integrated **dashboards provide real-time security and compliance status, facilitating swift actions.**
**AWS Detective** = leverages machine learning and graph analysis to analyze, investigate, and rapidly identify the underlying cause of security concerns or suspicious activities.
**IAM Access Analyzer** = Identify shared **resources outside your defined zone of trust** within AWS
**AWS Abuse** = If you suspect AWS resources are being used for abusive or illegal activities, report them promptly to the AWS Abuse team.
**CloudWatch** offers metrics for every service in AWS, providing valuable insights into resource utilization and performance
- **CloudWatch Alarms** = Alarms serve to initiate notifications based on various metrics.
- **CloudWatch Logs** = By default, logs from your EC2 instance do not go to CloudWatch.
**AWS EventBridge** = is a **serverless event bus that helps AWS services and applications communicate using events.**
**X-Ray** = is an AWS service that provides a visual analysis of our applications
**Amazon CodeGuru** = is an ML-powered service for automated code reviews and application performance recommendations.
- **Reviewer** = Identifies critical issues, security vulnerabilities, and hard-to-find bugs.
- **Profiler** = helps understand the **runtime behavior of your application.**
**AWS CodeCommit** = A **fully managed Git-based source code repository**
**AWS Shield** = helps detect and block malicious traffic automatically.
**AWS Professional Services** =  is a team of AWS experts who help customers:
- Assess existing applications
- Plan migrations
- Design cloud architectures
- Execute large migration projects
![[Pasted image 20260624185603.png]]
```
Technology Partner:
"Here's our software."

Consulting Partner:
"We'll help build your AWS solution."
```
**CodeDeploy** = Deploy my application.
**OpsWorks** = Configure and manage my servers.
**Batch** = Run this compute job.
# 8
**AWS CloudFront** = **a content delivery network (CDN) service** built for high performance, security, and developer convenience.
- Ideal for **static content requiring global availability.**
- Content cached at the edge locations
- Files cached for a **TTL**
- **S3 Bucket: Used for file distribution and caching at edge locations.**
- CloudFront as Ingress: Allows uploads directly to an S3 bucket.

**S3 Cross Region Replication:**
- Requires setup for each region where replication is needed.
- Files update in near real-time.
- Read-only access.
- Suitable for **dynamic content requiring low-latency access in specific regions.**
**S3 Transfer Acceleration** = speeds up **content transfers** to and from **Amazon S3 by as much as 50-500% for long-distance transfer of larger objects.**
**AWS Global Accelerator** = **Enhance your application's availability and speed worldwide** with AWS's global network.
**AWS Outposts** = are “server racks” that offers the same AWS infrastructure, services, APIs & tools to build your own applications on -premises just as in the cloud
**AWS WaveLength** = brings AWS infrastructure to the edge of **5G networks**, enabling ultra-low latency applications by processing data closer to end-users.
**AWS Local Zones** = Small AWS infrastructure locations placed near major cities to reduce latency
**Docker** = is a **software development platform used to deploy applications.**
- Applications are packaged in containers that can run on any operating system.
**Amazon ECR (Elastic Container Registry)** = is a private repository service provided by AWS, allowing users to store and manage their own Docker images securely.
**ECS (Elastic Container Service)** = A service that allows you to launch Docker containers on AWS.
    - You are responsible for provisioning and maintaining the infrastructure, such as the EC2 instances.
    - **Container Management:** AWS takes care of starting and stopping containers for you.
**Fargate** = Launch Docker containers on **AWS without provisioning the infrastructure, eliminating the need to manage EC2 instances.** (serverless)
**Lambda** = Run code automatically without managing servers (serverless)
- Time-limited execution
- Supports only specific runtimes
- Limited temporary disk space
- Fully serverless
**Amazon API Gateway** = Fully **managed service for API management.** (serverless)
**AWS Batch** = Fully managed **batch processing at any scale.**
- No execution time limit
- Supports any runtime packaged as a Docker image
- Uses EBS or instance store for disk space
- Relies on EC2 (self-managed or AWS-managed)
**AWS Lightsail** = A simplified, beginner-friendly way to launch servers, databases, and websites on AWS
**Amazon SQS – Simple Queue Service** = is a fully managed, serverless service used to **decouple applications.**
- FIFO = First In First Out (ordering of messages in the queue)
**Amazon SNS** = Basically, sending one message to many users
**Amazon Kinesis** = is a managed service for **real-time big data streaming.**
**Amazon MQ** = a managed message broker service for migrating such applications to the cloud.
**AWS CloudFormation** = is designed for **Managing infrastructure as code (IaC)** in AWS
**AWS Trusted Advisor** = is a **cloud optimization and guidance service** that continuously analyzes your AWS environment and gives recommendations to improve
**Amazon Kendra** = is a **machine learning–powered enterprise search service** that helps organizations search across large amounts of data using **natural language questions** instead of traditional keyword queries.
# 9
**Organizational Units** = You can simplify account management by grouping related accounts 
**Service Control Policies (SCP)** = whitelist or blacklist IAM actions.
**AWS Control Tower** = offers a streamlined solution for establishing and managing a secure and compliant multi-account AWS environment, incorporating industry best practices.
**AWS Resource Access Manager (AWS RAM)** = Share AWS resources with other AWS accounts or within your organization
**AWS Service Catalog** provides a solution by offering access to pre-defined virtual machines, databases, storage options, etc.
**AWS Pricing Calculator** = a free web-based planning tool that you can use to create cost estimates for using AWS services.
Use **cost allocation tags to track your AWS costs on a detailed level**
**Cost Explorer** = Gain insight into your AWS costs and usage trends over time.
**AWS Budgets** = Create budget and send alarms when costs exceeds the budget
**AWS Cost Anomaly Detection** = Utilize machine learning to continuously monitor your cost and usage.
**AWS Service Quotas** = Notify you when **you’re close to a service quota value threshold**
**AWS Total Cost of Ownership (TCO) Calculator** = Compare the cost of running IT infrastructure on-premises vs. AWS Cloud
# 10
**AWS Security Token Service (STS)** = creates temporary, limited-privilege credentials.
**Amazon Cognito** = manages user identity for web and mobile applications
**Amazon WorkSpaces** = Managed Desktop as a Service (DaaS) solution.
**Amazon AppStream 2.0** = Desktop application streaming service.
**AWS AppSync** = Store and sync data across mobile and web apps in real-time.
**AWS Amplify** = Comprehensive **toolkit for building and launching scalable web and mobile apps.**
**AWS Device Farm** = Fully-managed service that tests your web and mobile apps against desktop browsers, real mobile devices, and tablets
**AWS Infrastructure Composer** = Design and build new modern applications. Streamline the composition of modern applications from multiple AWS services.
**AWS Elastic Disaster Recovery (DRS)** = Enables rapid recovery of servers into AWS in case of data loss or disaster.
**AWS DataSync** = Facilitates the transfer of **large amounts of data from on-premises to AWS.**
**AWS Step Functions** = Allows you to build **serverless visual workflows to orchestrate Lambda functions.**

---
```
Basic
   ↓
Developer
   ↓
Business
   ↓
Enterprise
```
## Basic Plan
free
Includes:
- Documentation
- Whitepapers
- AWS Health Dashboard (public)
- Billing support
- Account support
- AWS Discussion Forums
❌ No technical support cases. -> need help? -> AWS Discussion Forums
## Developer Plan
Adds:
- Email support
- Technical support cases
- Best for development/testing
No phone support.
## Business Plan
Includes everything above PLUS:
✅ 24/7
- Phone
- Chat
- Email
Technical support.
Also includes:
- AWS Trusted Advisor (full checks)
- AWS Support API
Critical response:
Production system down
→ **1-hour response**
## Enterprise Plan
The highest level.
Everything in Business PLUS:
- Technical Account Manager (TAM) -> Your personal AWS advisor.
- Concierge Team -> Billing specialists
- Infrastructure Event Management (IEM) -> help prepare Before major events
- Operational Reviews
- Proactive guidance