#### Amazon S3
- Amazon S3 is an **object storage** service that stores unlimited amounts of data.
- Data is stored as **objects** inside **buckets** not as files in a file system.
- Maximum size of a single object: **5 TB**. For uploads > 5 GB, use **multipart upload** (recommended for > 100 MB).
- Each object has a **globally unique URL** (bucket names are globally unique, but buckets are Regional).
- An object consists of: **key** (name), **value** (data), **version ID**, **metadata**, and **sub-resources**.
**Amazon S3 components**
- **Buckets**
    - Containers for storing objects in Amazon S3.
    - Unique name with a Region-specific endpoint (e.g., `https://demo-9.s3.eu-west-3.amazonaws.com/`).
    - Define access control, storage charges, and usage reporting.
- **Objects**
    - Files stored in S3, along with metadata (e.g., `https://demo-9.s3.eu-west-3.amazonaws.com/Cup-of-Hot-Chocolate.png`).
    - Can be any format text, video, images, or binary data.
    - Metadata includes content type to help browsers render files correctly.
- **Object Key**
    - A unique identifier for each object within a bucket.
- **Region-Specific Storage**
    - Buckets are **Regional** objects stay in the selected Region unless explicitly replicated.
    - Example: `us-west-2` for the Oregon Region.
Key S3 benefits: **11 nines** of durability, **99.99%** availability (Standard), unlimited capacity.

**Amazon S3 uses prefixes to simulate a folder structure**
Amazon S3 has **no true file system**. Instead, it uses **key prefixes** (e.g., `images/2023/`) to create a human-friendly hierarchy within a flat namespace.

**Redundancy in Amazon S3**
When you upload an object to S3, Amazon S3 automatically replicates your data across multiple AZs within that region
#### Amazon S3 bucket types
![[Pasted image 20261010130020.png|521]]
**The one that matters most: Directory Buckets vs. General Purpose Buckets**:
The SAA-C03 exam can present a scenario where you need to choose between them. The decision comes down to one question: **does the workload need consistent single-digit millisecond latency on every request?**
- **Yes** → **Directory Bucket** with S3 Express One Zone. Accept the single-AZ trade-off for the performance gain. Examples: ML model training data, real-time inference preprocessing, latency-sensitive analytics.
- **No** → **General Purpose Bucket**. Multi-AZ durability, all storage classes available, lower cost per GB.
> - **S3 is not a file system** don't confuse it with EFS or EBS.
> - Bucket names are **globally unique** but data stays **in the Region** you choose.
> - Max object size = **5 TB**; single PUT limit = **5 GB** (use multipart for anything larger).
> - Know when to use each storage class: Glacier = archival; Standard-IA = infrequent but fast access needed; One Zone-IA = re-creatable data only (single AZ = no HA).
> - **S3 is not suitable for OS volumes** use EBS for that.
#### Amazon S3 use cases
S3 shines in four major scenarios: **media hosting**, **static website hosting**, **analytics data lakes**, and **backup & archival**.

**Media hosting**
- Stores and distributes media files (videos, photos, music) via unique HTTP URLs.
- Can serve content directly *or* act as an origin for **Amazon CloudFront** (CDN).
- Scales elastically to handle high traffic and sudden demand spikes.
- Ideal for dynamic, content-heavy websites (e.g., video and photo-sharing platforms).
**Host static websites**
- Hosts static websites with HTML, images, videos, and client-side scripts (e.g., JavaScript).
- Requires enabling **S3 website hosting** and setting a **bucket policy** for public access.
- Eliminates the need for a traditional web server or virtual machine.
- Offers **low-cost, scalable, and highly available** hosting.
**Anti-pattern**: Don’t use S3 to serve a dynamic web app (no server-side code execution) use EC2, Lambda, or App Runner for that.
**Data store for analytics**
- Supports **high-volume analytics** (e.g., financial analysis, clickstream tracking, media transcoding) due to **horizontal scaling** and **concurrent transactions**.
- Works with **EC2 Spot Fleets** or **Amazon EMR** for cost-effective compute.
- **Process Flow:**
    1. Compute resources (Spot Fleet/EMR) spin up based on availability.
    2. Raw data is extracted from **S3** and other sources.
    3. Data is processed and transformed.
    4. Processed data is stored back in **S3**.
    5. Compute resources terminate to **reduce costs**.
    6. **Amazon QuickSight** or other BI tools analyze the data.
