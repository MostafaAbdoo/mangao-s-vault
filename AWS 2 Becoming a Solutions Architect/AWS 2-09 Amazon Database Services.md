#### What is Amazon RDS?
**Amazon RDS** is a managed database service that sets up and operates a relational database in the cloud.
**Amazon RDS** frees you to focus on your applications so that you can give them the performance, high availability, security, and compatibility that they need. With Amazon RDS, your primary focus is your data and optimizing your application.
**Database types on Amazon RDS**
- MySQL
- MariaDB
- PostgreSQL
- Oracle Database
- Microsoft SQL Server
- Amazon Aurora
**Amazon RDS use cases**
- **Web & Mobile Apps:** Scalable, high-throughput database with no licensing constraints, ideal for variable workloads.
- **Ecommerce:** Secure, flexible, and cost-effective for online sales and retail businesses.
- **Gaming:** High-performance, managed database for mobile and online games, eliminating infrastructure concerns for developers.

**Characteristics of Amazon RDS DB instances:**
- **DB instance class** determines CPU, memory, and network performance
- **DB instance storage** three types:
    - **General Purpose SSD (gp3)** default for most use cases; cost-effective with baseline 3,000 IOPS
    - **Provisioned IOPS SSD (io1/io2)** high I/O workloads requiring consistent, low-latency performance
    - **Magnetic** legacy option, not recommended for new deployments
All Amazon RDS engine types (except for **Aurora**) use **Elastic Block Storage (EBS)**
**Amazon Aurora storage**
- Aurora uses a **shared cluster storage architecture** that is fully managed you don’t choose a storage type
- Storage **automatically scales** from 10 GiB up to **128 TiB** as your database grows
- Data is automatically replicated **6 ways across 3 AZs** for durability
The basic building block of **Amazon RDS** is the **DB instance**. A DB instance is an isolated database environment. It can contain multiple user-created databases, and you can access it by using the same tools and applications that you use with a standalone database instance. The resources in a database instance are determined from its database instance class, and the type of storage is determined from the type of disks.
#### High availability with Amazon RDS
**How It Works**
- The **Primary RDS instance** runs in **Availability Zone 1** and processes database requests from application instances in both AZs.
- A **Standby RDS instance** is deployed in **Availability Zone 2**, ensuring redundancy.
- **Synchronous replication** continuously updates the standby instance to keep it in sync with the primary database.
**Failover for the secondary instance will take place if:**
- Patching maintenance is being performed on the primary instance
- The primary DB instance has a host failure
- The Availability Zone of the primary database fails
- The primary instance is rebooted with failover selected
- The DB instance class is modified
**Deployment options for high availability**
1. **Multi-AZ DB Cluster** Creates a primary DB instance plus **two readable standby instances**, each in a different AZ. Provides HA, data redundancy, **and** read-scaling. Only available for MySQL and PostgreSQL.
2. **Multi-AZ DB Instance** The classic option: one primary, one standby in a different AZ. Standby **cannot** serve read traffic it’s for failover only.
**Anti-pattern:** Don’t use a Multi-AZ standby to offload read traffic it’s passive. For read scaling, add **Read Replicas** instead.

**Read replicas** can be **promoted to a standalone primary** DB instance. Because replication is asynchronous, promotion requires manual action and may involve a small amount of replication lag.
**Exam Tip:** **Multi-AZ = availability** (synchronous, automatic failover). **Read Replicas = scalability** (asynchronous, manual promotion). The exam loves to test this distinction. You can have up to **5 Read Replicas** per RDS instance.
**Exam Tip:** Know that gp3 replaced gp2 as the recommended default for RDS. Key difference: gp3 lets you scale IOPS and throughput independently of storage size, which is more cost-efficient for workloads that need extra I/O without extra storage.
#### Amazon RDS Proxy
**Exam Tip:** RDS Proxy is most commonly tested in the context of **Lambda functions connecting to RDS**. Lambda can create thousands of concurrent connections during scaling spikes RDS Proxy prevents connection exhaustion by pooling them. Whenever you see a scenario combining Lambda + RDS, RDS Proxy is the right answer.

