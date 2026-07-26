🟢 Relational Databases
1. **Structure**: Data stored in tables with rows and columns.
2. **Schema**: Fixed schema with predefined structure.
3. **Relationships**: Supports complex relationships using foreign keys.
4. **Query Language**: Uses SQL.
5. **Examples in AWS**: Amazon RDS (supports MySQL, PostgreSQL, Oracle, SQL Server).

🟠 Non-Relational Databases
1. **Structure**: Data stored in various formats like key-value pairs, documents, or graphs.
2. **Schema**: Dynamic schema allowing flexibility.
3. **Relationships**: Generally does not enforce relationships; handles large volumes of unstructured data.
4. **Query Language**: Various, depending on type (e.g., MongoDB, Cassandra).
5. **Examples in AWS**: Amazon DynamoDB (key-value store), Amazon DocumentDB (document store), Amazon Neptune (graph database).

## **Amazon RDS**
- **Managed** database service employing SQL for queries
- Enables creation of cloud databases managed by AWS
- **Supports various SQL-based databases:**
    - PostgreSQL
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - IBM DB2
    - Aurora (AWS proprietary database)

**Read Replicas:** 
You have:
- **1 Primary DB** → handles all **write operations** (`INSERT`, `UPDATE`, `DELETE`)
- Up to **15 replicas** → copies of the database used for **read queries**
If many users are reading data, the primary DB may become slow.
- Up to 15 Read Replicas supported
- Distribute read queries across replicas
- Primary database handles all writes
- Asynchronous replication from primary
- Enhances performance and responsiveness

**Multi-AZ:** AWS creates: **Primary DB** - **Standby DB** in another Availability Zone
- Failover for High Availability - activates during an Availability Zone outage, ensures continuous database service
- Main database handles all read/write operations
- Limited to one alternative Availability Zone for failover

**Multi-Region (Read Replicas):** You create read replicas in other AWS Regions.

- Disaster recovery in case of region issue
- Local performance for global reads
- Replication cost

### _**Advantages of RDS compared to deploying a database on EC2:**_
- RDS is a **managed service**, offering:
    - Automated provisioning and OS patching
    - Continuous backups with point-in-time restore
    - Monitoring dashboards
    - Read replicas for enhanced read performance
    - Multi-AZ setup for disaster recovery
    - Scheduled maintenance windows for upgrades
    - Scaling capabilities (vertical and horizontal)
    - Storage backed by EBS

## **Amazon Aurora**
- **Amazon Aurora** = Amazon Aurora is a global-scale relational database service built for the cloud with full MySQL and PostgreSQL compatibility.
    - provides **built-in security, continuous backups, serverless compute, up to 15 read replicas, automated multi-Region replication, and integrations with other AWS services.**
    - It supports both PostgreSQL and MySQL databases.
- Aurora is "AWS cloud-optimized" and boasts significant performance enhancements,
    - claiming a **5x improvement over MySQL on RDS** and **over 3x the performance of Postgres on RDS.**
- Aurora's storage automatically scales in increments of 10GB, up to 128 TB.
- While Aurora is more efficient, it also costs 20% more than RDS.
- Aurora is not included in the free tier.
### **Amazon Aurora Serverless**
Amazon Aurora Serverless is a version of Aurora where AWS automatically manages the database capacity for you.
- Automated database provisioning and dynamic scaling in response to actual usage. Both PostgreSQL and MySQL are compatible with Aurora Serverless DB.
- **Eliminates the need for capacity planning** and **reduces management overhead.**
- Pay-per-second billing offers potential cost savings.
- Suitable for irregular, sporadic, or unpredictable workloads.
## **NoSQL Databases**
- NoSQL databases, short for non-SQL or non-relational databases, are crafted for **distinct data models and boast adaptable schemas tailored for contemporary applications**.
- Advantages:
    - Flexibility: Simplified evolution of data models.
    - Scalability: Designed for horizontal scaling through distributed clusters.
    - High Performance: Optimization for specific data models.
    - Functionality: Tailored types optimized for respective data models.
- Examples: Key-value, document, graph, in-memory, and search databases.
- **JSON (JavaScript Object Notation) Overview:**
    - Widely used data format modeled after JavaScript object syntax.
    - Fits naturally into the NoSQL database model due to its schema-less nature.