This **serverless, scalable, and cost-efficient** approach makes S3 a key component in **big data workflows**
![[Pasted image 20261010132202.png|513]]
**Backup and archive critical data**
- **Highly durable & scalable** (11 nines), making it ideal for backups.
- Works for **on-premises backups** (via AWS Storage Gateway or DataSync) and **EC2 instance data**.
- **Cross-Region Replication (CRR)** automatically and asynchronously copies objects to another Region for disaster recovery.
- **S3 Glacier** tiers offer cost-effective long-term archival from millisecond retrieval (Instant) to 12-hour retrieval (Deep Archive).
- Use **S3 Object Lock** (WORM) to prevent deletion during a retention period critical for regulated industries.
---
- Requires **write permissions** to upload; reads require appropriate bucket/IAM policy.
- **Default encryption**: All objects are automatically encrypted at rest with **SSE-S3** (AES-256, AWS-managed keys) enabled by default since January 2023.
**Options for uploading objects to Amazon S3**:
- **AWS Management Console**
- **AWS CLI**
	- `aws s3 cp` and `aws s3 sync` commands; supports scripted, automated transfers.
- **AWS SDKs and REST API**
- **S3 Transfer Acceleration**
	- **Boosts Upload Speeds** – Speeds up file transfers over long distances to an S3 bucket.
	- **Uses Amazon CloudFront Edge Locations** – Data first reaches the nearest CloudFront edge before being routed to S3 via optimized AWS networks.
	- **Secure & Efficient** – Enhances transfer reliability while maintaining security.
	- Uses **AWS Global Backbone**

###### **AWS Transfer Family**
AWS Transfer Family is a **fully managed** service that lets you move files **into and out of S3 or EFS** using legacy file transfer protocols no custom server setup required.
**Supported protocols:** SFTP, FTPS, FTP, AS2
**Common use cases:**
- Third-party vendors uploading files to your S3 data lake (e.g., partner EDI feeds via AS2)
- Distributing data to customers over SFTP without building your own server
- Internal file transfers across teams using existing FTP tooling
**Key benefits:** Fully managed and scalable, no infrastructure to maintain, pay-per-use pricing, integrates directly with S3 and EFS.

###### **AWS DataSync**
AWS DataSync is a fully managed **online data transfer** service for migrating data **to and from AWS** storage at high speed.
**Key capabilities:**
- Transfers data between **on-premises file systems** (NFS, SMB) and AWS storage (S3, EFS, FSx)
- Also moves data **between AWS storage services** (e.g., S3 to EFS)
- Uses an **agent** deployed on-premises (or agentless for AWS-to-AWS transfers)
- Automatically handles encryption, checksums, bandwidth throttling, and retry
- Up to **10x faster** than open-source tools
###### **AWS Snowball Edge**
AWS Snowball Edge is a rugged, portable device with built-in **storage** and **compute** capabilities for edge processing and secure data transfer to AWS.
**Key Features:**
- **Faster Data Transfer** – Ships data via a regional carrier, bypassing slow internet speeds.
- **Edge Computing** – Processes data locally before transferring to AWS.
- **Automatic E Ink Labels** – Simplifies logistics with dynamic shipping labels.
**Device Configurations:**
- **Storage Optimized** – 210 TB storage for large-scale data migration.
- **Compute Optimized** – More processing power for running edge applications.
**The shared workflow**:
1. A device is shipped to you by your region's carrier
2. Use the device
3. Your region's carrier returns the device to AWS