**Amazon RDS Proxy** is a fully managed, highly available database proxy for Amazon RDS
RDS Proxy also automatically routes traffic to a new database instance while preserving application connections. This makes failovers more transparent for applications.
- **RDS Proxy** allows applications to pool and share connections established with the database. This makes applications more scalable.
- **RDS Proxy** reduces database failover times for Aurora and Amazon RDS databases. This makes applications more available and resilient.
- **RDS Proxy** enforces IAM authentication and stores credentials in AWS Secrets Manager. This makes applications more secure.
![[Pasted image 20261017133638.png]]
#### Backing up data in Amazon RDS
Amazon RDS provides two different options for backing up and restoring database instances:
1. Automated backups
2. Database snapshots

|**Features**|**Automated backups**|**Database snapshots**|
|---|---|---|
|Use case|Restore a database instance to a specific point in time.|Back up a database instance in a known state, and then restore it to that specific state.|
|Backup frequency|Daily during your backup window (transaction logs are captured every 5 minutes)|User-initiated (as frequently as the user chooses)|
|Retention period|The default is 7 days but it can be set to up to 35 days. The backups can be automatically deleted after any retention period.|Kept until the user explicitly deletes it|
|Sharing with other AWS accounts|Cannot be shared (needs to be copied  <br>to a manual snapshot first)|Can be shared (shared snapshots can be copied by other AWS accounts)|
**Storage & Copying:** Both backup types are stored in **Amazon S3** (managed by RDS, not visible in your S3 console). Snapshots can be copied within a Region, across Regions, or shared with other AWS accounts.
**Exam Tip:** Automated backup retention defaults to **7 days** (max 35 days). Automated backups are deleted when the RDS instance is deleted (unless you enable final snapshot). Manual snapshots persist until explicitly deleted.

**Amazon RDS cross-region backups**
For enhanced disaster recovery, RDS can **replicate snapshots and transaction logs** to a different AWS Region.
1. **Backup Storage:** Snapshots and logs from the primary RDS instance are stored in an Amazon S3 bucket managed by RDS.
2. **Cross-Region Replication:** When enabled, RDS automatically copies backups (automated or manual) to a selected Region or AWS account.
Additionally, you can create **cross-Region read replicas** to:
- Strengthen disaster recovery.
- Reduce latency by serving read queries closer to users.
- Simplify migration between AWS Regions.
**Important constraint:** You **cannot enable encryption on an existing unencrypted RDS instance directly**. The workaround:
1. Take a snapshot of the unencrypted instance.
2. Copy the snapshot and enable encryption during the copy.
3. Restore the encrypted snapshot to a new DB instance.
#### Amazon Aurora
**Amazon Aurora** is AWS's own cloud-native relational database engine, fully compatible with **MySQL** (up to 5x faster) and **PostgreSQL** (up to 3x faster) than standard community editions running on RDS.

Aurora is part of Amazon RDS under the hood you manage it the same way but it uses a fundamentally different storage architecture designed specifically for the cloud.

**Exam Tip:** Aurora is not a separate product it's an RDS engine option. But it behaves very differently from MySQL/PostgreSQL on RDS because of its shared storage architecture. Know those differences cold.
**Aurora's shared cluster storage**
Aurora decouples **compute** from **storage**. Instead of EBS volumes attached to individual instances, Aurora uses a **shared cluster volume** that:
- Automatically scales from **10 GiB to 128 TiB**
- Replicates data **6 ways across 3 Availability Zones** (2 copies per AZ)
- Tolerates losing **up to 2 copies** of data without write availability loss
- Tolerates losing **up to 3 copies** without read availability loss
Aurora manages data updates and availability using a **quorum-based voting system**:
- **Write Quorum:** Needs **4 out of 6** copies to acknowledge a write before it's considered successful.
- **Read Quorum:** Needs **3 out of 6** copies to respond for a read to succeed.

