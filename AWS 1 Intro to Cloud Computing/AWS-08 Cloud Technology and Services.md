## What is the AWS global infrastructure?
Global application = an application **deployed** in **multiple geographies**
- **We use global infrastructure for…**
    - **Decreased latency**
        - Deploy your applications **closer to your users** to **decrease latency**
    - **Disaster Recovery**
        - If an AWS region goes down (earthquake, storms, power shutdown, politics), **can fail-over to another region** and have your **application still working**
    - **Attack protection**
        - **Distributed global infrastructure** is **harder** to attack
- **Regions**
    - **Geographical locations** around the world where AWS has **data centers.** Each region is a separate geographic area with multiple availability zones.
- **Availability Zones (AZs)**
    - **Isolated locations** within AWS regions that are engineered to be operationally independent of each other, with their own power, cooling, and networking infrastructure.
    - They provide redundancy and fault tolerance.
- **Edge Locations**
    - Locations **around the world** to **cache copies of data** closer to users for **faster access.**
## **AWS Route53**
**AWS Route53** =a highly available and scalable Domain Name System (DNS) web service. Route 53 connects user requests to internet applications running on AWS or on-premises.
- **Routing Policies = how traffic is directed to your resources based on various criteria**
    - Simple routing policy
    - Weighted routing policy
    - Latency Routing Policy
    - Failover Routing Policy

**Weighted Routing:** Distributes traffic across multiple resources based on weight values you assign, allowing you to control the proportion of traffic sent to each resource.
**Latency-Based Routing:** Routes traffic to the resource with the **lowest latency** from the user's location, optimizing performance for end users.
**Failover Routing:** Automatically directs traffic to a **standby resource in the event of a failure or outage,** ensuring high availability.
## **AWS CloudFront**
- **AWS CloudFront** = **a content delivery network (CDN) service** built for **high performance, security, and developer convenience.**
    - Content cached at the edge locations
    - Improves user performance
- **S3 Bucket: Used for file distribution and caching at edge locations.**
    - Utilizes CloudFront Origin Access Control (OAC), replacing Origin Access Identity (OAI).
- CloudFront as Ingress: Allows uploads directly to an S3 bucket.
    - Custom Origin Options with CloudFront:
        - Application Load Balancer (ALB)
        - EC2 Instance
        - S3 Website (requires enabling as a static website)
        - Any HTTP Backend
## **The Difference Between CloudFront and S3 Cross Region Replication**

**CloudFront:**
- **Global Edge network.**
- Files cached for a **TTL** (e.g., a day).
- Ideal for **static content requiring global availability.**

**S3 Cross Region Replication:**
- Requires setup for each region where replication is needed.
- Files update in near real-time.
- Read-only access.
- Suitable for **dynamic content requiring low-latency access in specific regions.**
## **S3 Transfer Acceleration**
**S3 Transfer Acceleration** = speeds up **content transfers** to and from **Amazon S3 by as much as 50-500% for long-distance transfer of larger objects.**
Normally: `Your ISP → many internet hops → AWS`
With Transfer Acceleration:
```
Nearest AWS edge
       ↓
AWS private global network
       ↓
S3
```
Normal S3 URL: `bucket.s3.amazonaws.com`
Accelerated endpoint: `bucket.s3-accelerate.amazonaws.com`
## **AWS Global Accelerator**
- **Enhance your application's availability and speed worldwide** with AWS's global network.
- Use AWS's network to find the best path for your application, resulting in a 60% boost in performance.
- Your application gets two special IP addresses (Anycast IPs), **directing traffic through Edge Locations, 2 Anycast IP are created for your application and traffic is sent through Edge Locations**
- Traffic is then routed from Edge Locations to your application, ensuring efficient delivery.
## **AWS Outposts**
**AWS Outposts** = AWS Outposts are “server racks” that offers the same AWS infrastructure, services, APIs & tools to build your own applications on -premises just as in the cloud - **enables organizations** to run **AWS services locally, providing a consistent hybrid cloud experience across on-premises and the cloud.** - **Seamless, local data processing** - **Fully managed, capacity-based pricing**