**When to use DataSync vs. Snowball:**
- Use **DataSync** when you have sufficient network bandwidth and need automated, recurring transfers.
- Use **Snowball** when your data volume is too large to move over the network in a reasonable time (generally > 10 TB or very slow WAN links).
#### S3 Storage classes
**General Purpose**
- **S3 Standard** – Default class. High durability, availability, and performance. Use for frequently accessed, active data.
**Intelligent-Tiering**
- **S3 Intelligent-Tiering** – Automatically moves objects between access tiers (Frequent, Infrequent, Archive) based on usage. No retrieval fees. Small monthly monitoring fee per object. Best for unpredictable access patterns.
**Infrequent Access (IA)**
- **S3 Standard-IA** – Lower storage cost with per-GB retrieval fee. 30-day minimum. Use for infrequently accessed data that still needs millisecond retrieval (e.g., old project files, compliance records).
- **S3 One Zone-IA** – Same as Standard-IA but stored in **a single AZ** (lower cost, but no cross-AZ redundancy). Use only for data you can recreate if lost.
**Archive**
- **S3 Glacier Instant Retrieval** – Millisecond retrieval, 90-day minimum. Best for medical images or news archives accessed once a quarter.
- **S3 Glacier Flexible Retrieval** – Retrieval in minutes to hours. 90-day minimum. Ideal for backup and disaster recovery.
- **S3 Glacier Deep Archive** – Cheapest storage tier. 12–48 hour retrieval. 180-day minimum. For data retained 7–10 years (compliance, legal archives).
**On-Premises**
- **S3 on Outposts** – Brings S3 to AWS Outposts hardware for on-site data residency
#### Amazon S3 Lifecycle Management
An **S3 lifecycle configuration** automates data transitions and expirations based on defined rules.
- **Transition Actions**
    - Move objects to a lower-cost storage class after a set period.
    - Example: Transfer objects to **S3 Standard-IA** after **30 days** or **S3 Glacier Flexible Retrieval** after **1 year**.
    - **Note**: Transition requests incur costs.
- **Expiration Actions**
    - Define when objects are **permanently deleted** from S3.
    - Amazon S3 automatically deletes expired objects.
**Benefits of Lifecycle Policies**
- **Cost Optimization**: Automatically transitions data to lower-cost storage as it ages.
- **Automated Management**: No manual intervention needed for data archiving or deletion.
- **Flexible Scope**: Policies can be applied to individual objects or entire buckets.
![[Pasted image 20261011180958.png]]
---
**S3 Replication**
S3 Replication automatically copies objects from a **source bucket** to a **destination bucket** in the same region or in a different region. Versioning must be enabled on both buckets.
**Requires S3 Versioning** to be enabled on the bucket.
	**Key points:**
	- Replication is **asynchronous** and applies to **new objects** only (not retroactive by default).
	- Objects are replicated with their **storage class** unless you override it in the replication rule.
	- **Delete markers** are not replicated by default (optional setting).
	- Requires an **IAM role** with permission to replicate objects.

**Amazon S3 Versioning**
Amazon S3 Versioning protects objects from accidental overwrites and deletes by keeping **multiple versions** of every object **within the same bucket**.

| **Action**                         | **Versioning Enabled**                                                           | **Versioning Disabled or Suspended**        |
| ---------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------- |
| Upload an object with the same key | Creates a new object with a different version ID; both versions are retrievable. | Overwrites the original object permanently. |
| Delete                             | Adds a **delete marker**; the object is still retrievable by version ID.         | Deletes the object permanently.             |
	**Bucket-level states:**
	- **Disabled** (default) — no versioning; objects are overwritten permanently
	- **Enabled** — all versions are kept; each has a unique version ID
	- **Suspended** — stops creating new versions but preserves existing ones