**High availability and fault tolerance**
Aurora is built for HA by default:
- **Cluster endpoint** always points to the current primary (writer)
- **Reader endpoint** load-balances reads across all Aurora Replicas
- **Automated failover** if the primary fails, Aurora promotes a replica in **~30 seconds** (much faster than standard RDS Multi-AZ failover of 60–120s)
**Aurora Replicas (Read Replicas)**
- You can add up to **15 Aurora Replicas** in the same Region, all sharing the same underlying storage
- Replicas have **minimal replication lag** (typically under 100ms) because they read from the shared storage there's no data replication happening between instances
- Replicas can be promoted to primary automatically during failover (you can set **priority tiers** to control promotion order)
**Exam Tip:** Aurora Replicas serve double duty they're both a read-scaling mechanism **and** an HA failover target. If you have 1+ Aurora Replicas, failover happens in ~30 seconds. With no replicas, Aurora must provision a new instance (takes minutes).

**Aurora Serverless v2**
**Aurora Serverless v2** automatically scales compute capacity based on actual workload instantly and in fine-grained increments (0.5 ACU steps).
**When to use it:**
- Unpredictable or intermittent workloads (dev/test environments, infrequent batch jobs)
- Applications with sudden traffic spikes (e.g., event-driven apps, SaaS multi-tenant)
- You want to pay only for what you use

**Aurora Global Database**
An **Aurora Global Database** spans multiple AWS Regions:
- **One primary Region** handles reads and writes
- Up to **5 secondary Regions** replicate data with typical lag under **1 second**
- Secondary Regions can serve **low-latency reads** for global users
- In a disaster, a secondary Region can be **promoted to primary** in under 1 minute (RTO < 1 min, RPO < 1 sec)
**Exam Tip:** Aurora Global Database is the right answer for: (1) cross-region disaster recovery with RPO < 1 second, and (2) reducing read latency for a globally distributed user base.