- **Low-Latency Applications:** Perfect for apps needing **quick access to on-premises data or processing.** - **Data Residency Compliance:** Meets **data sovereignty requirements by keeping data on-premises.**
## **AWS WaveLength**
**AWS WaveLength** = brings AWS infrastructure to the edge of **5G networks**, enabling ultra-low latency applications by processing data closer to end-users.
- **Ultra-low latency** - Edge computing use cases and **ensures compliance with data residency requirements** - **High-bandwidth and secure connection to the parent AWS Region** - No additional charges or service agreements
## **AWS Local Zones**
Small AWS infrastructure locations placed near major cities to reduce latency
**AWS Local Zones** = bring AWS services closer to large population, industry, and IT centers.
- Runs **latency-sensitive applications** - Enable customers to **run applications that require single-digit millisecond latency to end-users or local data processing**
## **AWS Global Applications Architecture**
**Single Region, Single AZ** - All application resources and data are **hosted within a single AWS region and a single availability zone within that region.** - This setup is the **most basic configuration** and **offers limited fault tolerance and resilience.**
- ❌ High Availability
- ❌ Global Latency
- 🟢 LESS Instance Difficulty
**Single Region, Multi AZ** - Application resources are **deployed across multiple availability zones within a single AWS region.** - This setup provides **higher fault tolerance and resilience compared to a single AZ configuration while maintaining simplicity.**
- 🟢 High Availability
- ❌ Global Latency
- 🟠 Instance Difficulty
**Multi Region, Active-Passive** - Applications are **deployed across multiple AWS regions, with one region serving as the primary active region and others as passive standby regions.** - The primary region hosts the active workload and serves user traffic, while standby regions are kept in sync and act as failover targets in case of disaster or regional outages..
- 🟢 Global Reads Latency
- ❌ Global Writes’ Latency
- 🟠 Instance Difficulty
**Multi Region, Active-Active** - Applications are **deployed across multiple AWS regions, with all regions actively serving user traffic simultaneously.** - This setup provides **high availability, fault tolerance, and scalability by distributing the workload across geographically diverse regions.**
- 🟢 Reads’ Latency
- 🟢 Writes’ Latency
- 🔴 Instance Difficulty
# **Other Compute Services**
## **Docker**
- Docker is a **software development platform used to deploy applications.**
    - Applications are packaged in containers that can run on any operating system.
- **Apps exhibit consistent behavior regardless of their deployment environment.**
    - Ensures no compatibility issues across different machines.
    - Results in predictable behavior, reducing workload.
    - Simplifies maintenance and deployment processes.
    - Compatible with any programming language, operating system, and technology.
    - Allows for rapid scaling of containers, with adjustments taking just seconds.
