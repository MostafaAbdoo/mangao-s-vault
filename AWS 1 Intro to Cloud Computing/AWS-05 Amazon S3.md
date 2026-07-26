**Amazon S3 (Simple Storage Service):**
- A highly scalable, durable, and secure object storage service for storing and retrieving any amount of data.
    - Often called **‘infinitely scaling’ storage**
- Offers features like **versioning, lifecycle policies, and fine-grained access controls** for managing data.
- Supports **data transfer acceleration** and integration with other AWS services for data processing and analytics.
- Commonly used for **backup and recovery, data archiving, content distribution, and as a data lake for big data analytics.**
- Amazon S3 facilitates the **storage of objects (files) within "buckets" (directories):**
    - Bucket names must be globally unique across all regions and accounts.
    - Buckets are established at the regional level.
- Despite S3 appearing as a global service, **buckets are confined to a specific region.**
- **Naming conventions mandate:**
    - No uppercase letters or underscores.
    - Length between 3 and 63 characters.
    - Not in the format of an IP address.
    - Commencing with a lowercase letter or number.
    - Excluding the prefix "xn--" at the start.
    - Avoiding the suffix "-s3alias" at the end.
**S3 Bucket Policies**
A **bucket policy** is a set of rules attached to a bucket in Amazon S3 that controls: **Who can access the bucket and what they can do** 
- **JSON**: Policies are written in **JSON format**
- Buckets and objects: You can apply rules to: The **bucket itself** or the **objects inside it**
- Permit or Restrict
- APIs permitted or restricted: You are controlling **API calls**
- User or account affected by the policy
**Use the S3 bucket policy to:**
- Enable public access to the bucket
- Mandate encryption on uploaded objects
- Provide access permissions to another account (Cross-Account Access)