**S3 Object Lock**
S3 Object Lock is a data protection feature that implements a **Write Once, Read Many (WORM)** model. It prevents objects from being deleted or overwritten for a fixed amount of time or indefinitely, providing the ultimate defense against **ransomware, malicious insiders, and accidental deletion** and **requires S3 Versioning** to be enabled on the bucket.
- **Governance Mode**: Users cannot overwrite or delete an object version unless they have special IAM permissions `s3:BypassGovernanceRetention`
- **Compliance Mode**: No one can delete or overwrite the object—not even the AWS Root Account. The retention period cannot be shortened or bypassed by anyone, including AWS support until the timer expires
**Retention Periods**
- Specifies a **fixed duration** (e.g., 3 years).
- During this window, the object version is protected.
- Once the timestamp expires, the object can be deleted normally.
**Legal Holds**
- Provides an **indefinite lock** with no expiration date.
- It functions like an On/Off switch. It stays locked until a user with the `s3:PutObjectLegalHold` permission explicitly removes it.
- **Independent of retention periods:** An object can have both a 3-year retention period and a legal hold simultaneously. Even if the 3 years expire, the legal hold will keep it locked.
---
**S3 Storage Lens is an observability tool.** It aggregates metrics across your entire AWS Organization, specific accounts, regions, or buckets into a single centralized dashboard to help you optimize costs and improve security

**Object Metadata**
Every S3 object carries **metadata** key-value pairs that describe the object. There are two types:
- **System metadata** Managed by S3. Examples: `Content-Type`, `Content-Length`, `Last-Modified`, `ETag`. You can set some of these (like `Content-Type`) at upload time.
- **User-defined metadata** Custom key-value pairs you attach at upload. Keys must be prefixed with `x-amz-meta-`. Example: `x-amz-meta-author: john`. Retrieved in the object's HTTP response headers.
**Note:** Metadata cannot be changed after an object is uploaded without making a **copy** of the object (using `CopyObject`). This is a common gotcha on the exam.

**Object Tags**
Tags are **key-value labels** (up to 10 per object) you can apply to objects at any time even after upload.
**What tags are used for:**
- **Cost allocation** filter S3 costs in Cost Explorer by tag
- **Lifecycle rules** apply transitions/expirations to only tagged objects
- **Access control** use tag-based conditions in IAM or bucket policies
- **S3 Replication** replicate only objects with specific tags

**Copying Objects**
Use `CopyObject` (Console, CLI, or SDK) to:
- **Change storage class** without re-uploading (e.g., Standard → Standard-IA)
- **Update metadata** (the only way to change metadata post-upload)
- **Move objects** between buckets or Regions (copy + delete)
- **Encrypt an existing unencrypted object** by copying it with encryption settings
For objects > 5 GB, use **multipart copy** (the SDK handles this automatically).

**S3 Event Notifications**
S3 can **automatically trigger actions** when certain events happen in a bucket — without polling.
**Supported event types:**
- Object created (`s3:ObjectCreated:*`): covers Put, Post, Copy, multipart complete
- Object deleted (`s3:ObjectRemoved:*`)
- Object restore initiated/completed (Glacier)
- Replication failures
**Destinations:**
- **Amazon SNS:** fan-out notifications to multiple subscribers
- **Amazon SQS:** queue events for decoupled processing
- **AWS Lambda:** run code in response to the event (most common)
- **Amazon EventBridge:** route events to 20+ targets with filtering rules
**EventBridge vs. native notifications:** EventBridge supports more targets and advanced filtering. Native S3 notifications (SNS/SQS/Lambda) are simpler and have lower latency for single-destination use cases.
#### Amazon S3 Security configurations
- All S3 buckets and objects are **private by default** no public access without explicit configuration.
- **S3 Block Public Access** is enabled by default at both the **account level** and **bucket level**. Account-level settings *override* bucket/object settings.
- Default encryption: **SSE-S3** (AES-256, Amazon-managed keys) is automatically applied to all new objects since January 2023.
- Apply the **principle of least privilege** grant only the minimum permissions required.
- Use **IAM policies** or **bucket policies** rather than ACLs (ACLs are legacy and not recommended for most use cases).
**Server-side encryption (AWS manages decryption on download):**
- **SSE-S3** – AWS manages the keys using AES-256. Default for all buckets. Zero extra cost.
- **SSE-KMS** – Uses AWS KMS customer-managed keys (CMKs). Adds audit trail in CloudTrail. Slight performance/cost overhead per request. Good for compliance.
- **SSE-C** – You provide your own encryption keys per request. AWS encrypts/decrypts but never stores your key.
**Client-side encryption:**
- You encrypt data **before** uploading to S3. You own and manage the entire encryption process and keys.