- **Characteristics of JSON:**
    - Data can include nested structures, allowing for complex data hierarchies.
    - Fields in JSON data can be added, removed, or modified over time without constraints.
    - Supports a variety of data types, including arrays, strings, numbers, and boolean values
## **Amazon DynamoDB**
- Automated key/value database provisioning and dynamic scaling in response to actual usage.
- Eliminates the need for capacity planning and reduces management overhead.
- Pay-per-request or provisioned-capacity billing offers potential cost savings.
- Is a key/value database:
    - A key-value database is a type of non-relational database (NoSQL) that stores data as key-value pairs.
    - A key serves as a unique identifier for retrieving associated data.
    - Values can range from simple data types to complex objects.
- Suitable for irregular, sporadic, or unpredictable workloads.
- Designed for extremely high scalability, low latency, and high availability.
- Commonly used in:
    - serverless applications
    - gaming leaderboards
    - session management
    - IoT applications
    - real-time applications
### **Amazon DynamoDB - Global Tables**
- Make a DynamoDB table accessible with low latency in multiple-regions
- Active-Active replication (read/write to any AWS Region)
## **Amazon DocumentDB**
- MongoDB is used to store, query, and index JSON data.
- Has similar "deployment concepts" as Amazon Aurora.
- Amazon DocumentDB (with MongoDB compatibility):
- Fully managed and highly available, with replication across 3 Availability Zones (AZs).
- DocumentDB storage automatically grows in increments of 10GB.
- Automatically scales to handle workloads with millions of requests per second.
## **Amazon ElastiCache**
It is mainly used to make applications much faster by storing frequently accessed data in memory instead of repeatedly querying slower databases.
- Similar to how RDS provides managed relational databases…
    - ElastiCache offers managed Redis or Memcached services
- Caches are high-performance, low-latency in-memory databases
- They help alleviate the load on databases for workloads with heavy read operations
- AWS handles operating system maintenance and patching, optimizations, setup, configuration, monitoring, failure recovery, and backups
## **DynamoDB Accelerator - DAX**
- Fully managed in-memory cache for DynamoDB.
- Offers up to a 10x performance improvement, reducing latency from single-digit milliseconds to microseconds.
- Provides a secure, highly scalable, and highly available solution.
- Difference with ElastiCache at the CCP level:
    - DAX is exclusively used with and integrated into DynamoDB. ElastiCache can be used with other databases.

## **Amazon Redshift**
- Redshift is based on PostgreSQL **but is not used for Online Transaction Processing (OLTP).**
    - Provides a SQL interface for performing queries.
- It is designed for **Online Analytical Processing (OLAP) - suitable for analytics and data warehousing.**
    - Typically, data is loaded once every hour, not every second.
- Offers **10x better performance than other data warehouses and can scale to petabytes of data.**
- Utilizes columnar storage of data, as opposed to row-based storage.
- Features Massively Parallel Query Execution (MPP) and is highly available.
- Pricing is based on a pay-as-you-go model, depending on the instances provisioned.
- Compatible with Business Intelligence (BI) tools such as AWS Quicksight or Tableau for data visualization and analysis.
## **Amazon Redshift Serverless
- Automatically **provisions and scales the underlying capacity of the data warehouse.**
- Allows running **analytics workloads without managing data warehouse infrastructure.**
- Offers a pay-only-for-what-you-use pricing model, helping save costs.
- Use cases include reporting, dashboarding applications, and real-time analytics.
## **Amazon EMR (Elastic MapReduce)**
- A cloud **big data platform for processing massive amounts of data.**
- Supports open-source tools such as Apache Hadoop, Apache Spark, HBase, Flink, and Presto.
- Simplifies running big data frameworks for processing and analyzing large datasets.
- Designed to be cost-effective, scalable, and secure.
- Commonly used for data transformation, data processing, and data analytics tasks.
- Allows **quick setup and configuration of clusters of virtual servers for data processing.**
- Suitable for handling vast amounts of data efficiently.
## **Amazon Athena**
- **Serverless query service to analyze data stored in Amazon S3.**
    - Uses standard SQL language to query files.