# **Characteristics of Amazon S3**
**Durability:**
- Amazon S3 offers high durability of 99.999999999% (11 9's) for objects, ensuring data is protected across multiple Availability Zones.
- With this level of durability, if you store 10 million objects with Amazon S3, you can expect to lose a single object once every 10,000 years.
- This durability applies to all storage classes.
**Availability:**
- Refers to how readily available a service is for use.
- Availability varies depending on the storage class.
- For example, S3 Standard has 99.99% availability, which equates to potential unavailability of approximately 53 minutes per year.
>You might temporarily not access a file (availability issue),  but it’s still not lost (durability intact)

**Static Websites and Versioning:**
Amazon S3 can be used to host a **static website**.S3 acts like a web server and:
- Serves your HTML files
- Makes them accessible via a public URL
`http://bucket-name.s3-website-REGION.amazonaws.com` The exact format slightly changes depending on region, but idea is the same.

**Versioning:**
- You can version your files in Amazon S3
- It is enabled at the bucket level
- Same key overwrite will change the “version”: 1, 2, 3….
- It is best practice to version your buckets
- Protect against unintended deletes (ability to restore a version)
- Easy roll back to previous version
- Any file not versioned before versioning is enabled will be assigned the version "null".
- Suspending versioning will not remove any previously stored versions.

**Replication (Cross Region Replication and Same Region Replication):**
**Versioning must be enabled on both the source and destination buckets for replication.**
Supports:
    - Cross-Region Replication (CRR) Same-Region Replication (SRR).
    - Buckets can be in different AWS accounts. You can replicate: Within same account - Across different AWS accounts
    - Copying of objects is **asynchronous.** (NOT instant)
    - Proper **IAM permissions must be granted to S3 for replication.**
# **S3 Storage Classes**
**S3 Storage Classes Overview:**
- Designed for varying use cases based on **access frequency, durability, and cost.**
- Includes options like Standard, Intelligent-Tiering, Standard-IA, and One Zone-IA.
- Enables **cost optimization through diverse pricing structures.**
- Supports **automatic data migration with lifecycle policies.**
- Provides **global data storage** with high availability.
- Allows easy transition between classes to meet changing needs.

**All S3 Storage Classes:**
1. **Amazon S3 Standard - General Purpose**
2. **Amazon S3 Standard-Infrequent Access (IA)**
3. **Amazon S3 One Zone-Infrequent Access**
4. **Amazon S3 Glacier Instant Retrieval**
5. **Amazon S3 Glacier Flexible Retrieval**
6. **Amazon S3 Glacier Deep Archive**
7. **Amazon S3 Intelligent Tiering**

**S3 - Encryption**
Server Side Encryption (Default):
- Server **encrypts the file after receiving it**

**IAM Access Analyzer for S3**
- Ensures only **intended individuals have access to your S3 buckets.**
- Evaluates S3 Bucket Policies, S3 ACLs, and S3 Access Point Policies.
- Powered by IAM Access Analyzer.
- Examples include **publicly accessible buckets or buckets shared with other AWS accounts.**

## **Shared Responsibility Model: S3**
- **AWS Responsibility:**
    - Guarantee unlimited storage.
    - Provide encryption for stored data.
    - Ensure data separation between different customers.
    - Ensure AWS employees cannot access your data.
- **Your Responsibility:**
    - Configure S3 buckets.
    - Set bucket policies and public access settings.
    - Manage IAM users and roles.
    - Enable encryption for data storage.
## S3 Migration
Migrating data to Amazon S3 involves transferring existing data from on-premises storage systems or other cloud providers to S3. This process can be performed using various methods, including AWS DataSync(Automated transfer service and Faster than normal uploads), AWS Snow Family (Snowball, Snowcone, Snowmobile), or through direct data transfer methods over the internet. S3 Migration enables organizations to leverage the scalability, durability, and cost-effectiveness of S3 for their storage needs.
### **The Snow Family**
When data is HUGE (TBs–PBs), Instead of internet:
- AWS sends you a device
- You copy data locally
- Ship it back to AWS
AWS Snowball devices are:
1. **AWS Snowball Edge Storage Optimized:** Designed for large-scale data migrations and capacity-oriented workloads, this device offers 210 TB of NVMe storage and can transfer data at up to 1.5 gigabytes per second.
2. **AWS Snowball Edge Compute Optimized:** Suited for high-performance edge computing workloads, this device provides 104 vCPUs, 416 GB of RAM, and 28 TB of NVMe SSD storage. It's ideal for applications such as machine learning inference and video analytics in remote or disconnected environments.
### **Snow Family: Usage Process**
- **Request Snowball Devices:** Order Snowball devices through the AWS Management Console for delivery to your location.
- **Install Software:** Install the Snowball client or AWS OpsHub on your servers to facilitate data transfer.
- **Connect and Copy:** Connect the Snowball device to your servers and copy files using the client software.
- **Return Device:** Ship the device back to AWS once the data transfer is complete. The device is pre-configured to return to the correct AWS facility.
- **Data Transfer to S3:** Upon receipt, AWS will transfer the data from the Snowball device into an Amazon S3 bucket.
- **Data Wiping:** After the data transfer, the Snowball device is completely wiped to ensure security and privacy.
### **AWS OpsHub**

- Historically, using Snow Family devices required a **Command Line Interface (CLI) tool.**
    - Today, AWS OpsHub, a software installed on your computer or laptop, can be used to **manage Snow Family devices.**
- **OpsHub enables:**
    - Unlocking and configuring single or clustered devices.
    - Transferring files to and from the device.
    - Launching and managing instances running on Snow Family devices.
    - Monitoring device metrics such as storage capacity and active instances.
    - Launching compatible AWS services on your devices, such as Amazon EC2 instances, AWS DataSync, and Network File System (NFS).
## **Hybrid Cloud for Storage**
- **Hybrid Cloud Approach: AWS promotes a hybrid cloud model where part of your infrastructure is on-premises and part is on the cloud.**
- **This approach may be due to:**
    - Long cloud migration processes.
    - Specific security requirements.
    - Compliance requirements.
    - Overall IT strategy.
- **S3 and On-Premises Integration:** Given that S3 is a proprietary storage technology, unlike EFS or NFS, the question arises: How do you expose S3 data on-premises?
    - **Solution:** **AWS Storage Gateway** provides a seamless way to connect on-premises environments with S3 storage, enabling hybrid cloud storage solutions!
## **AWS Storage Gateway**
- **Hybrid Storage with AWS S3:**
    - Connects **on-premise data systems** seamlessly with AWS Cloud.
    - Facilitates **smooth integration and data movement** between local storage and S3.
- **Key Use Cases:**
    - **Disaster recovery:** Ensures data continuity by replicating on-premises data to the cloud.
    - **Backup & restore:** Provides off-site backups in the cloud for additional security and accessibility.
    - **Tiered storage**: Moves less frequently accessed data to the cloud to optimize costs and performance.
- **AWS Storage Gateway Types:**
    1. **File Gateway:** Integrates on-premises environments with cloud-based storage for file data.
    2. **Volume Gateway:** Uses block-based storage interfaces compatible with existing applications.
    3. **Tape Gateway:** Simulates a physical tape library with virtual tape storage in AWS.
- _**Detailed knowledge of Storage Gateway types is not required for the exam**_