|   |   |
|---|---|
|Amazon S3 access points|Configures access with names and permissions specific to each  <br>application|
|Pre-signed URLs|Grants time-limited access to others with temporary URLs|
|AWS Trusted Advisor|Provides a bucket permission check|
#### S3 Object Lambda
Enhances S3 by allowing custom processing of data before returning it to an application.
**Custom Processing:** Use AWS Lambda to modify data retrieved via S3 GET requests.
**Use Cases:**
- **Filtering** rows in datasets.
- **Image Processing** (e.g., resizing dynamically).
- **Data Redaction** (e.g., masking sensitive info).
- **On-the-fly transformations** without modifying stored data.
---
**You pay for:**
- Storage: GB stored per month (varies by Region and storage class)
- Requests: PUT, COPY, POST, LIST, GET operations (rates differ by class)
- Data retrieval: applies to IA and Glacier tiers (per GB retrieved)
- Lifecycle transition requests
**You do NOT pay for:**
- Data transferred **in** from the internet
- Data transferred **out to CloudFront**
- Data transferred between S3 buckets **in the same Region**
- The **first 100 GB/month** of data transferred out to the internet

**Amazon Macie** uses ML to discover and classify sensitive data (PII, credentials) stored in S3 a common exam answer for data privacy requirements.
#### Amazon S3 static website hosting
S3 can serve HTML, CSS, JavaScript, images, and other static files directly over HTTP no web server or EC2 instance required.
**What it supports:**
- Static HTML, CSS, JavaScript
- Image, video, and audio files
- Client-side routing (single-page apps, if configured correctly)
**What it does NOT support:**
- Server-side code (PHP, Python, Node.js, etc.)
- HTTPS by default you need **CloudFront** in front of S3 to serve over HTTPS
**How to enable static website hosting**
1. **Create an S3 bucket** 
2. **Disable Block Public Access**
3. **Enable Static Website Hosting** under bucket **Properties** → set the **index document** (e.g., `index.html`) and optionally an **error document** (e.g., `error.html`) unlike **REST API Endpoint**
4. **Add a bucket policy** to allow public read access
**Adding HTTPS with CloudFront**:
S3 static website hosting only supports **HTTP**, not HTTPS. To add HTTPS:
- Create a **CloudFront distribution** with your S3 bucket as the origin.
- Request a free SSL/TLS certificate via **AWS Certificate Manager (ACM)**.
- Point your custom domain (via Route 53) to the CloudFront distribution.
This pattern (S3 + CloudFront + ACM + Route 53) is a standard architecture for hosting secure, globally distributed static websites on AWS
#### Amazon S3 Files
Before S3 Files, architects had to choose:
- **Amazon S3** unlimited scale, low cost, 11 nines durability, but object-based: no in-place edits, no standard file tools.
- **Amazon EFS** true NFS file system, but separate from S3: data lives in two places, requires sync pipelines, and you pay to store everything twice.
> **Plain English:** S3 Files makes your S3 bucket behave like a shared network drive to any EC2, container, or Lambda while the data stays in S3 the whole time.