- Supports formats such as CSV, JSON, ORC, Avro, and Parquet (built on Presto).
    - Pricing: $5.00 per TB of data scanned.
- Optimize cost by using compressed or columnar data formats (reduces data scanned).
- Use cases include **business intelligence, analytics, reporting, and analyzing logs such as VPC Flow Logs, ELB Logs, and CloudTrail trails.**
## **Amazon QuickSight**
- Serverless **machine learning-powered business intelligence service for creating interactive dashboards.**
    - Fast, automatically scalable, and embeddable, with per-session pricing.
- **Use cases include:**
    - Business analytics.
    - Building visualizations.
    - Performing ad-hoc analysis.
    - Gaining business insights using data.
- Integrated with various data sources such as RDS, Aurora, Athena, Redshift, and S3.

## **Amazon Neptune**
- Fully managed **graph database service.**
- Popular for datasets like **social networks, where users have friends, posts have comments, comments have likes from users, and users share and like posts.**
- Highly available across three Availability Zones (AZs), with up to 15 read replicas.
- Designed to build and run applications working with highly connected datasets, optimized for complex and challenging queries.
    - Capable of storing up to billions of relationships and querying the graph with millisecond latency.
    - Highly available with replications across multiple AZs.
- Great for use cases such as **knowledge graphs (like Wikipedia), fraud detection, recommendation engines, and social networking**
## **Amazon TimeStream**
- It is designed specifically for data that changes over time and is collected continuously.
- Fully managed, fast, scalable, serverless **time series database.**
- Automatically scales up and down to adjust capacity as needed. Capable of storing and analyzing trillions of events per day.
- Offers performance that is 1000s of times faster and costs 1/10th that of traditional relational databases.
- Provides **built-in time series analytics functions to help identify patterns in data in near real-time.**
## **Amazon Quantum Ledger Database (QLDB)**
- **Amazon Quantum Ledger Database (Amazon QLDB) designed to record financial transactions in a transparent, immutable, and cryptographically verifiable manner.** One trusted authority keeps a tamper-proof history of all data changes.
- Fully managed, serverless, highly available, with replication across three Availability Zones (AZs).
- Used to **review the history of all changes made to your application data over time.**
- Immutable system: no entry can be removed or modified, ensuring data integrity.
- Offers 2-3x better performance than common ledger blockchain frameworks and allows data manipulation using SQL.
- Difference with Amazon Managed Blockchain: QLDB does not have a decentralization component and is designed to comply with financial regulation rules.
## **Amazon Managed BlockChain**
- **Blockchain Technology:**
    - Enables the **development of applications where multiple parties can execute transactions without requiring a trusted, central authority.** Many organizations trust a shared system without a central owner.
- **Amazon Managed Blockchain:**
    - A managed service that allows you to:
        - Join public blockchain networks.
        - Create your own scalable private network.
        - Compatible with blockchain frameworks Hyperledger Fabric and Ethereum.
![[Pasted image 20260508084809.png]]
## **AWS Glue**
- Managed database to **extract, transform, and load (ETL) service.**
- Useful for preparing and transforming data for analytics.
- Fully serverless service.

# **Databases: Migration Services**
## **DMS – Database Migration Service**
- Quickly and securely **migrates databases to AWS, ensuring resilience and self-healing capabilities.**
- The source database remains **available during the migration process.**
- Supports both homogeneous migrations (e.g., Oracle to Oracle) and heterogeneous migrations (e.g., Microsoft SQL Server to Amazon Aurora).

## **AWS Schema Conversion Tool**
- It helps convert your database structure (**schema**) and some database code automatically.
- Both solutions automatically convert database schemas and code objects, marking unconvertible objects with manual conversion instructions.
- AWS SCT scans application source codes for SQL statements and optimizes them for AWS services during database migration.
- Post-conversion, AWS SCT facilitates data migration from various warehouses to Amazon Redshift using data migration agents.
- These tools simplify migrations, support popular databases, and save significant manual effort and resources.
- AWS offers **two schema conversion solutions for database migrations: the managed AWS DMS Schema Conversion (DMS SC) and the downloadable AWS Schema Conversion Tool (AWS SCT).**