### **Docker Images
- Docker images are **stored in Docker Repositories, which can be public or private.**
- **Public Repository:** Docker Hub ([https://hub.docker.com/](https://hub.docker.com/)) is a popular public repository where you can find base images for various technologies and operating systems, such as Ubuntu, MySQL, NodeJS, Java, and more.
- **Private Repository:** Amazon ECR (Elastic Container Registry) is a private repository service provided by AWS, allowing users to store and manage their own Docker images securely.
## **Elastic Container Service (ECS)**
- **ECS (Elastic Container Service):** A service that allows you to launch Docker containers on AWS.
    - You are responsible for provisioning and maintaining the infrastructure, such as the EC2 instances.
    - **Container Management:** AWS takes care of starting and stopping containers for you.
## **Fargate**
- Launch Docker containers on **AWS without provisioning the infrastructure, eliminating the need to manage EC2 instances.**
- Simplifies the **deployment process as it is a serverless offering.**
    - AWS automatically runs containers for you based on the specified CPU and RAM requirements.
## **Elastic Container Registry (ECR)**
- **Elastic Container Registry (ECR):** A private Docker registry service on AWS.
- ECR is used to **store your Docker images securely**, allowing them to be easily accessed and run by ECS (Elastic Container Service) or Fargate.
## **Lambda**
- **Run code automatically without managing servers**
- Eliminates the need for **provisioning or managing servers.**
- Triggers functions in **response to events from various AWS services.**
    - Limited by time - short executions
    - Automatically scales based on demand, handling any number of requests.
    - Charges only for the compute time used, with no cost when idle.
    - Seamlessly integrates with other AWS services for comprehensive solutions.
### **Benefits of Lambda
- **Simple Pricing Structure:**
    - Charges based on each request and the amount of compute time used.
    - Includes a generous free tier offering up to 1,000,000 AWS Lambda requests and 400,000 GB-seconds of compute time.
    - Fully compatible with the entire range of AWS services.
- **Event-Driven Architecture:**
    - Functions are automatically triggered by AWS as required.
- **Broad Programming Support:**
    - Compatible with numerous programming languages.
- **Effortless Monitoring:**
    - Seamless integration with AWS CloudWatch for convenient tracking and management.
- **Scalable Performance:**
    - Easily upgrade resources per function, with up to 10GB of RAM available.
    - Enhanced RAM capacity boosts both CPU performance and network capabilities.

Use **AWS Lambda** over **standard compute like EC2 instances** when:
1. **Event-driven workloads**: Triggered by events (e.g., API calls, S3 uploads).
2. **Short-lived tasks**: Jobs that run for seconds or minutes.
3. **No infrastructure management**: You don't want to manage servers.
4. **Cost-efficiency**: Pay only for execution time, not idle time.
Choose **EC2** for long-running, persistent, or highly customizable workloads.

## **Amazon API Gateway**
- Fully **managed service for API management.**
- Serverless and scalable.
- Supports RESTful and WebSocket APIs.
- Includes security, user authentication, and API throttling.
- Provides API keys and monitoring features.
## **AWS Batch**
- Fully managed **batch processing at any scale.**
    - Can efficiently run hundreds of thousands of computing batch jobs on AWS.
- Batch jobs have a defined **start and end**, as opposed to continuous jobs.
- Dynamically launches EC2 instances or Spot Instances as needed.
- Automatically provisions the required amount of compute and memory resources.
    - Allows submission or scheduling of batch jobs, with AWS Batch handling the execution.
    - Batch jobs are defined as Docker images and run on ECS (Elastic Container Service).
- Helps optimize costs and reduces focus on infrastructure management.
**Batch**
- No execution time limit
- Supports any runtime packaged as a Docker image
- Uses EBS or instance store for disk space
- Relies on EC2 (self-managed or AWS-managed)

**Lambda**
- Time-limited execution
- Supports only specific runtimes
- Limited temporary disk space
- Fully serverless
## **AWS Lightsail**
**A simplified, beginner-friendly way to launch servers, databases, and websites on AWS**
**“Easy AWS”** or **“simplified EC2”**
- Provides virtual servers, storage, databases, and networking.
- Offers low and predictable pricing.
- Simpler alternative to using EC2, RDS, ELB, EBS, Route 53, etc.
- Ideal for individuals with **limited cloud experience.**
- Allows setup of notifications and monitoring for Lightsail resources.
- **Use Cases:**
    - Simple web applications with templates for LAMP, Nginx, MEAN, Node.js, etc.
    - Websites with templates for WordPress, Magento, Plesk, Joomla, etc.
    - Development and testing environments.
    - Offers high availability but lacks auto-scaling and has limited AWS integrations.
# **Cloud Integration
### **Why do we need to integrate our accounts?**
- When we start deploying multiple applications, they need to communicate with one another
- There are two patterns of application communication
    1. **Synchronous communications (application to application)**
    2. **Asynchronous / Event based (application to queue to application)**
- Synchronous communication can be problematic during **sudden spikes in traffic.**
- Decoupling applications using AWS services:
    - **SQS:** queue model for managing message queues.
    - **SNS:** pub/sub model for sending messages to multiple recipients.
    - **Kinesis:** real-time data streaming model for processing large amounts of data.
- These services allow scaling independently from the application.
## **Amazon SQS – Simple Queue Service**
SQS is a fully managed, serverless service used to **decouple applications.**
- It can scale from handling 1 message per second to 10,000s per second.
- Messages have a default retention of 4 days, with a maximum of 14 days.
- There's **no limit to the number of messages in the queue.**
- Messages are deleted after they're successfully processed by consumers.
- Offers low latency, with less than 10 ms on publish and receive operations.
- Consumers can share the workload to read messages, allowing for horizontal scaling.
- FIFO = First In First Out (ordering of messages in the queue)
- Messages are processed in order by the consumer
## **Amazon Kinesis**
- Kinesis is a managed service for **real-time big data streaming.**
- It **collects, processes, and analyzes real-time streaming data at any scale.**
- **It offers several components:**
    - **Kinesis Data Streams:** Low-latency streaming to ingest data at scale from hundreds of thousands of sources.
    - **Kinesis Data Firehose:** Loads streams into S3, Redshift, ElasticSearch, etc.
    - **Kinesis Data Analytics:** Performs real-time analytics on streams using SQL.
    - **Kinesis Video Streams:** Monitors real-time video streams for analytics or ML.
**CloudFront = Deliver video to viewers**  
**Kinesis = Process video data streams**
## **Amazon SNS**
- In SNS, event publishers send **messages to one SNS topic.**
- **Basically, sending one message to many users**
- There can be multiple event subscribers listening to the SNS topic notifications.
- Each subscriber receives all the messages sent to the topic.
- SNS supports up to 12,500,000 subscriptions per topic and has a limit of 100,000 topics.
## **Amazon MQ**
- SQS and SNS are AWS' proprietary protocols for cloud-native services.
- Traditional applications on-premises may use open protocols like MQTT, AMQP, STOMP, Openwire, WSS.
- Amazon MQ is a **managed message broker service for migrating such applications to the cloud.**
- Amazon MQ doesn't scale as much as SQS/SNS and runs on servers, supporting Multi-AZ with failover.
- Amazon MQ offers both queue features (like SQS) and topic features (like SNS).
- Amazon MQ is a managed message broker service for:
    - RabbitMQ
    - ActiveMQ
## **AWS CloudFormation**
AWS CloudFormation is designed for **Managing infrastructure as code (IaC)** in AWS

It allows you to define and provision AWS resources using templates (JSON or YAML) By deploying the same template to different AWS Regions, such as:
- EC2 instances
- VPCs
- Subnets
- IAM roles
- RDS databases
## **AWS Trusted Advisor**
AWS Trusted Advisor is a **cloud optimization and guidance service** that continuously analyzes your AWS environment and gives recommendations to improve:
- 💰 Cost optimization
- 🔒 Security
- ⚡ Performance
- 🔁 Fault tolerance
- 📊 Service limits
## **Amazon Kendra**
Amazon Kendra is a **machine learning–powered enterprise search service** that helps organizations search across large amounts of data using **natural language questions** instead of traditional keyword queries.