|Scenario|Choose|
|---|---|
|Need MySQL/PostgreSQL with higher throughput and faster failover|**Aurora**|
|Need Oracle or SQL Server|**RDS** (Aurora doesn't support these)|
|Unpredictable workload, want auto-scaling compute|**Aurora Serverless v2**|
|Global app needing sub-second cross-region replication|**Aurora Global Database**|
|Cost-sensitive dev/test with standard MySQL|**RDS MySQL** (cheaper)|
|Up to 15 read replicas in one Region|**Aurora**|
|Up to 5 read replicas in one Region|**RDS**|
**Exam Tip:** Aurora costs more than standard RDS but offers superior performance, faster failover, and more read replicas. The exam won't ask you to memorize pricing, but it will present trade-off scenarios where you need to justify choosing Aurora.
Aurora failover is ~30s; RDS Multi-AZ failover is ~60–120s.
#### What is AWS DMS?
AWS Database Migration Service (**AWS DMS**) is a **fully managed service** for migrating and replicating databases and analytics workloads to and within AWS.
- Supports **homogeneous (same engine)** and **heterogeneous (different engine)** migrations. One endpoint must be on AWS.
- Works with relational, NoSQL, analytics, and data warehouse sources/targets.
- **Minimal downtime** source databases stay operational during migration.
- Enables **continuous, low-latency replication** for tasks like building data lakes in Amazon S3 or consolidating data in Amazon Redshift.
- **On-premises to on-premises migration is not supported.**
**Supported Databases:**
- **Sources:** Oracle, SQL Server, MySQL, PostgreSQL, MariaDB, IBM Db2, SAP, MongoDB, Aurora.
- **Targets:** Oracle, SQL Server, PostgreSQL, MySQL, Amazon Redshift, SAP ASE, Amazon S3, Amazon DynamoDB, Amazon OpenSearch Service.
![[Pasted image 20261017150808.png|544]]
DMS supports three migration modes: **Full Load** (one-time bulk copy), **CDC** (continuous change capture), or **Full Load + CDC** (bulk copy then stay in sync). Once in sync, you cut over your app with minimal downtime.

**Homogeneous database migrations**
migrating on-premises or cloud databases to **the same engine** on Amazon RDS or Aurora
![[Pasted image 20261017151933.png|551]]

**Heterogeneous database migrations**
Heterogeneous migrations involve **moving data between different database engines** (e.g., Oracle to PostgreSQL). AWS provides multiple tools to **simplify and automate schema conversion at scale**
**Migration Tools:**
- **AWS Schema Conversion Tool (AWS SCT)** converts the source schema and code objects to match the target engine.
- **AWS DMS Schema Conversion** a fully managed, console-based alternative to SCT.
![[Pasted image 20261017152208.png|572]]
Set up an **AWS DMS replication instance**, which runs one or more migration tasks
- Heterogeneous migrations are a two-step process: (1) convert the schema with AWS SCT, then (2) migrate the data with AWS DMS.
- For replicating to data lakes, DMS can stream changes from a source DB directly into Amazon S3.
- **Exam Tip:** For **homogeneous migrations** (same engine), DMS handles it directly using native tools. For **heterogeneous migrations** (different engines, e.g., Oracle → PostgreSQL), you need AWS SCT first to convert the schema. The exam commonly tests this distinction. Also remember: **DMS requires at least one endpoint to be in AWS** on-premises to on-premises is not supported.
#### Amazon DynamoDB
- Automated *key/value* database provisioning and dynamic scaling in response to actual usage.
- **Fully managed:** AWS handles all underlying compute and storage no servers to manage.
- **Scalability:** Automatically scales throughput and storage as your data grows.
- **Redundancy:** Data is automatically replicated across **multiple Availability Zones within a single AWS Region** for durability. Multi-region replication requires **Global Tables** (opt-in).
- **Recoverability:** Supports on-demand backups and **Point-in-Time Recovery (PITR)** up to 35 days.
- **Low latency:** Single-digit millisecond read/write latency at any scale.
- **Security:** Integrates with IAM for fine-grained access control; supports encryption at rest with AWS KMS.
- **Flexibility:** Schema-free items in the same table can have different attributes. JSON is a commonly used data format.
- **TTL (Time to Live):** Automatically expires and deletes items after a set timestamp, at no extra cost ideal for session data, logs, and temporary records.
![[Pasted image 20261018223413.png]]
- A *sort key* denotes how the items that share the same partition key are sorted within the partition. Sort keys can be timestamps, version numbers, or audit log IDs. The sort keys are used to create groupings of data.
- A **composite primary key** is the combination of the partition key and sort key which will uniquely identify an item. Having a primary key enables rich query capabilities.

**DynamoDB Streams**

**DynamoDB Streams** captures a time-ordered sequence of item-level changes in a DynamoDB table. Each change (insert, update, delete) is written to the stream and retained for **24 hours**.
Once a change sits in the stream for 24 hours, it automatically expires and is permanently deleted by AWS
Common use: trigger an **AWS Lambda** function whenever an item changes — for example, to send notifications, replicate data, or update a secondary system.
**Exam Tip:** DynamoDB Streams + Lambda is a classic event-driven architecture pattern in the exam. Think of it as DynamoDB’s built-in change data capture (CDC) mechanism.

**Global Tables**
- **Global tables** enable **multi-region, multi-active replication** for **fast, local read/write performance** across global applications.
- Unlike default DynamoDB replication (within a single Region), global tables **replicate data across multiple AWS Regions** automatically.
---
- **Provisioned Mode** you set RCU/WCU targets; supports Auto Scaling. Best for predictable, steady traffic.
- **On-Demand Mode** automatically handles any traffic level, charged per request. Best for unpredictable or spiky workloads.

 **DynamoDB Accelerator (DAX)**
 **DAX** is an in-memory cache that sits in front of DynamoDB, delivering up to **10x faster** read performance dropping latency from milliseconds to **microseconds**
 DAX also reduces the number of read requests hitting your DynamoDB table using *cache nodes*, which lowers provisioned RCU requirements and cuts costs.
 **DAX Cluster Deployment**
- **Subnet Group**: You define a group of subnets for DAX deployment.
- **Node Deployment**: One node is placed in each subnet within the group.
- **Primary & Read Replicas**: One node functions as the primary, while others serve as read replicas, ensuring **high availability and performance**.

#### AWS fully managed purpose-built database options
- **Amazon DocumentDB** (MongoDB-compatible) document store for JSON-like data
- **Amazon Keyspaces** (Apache Cassandra-compatible) wide-column store for high-speed writes
- **Amazon MemoryDB** durable, Redis-compatible(key-value pairs) in-memory(RAM) database (now also supports **Valkey**, the open-source Redis fork)
- **Amazon Neptune** managed graph database for highly connected data
- **Amazon Timestream** serverless time-series database for IoT and operational metrics
#### Matching a database to your business need
Always start with the **workload requirements** and **data model**, then evaluate which database fits. 

|Data Model|AWS Service|Best For|
|---|---|---|
|Relational (structured, SQL)|RDS / Aurora|OLTP apps, ERP, CMS|
|Key-value / document|DynamoDB|High-scale serverless apps|
|Document (JSON)|DocumentDB|MongoDB workloads, content management|
|Wide-column|Keyspaces|High-speed time-series, IoT writes|
|In-memory (durable)|MemoryDB|Leaderboards, session data with durability|
|Graph|Neptune|Social networks, fraud detection, recommendations|
|Time-series|Timestream|IoT metrics, operational monitoring|
|In-memory cache|ElastiCache|Reducing DB read load, session caching|
**Exam Tip:** The SAA-C03 exam frequently presents a workload description and asks you to pick the right database. The key signals are: (1) data model type, (2) whether consistency or latency is the priority, and (3) whether the data needs to persist (MemoryDB) or just accelerate access (ElastiCache). Use MemoryDB when you need Redis-speed performance but can't afford data loss.
#### Amazon ElastiCache
**Amazon ElastiCache** is a service that makes it simple to deploy and operate an in-memory cache. It provides a high-performance, scalable, and cost-effective caching solution.

 **Amazon ElastiCache has two deployment options**:
 **Serverless caching**
Amazon ElastiCache Serverless simplifies cache creation and instantly scales to support customers' most demanding applications. You can create a highly-available and scalable cache in less than a minute, eliminating the need to provision, plan for, and manage cache cluster capacity. ElastiCache Serverless automatically stores data redundantly across three Availability Zones and provides a 99.99% availability SLA.
**Self-designed clusters**
If you need fine-grained control over your **Valkey**, **Redis OSS**, or **Memcached** cluster, you can choose to design your own cluster with ElastiCache. ElastiCache enables you to operate a node-based cluster, by choosing the node-type, number of nodes, and node placement across AWS Availability Zones for your cluster.

ElastiCache supports three caching engines:
- **Valkey** recommended for new deployments; open-source Redis fork under the Linux Foundation. Fully compatible with Redis APIs.
	- **Best for:** All **new** applications and workloads requiring advanced data structures (hashes, sets, sorted sets, streams) and high availability, while staying on a purely open-source track. 
- **Redis OSS** original Redis open-source engine, supported for existing workloads.
	- **Best for:** Legacy or **existing** applications already running on Redis OSS that have not yet migrated to Valkey.
- **Memcached** simple, multithreaded key-value store designed purely for caching.

ElastiCache is fundamentally built around a **key-value data model**, While you always look things up using a key (e.g., `user:1001`), engines like **Valkey** and **Redis OSS** let you store rich, structured data types _as_ that value, rather than just plain text so it's not that simple

##### **Comparison between Redis OSS/Valkey and Memcached**
**Redis OSS / Valkey**
**Features:**
- Rich data structures: strings, hashes, lists, sets, sorted sets, streams, and more
- **Persistence:** optional durability data can survive node failure
- **Replication and automatic failover:** built-in, with Multi-AZ support
**Best Use Cases:**
- Sessions, leaderboards, queues, real-time analytics
- Workloads that need data structures beyond simple key-value pairs
- Applications that require cache durability (data must not be lost on failure)
**Memcached**
**Features:**
- **Simple key-value store:** Primarily designed for storing and retrieving simple key-value pairs.
- **High performance:** Known for its exceptional performance, especially in scenarios that require frequent key-value lookups.
- **No persistence:** Memcached is designed for in-memory caching and does not offer persistence options.
**Best Use Cases:**
- **Simple caching:** When your application primarily needs to cache frequently accessed data in a simple key-value format.
- **High performance:** If your application requires extremely fast read and write operations.
- **No persistence requirements:** When data durability is not a concern, as Memcached data is volatile and lost upon instance termination or failure