**What is Amazon S3 Files?**
**Amazon S3 Files** is a shared, high-performance file system that mounts an S3 bucket directly onto AWS compute resources using the **NFS v4.1+** protocol. It is built on top of **Amazon EFS** under the hood, giving you EFS-grade performance while your data remains in S3.
**Key characteristics:**
- **Full file system semantics** create, read, update, and delete files using standard POSIX operations
- **~1ms latency** for actively accessed data (cached on high-performance storage)
- **Data never leaves S3** the bucket is the source of truth at all times
- **Bidirectional sync** changes via the file system reflect in S3 within minutes; changes in S3 are visible in the file system within seconds
- **Up to 25,000 concurrent connections** from any mix of EC2, ECS, EKS, Fargate, Lambda, and Batch
- **No code changes required** if your app works with files, it works with S3 Files
**Use Cases**
- **ML training pipelines** training data lives in S3; S3 Files lets frameworks access it directly without staging to EFS first.
- **Agentic AI systems** multiple agents share the same S3 bucket as a file-based workspace simultaneously.
- **Data analytics** file-based tools (Spark, Pandas on EMR) query S3 data lakes directly.
- **Multi-team environments** scope access to a key prefix so each team sees only their slice of the bucket.
**Security**

|Layer|Mechanism|
|---|---|
|Access control|IAM identity policies + S3 resource policies at both file system and object level|
|Encryption in transit|TLS 1.3 (always on)|
|Encryption at rest|SSE-S3 (default) or SSE-KMS (customer-managed keys)|
|File permissions|POSIX UID/GID stored as S3 object metadata|
|Monitoring|Amazon CloudWatch metrics + AWS CloudTrail for management events|

---

| **Amazon S3**               | **Amazon S3 Files**              | **Amazon EFS**                                                  |                                                                           |
| --------------------------- | -------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Storage type**            | Object                           | Object + File system                                            | File (NFS)                                                                |
| **Access protocol**         | REST API (HTTPS)                 | NFS v4.1+                                                       | NFS v4.1                                                                  |
| **Edit files in place**     | ❌ (must replace full object)     | ✅ (full POSIX semantics)                                        | ✅                                                                         |
| **Concurrent access**       | Via API calls                    | Up to 25,000 connections                                        | Thousands of instances                                                    |
| **Latency**                 | Milliseconds (API overhead)      | ~1ms (cached active data)                                       | Sub-millisecond                                                           |
| **Data location**           | S3 bucket                        | Stays in S3                                                     | Separate EFS storage                                                      |
| **S3 APIs still work**      | ✅                                | ✅ (simultaneously)                                              | ❌                                                                         |
| **Automatic scaling**       | ✅                                | ✅                                                               | ✅                                                                         |
| **Durability**              | 11 nines                         | 11 nines (S3-backed)                                            | 11 nines                                                                  |
| **Best for**                | APIs, data lakes, backups, media | Shared file access to existing S3 data, AI agents, ML pipelines | Shared file workloads with no S3 dependency (web servers, CMS, home dirs) |
| **Data duplication needed** | N/A                              | ❌ (data stays in S3)                                            | ✅ (must copy from S3 to EFS)                                              |
**The simplest decision rule:**
- Data already in S3 + need file system access → **S3 Files**
- New workload needing shared file storage with no S3 link → **EFS**
- Windows workloads → **FSx for Windows File Server**
- On-prem NAS migration or HPC → **FSx (ONTAP or Lustre)**

**The core insight:** With S3 Files, you pay EFS-level rates ($0.30/GB) only on your **active working set** (data in the cache). Everything else in the bucket stays at S3 rates (~$0.023/GB). The bigger the gap between your total data and your hot data, the more you save vs. EFS.
![[Pasted image 20261015175247.png]]
**When S3 Files saves money vs. EFS**
- **Large datasets, small active subset** 1 PB bucket with 1 TB hot = pay S3 rates on 999 TB, cache rates on only 1 TB
- **Data already lives in S3** no copy cost, no migration, no duplicate storage bill
- **Read-heavy large-file workloads** large sequential reads bypass the cache entirely; you pay only S3 GET request rates
**When EFS may still be the better fit**
- **Small, entirely hot datasets** if 100% of your data is constantly active and there's no S3 dependency, EFS avoids the overhead of paying both S3 storage and cache fees
- **Extremely write-heavy small-file workloads** every write goes to the cache at $0.06/GB; at very high write volumes this adds up
- **No data in S3 to begin with** EFS is simpler when your workload has no S3 origin
- **Windows or NAS migration workloads** use FSx, not S3 Files
#### Amazon EFS
Amazon EFS is a **fully managed NFS (Network File System)** that automatically scales storage capacity up and down as you add or remove files no provisioning required.

**Think of it this way:** Where EBS is like a hard drive you plug into one EC2 instance, EFS is like a shared network drive that many instances can mount simultaneously.

|**EFS**|**EBS**|**S3**|
|---|---|---|
|Storage type|File (NFS)|Block|Object|
|Access|Multiple instances simultaneously|One instance at a time (mostly)|Via HTTP/API|
|Scaling|Automatic, elastic|Must provision size in advance|Unlimited, automatic|
|AZ redundancy|Multi-AZ (Standard)|Single AZ|≥3 AZs|
|Use case|Shared app data, CMS, home dirs|OS volumes, databases|Backups, media, data lakes|
|Protocol|NFS|iSCSI / virtualized|REST API / HTTPS|
**EFS use cases**
- **Web serving & CMS** – Multiple web servers sharing the same content (WordPress, Drupal, etc.)
- **Developer tools & home directories** – Shared development environments, CI/CD pipelines
- **Container storage** – Persistent shared storage for ECS and EKS workloads
- **Data analytics** – Shared scratch space for distributed processing jobs
- **Machine learning training** – All training nodes accessing the same dataset simultaneously

**EFS storage classes**
EFS offers two tiers, managed automatically with **EFS Intelligent-Tiering** (lifecycle policies):
- **EFS Standard** – For frequently accessed files. Data replicated across ≥3 AZs. Highest availability and durability.
- **EFS Standard-Infrequent Access (Standard-IA)** – Lower cost for files not accessed daily. Same multi-AZ redundancy. Per-access retrieval fee applies.
- **EFS One Zone** – Single-AZ storage. Lower cost than Standard. Good for dev/test or non-critical data.
- **EFS One Zone-IA** – Cheapest option. Single AZ + infrequent access pricing
**EFS Intelligent-Tiering** automatically moves files between Standard and Standard-IA (or One Zone and One Zone-IA) based on access patterns no manual intervention needed.

**EFS performance modes**
Choose the performance mode when you create the file system (cannot be changed after creation):
- **General Purpose** (default) – Low latency, ideal for web servers, CMS, home directories, development. Best for most use cases.
- **Max I/O** – Higher aggregate throughput and I/O operations per second, slightly higher latency. Use for big data workloads where many thousands of instances need concurrent access.
**EFS throughput modes**
- **Bursting Throughput** (default) – Throughput scales with file system size. Smaller file systems get burst credits (similar to EBS burst). Good for most workloads.
- **Elastic Throughput** – Automatically scales throughput up or down based on workload. Best for spiky, unpredictable workloads.
- **Provisioned Throughput** – You specify the throughput you need regardless of file system size. Use when your throughput needs exceed what bursting provides.

**How Amazon EFS works**
1. Create an EFS file system in your VPC.
2. Create **mount targets** one per AZ in your VPC (acts as a network endpoint).
3. Mount the file system on EC2 instances using the NFS client or the **EFS mount helper** (`amazon-efs-utils`).
4. All instances that mount the file system share the same view of the data in real time.