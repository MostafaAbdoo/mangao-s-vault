*Amazon Elastic Compute Cloud (EC2)*
**Amazon EC2** Instances = a virtual server in the AWS Cloud. When you launch an EC2 instance, the instance type that you specify determines the hardware available to your instance.

- Allows users to rent virtual servers (known as instances) on which they can run their own applications.
- Each instance type offers a different balance of compute, memory, network, and storage resources.
- Infrastructure as a Service
Offers a wide range of instance types optimized for various use cases, such as general-purpose computing, memory-intensive workloads, and GPU processing.
- Renting virtual machines (EC2)
- Storing data on virtual drives (EBS)
- Distributing load across machines (ELB)
- Scaling the services using an auto-scaling group (ASG)
#### **User Data Script**
EC2 instances can be configured at launch(only once) using User Data scripts.
**Bootstrapping = refers to executing commands upon a machine's startup**
including:
- Installing updates
- Installing software
- Downloading files from the internet
- Any other tasks you might require
This user data script creates a server and static web page when the EC2 instance is first created.

#### **EC2 Instances and Instance Types**
 **Operating System (OS) - Compute Power & Cores (CPU) - Random-Access Memory (RAM) - Storage Space (Network-attached or Hardware) - Network Card - Firewall Rules - Bootstrap Script**
 AWS offers a variety of EC2 instance types optimized for diverse use cases. AWS follows a specific naming convention for its services and resources.
    - m5.2xlarge
        - **m:** instance class
        - **5:** generation (AWS improves them over time)
        - **2xlarge:** size within the instance class
The different EC2 types are:
1. General Purpose
2. Compute Optimized
3. Memory Optimized
4. Accelerated Computing
5. Storage Optimized
6. HPC Optimized
7. Instance Features
8. Measuring Instance Performance
Tools like CloudWatch, Cost Explorer, Trusted Advisor, and third-party tools can assist in right sizing.
### **EC2 Purchasing Options**
#### **On-Demand Instances** : 
**Pay-as-you-go model with no long-term commitments.**
Offers **flexibility to start and stop instances as needed without upfront payments or long-term contracts.**
**Use cases:**
- **Variable Workloads:** Ideal for unpredictable usage patterns or fluctuating workloads.
- **Development and Testing:** Well-suited for temporary environments without long-term commitments.
- **Short-Term Projects:** Quickly provision instances for short-term initiatives or projects.
#### **Reserved Instances**
- Reserved capacity for **a specific instance type in a selected region.**
    - **72% discount compared to on-demand**
    - You reserve a specific instance attributes (Instance Type, Region, Tenancy, OS)
- Options for **1-year or 3-year terms**.
**Use cases:**
  - Ideal for applications with steady-state usage patterns.
  - **Capacity Planning:** Enables organizations to secure reserved capacity in advance for anticipated workloads, ensuring resource availability when needed.
#### **Savings Plans**

- Flexible pricing model offering savings on compute usage across EC2 instances, AWS Lambda, and Fargate.
    - Provides significant discounts compared to On-Demand rates, with **the flexibility to choose between different commitment options.**
- Ideal for **workloads with consistent usage or those requiring flexibility in instance types and sizes.**
#### **Spot Instances

- Bid on unused EC2 capacity, allowing access to spare compute capacity at reduced rates.
- Offers the potential for **significant cost savings,** sometimes up to 90% compared to On-Demand prices.
- Suitable for **fault-tolerant applications, batch processing, or workloads with flexible start and end times.**
- **Use cases:**
    - Useful for workloads that are resilient to failure
    - Batch jobs
    - Data analysis
    - Image processing
#### **Dedicated Hosts**

- Physical servers dedicated to a single customer's use, providing **isolation and control over instance placement**.
- Offers compliance and regulatory requirements by ensuring instances run on dedicated infrastructure.
- Ideal for applications with licensing restrictions or regulatory constraints.
#### **Dedicated Instances**

- Dedicated Instances **provide physical servers exclusively dedicated to a single customer's use.**
    - Enhanced security and isolation for sensitive workloads.
    - Predictable performance
    - Ideal for applications with **strict compliance or licensing requirements**
- May share hardware with other instances in same account
- No control over instance placement
#### **Capacity Reservations**

- Capacity reservations **allow users to reserve capacity for specific instance types in a selected AWS Availability Zone.**
- Capacity Reservations focus on **guaranteeing** that you can actually launch an instance when you need it, regardless of hardware tenancy.
- Provides guaranteed capacity availability for instances within the reserved capacity pool.
    - Offers flexibility to launch instances within the reservation as needed, ensuring resource availability for critical workloads.
- Capacity reservations can be purchased for On-Demand instances, Reserved Instances, or Savings Plans, providing various pricing options and cost-saving opportunities.

## **How to choose EC2 Purchasing Options**

1. **Define Workload Requirements**
    - Identify the **specific requirements of your workload**, including compute, memory, storage, and networking needs.
2. **Analyze Performance Characteristics**
    - Evaluate the **performance characteristics of different EC2 instance types, such as CPU performance, memory size, and network performance.**
3. **Consider Cost Optimization**
    - Assess the **cost implications of different instance types**, taking into account On-Demand pricing, Reserved Instances, Savings Plans, and spot instance options.
4. **Review Use Case Suitability**
    - Explore the **various EC2 instance families available, such as General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, etc.**
5. **Evaluate Instance Families**
    - Conduct testing and benchmarking to validate the performance and suitability of selected EC2 instance types for your workload before deployment.
6. **Monitor and Optimize**
    - Continuously monitor EC2 instance performance and usage metrics to identify optimization opportunities and make adjustments as needed to meet evolving workload requirements.

### **Security in EC2**
- **Security Groups are essential for network security in AWS.**
    - They manage **inbound and outbound traffic** for EC2 Instances.
- Security Groups consist solely of **rules.**
- Rules within Security Groups **can specify permissions by IP address or by referencing other Security Groups.**
- Can be attached to **multiple instances.**
- Restricted to a specific region and VPC combination.
- Advisable to maintain **a separate security group** specifically for SSH access.
    - If your application times out and is not accessible, it's likely a security group configuration issue.
- If you receive a "connection refused" error, the issue may be with the application itself or it may not have been started.
    - By default, **all inbound traffic is blocked.**
    - By default, **all outbound traffic is allowed.**
**They regulate:**
- Access to specific ports
- Authorized IP ranges for both IPv4 and IPv6
- Control of inbound traffic (from others to the instance)
- Control of outbound traffic (from the instance to others)
## **Elastic Block Store (EBS) Volumes**
provides **persistent block-level storage** for virtual machines (EC2 instances). It allows instances to persist data even after termination
Think of EBS like a **virtual hard disk** that you attach to a server in the cloud.
- You launch an EC2 instance
- Then you attach an EBS volume
- The instance reads/writes data to it like a normal disk
**1. Persistent storage**
- Data **does NOT disappear** when the EC2 instance stops or crashes
- Unlike instance store (temporary storage)
**2. Block storage
- Works like a raw disk (not files like S3)
- You can:
    - Format it (NTFS, ext4, etc.)
    - Install databases
    - Run applications needing fast I/O
**3. Attached to one instance (mostly)
- An EBS volume is usually attached to **one EC2 instance at a time**
- But you can detach and reattach it to another instance
**4. Located in a single Availability Zone
- Each volume lives inside one AZ
- a volume in us-east-1a cannot be attached to an instance in us-east-1b.
- For backup and to move a volume across AZs → you use **snapshots**
EBS (Elastic Block Store) is a **network drive, meaning it's not a physical drive and uses the network to communicate with the instance, which may result in some latency.**

##### **Delete on Termination Attribute**
- Controls the EBS behaviour when **an EC2 instance terminates**
    - By default, the root EBS volume is deleted (attribute enabled)
    - By default, any other attached EBS volume is not deleted (attribute disabled)

##### **EBS Snapshots Features**
- **EBS Snapshot Archive**
    - Move a Snapshot to an ”archive tier” that is 75% cheaper Takes within 24 to 72 hours for restoring the archive
- **Recycle Bin for EBS Snapshots**
    - Setup rules to retain deleted snapshots so you can recover them after an accidental deletion Specify retention (from 1 day to 1 year)
## **EC2 Instance Store**

- **EBS volumes are network drives that offer good but "limited" performance.**
- For high-performance hardware disks, use EC2 Instance Store, which provides better I/O performance.
- EC2 Instance Store volumes are ephemeral, meaning they lose their storage if the instance is stopped.
- They are suitable for buffer, cache, scratch data, or temporary content.
- There is a risk of data loss if the hardware fails.
- Managing backups and replication is your responsibility
## **Amazon Machine Image (AMI)**

- **Amazon Machine Image (AMI)** = customization of an EC2 instance
    - Can add your own software, configuration, operating system, monitoring…
- Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a specific region (and can be copied across regions)
- You can launch EC2 instances from:
    - **A Public AMI:** AWS provided
    - **Your own AMI:** you make and maintain them yourself
    - **An AWS Marketplace AMI:** an AMI someone else made (and potentially sells)
    - **Community AMI:** These are AMIs that comes from AWS users, and are not verified by AWS
- Start an EC2 instance and customize it according to your needs.
    - Stop the instance to ensure data integrity before creating an image.
    - Build an AMI (Amazon Machine Image) from the stopped instance, which will also create EBS snapshots.
    - Launch new instances using the custom AMI or other available AMIs
## **EC2 Image Builder**
is a service from Amazon Web Services that lets you: **Automatically build, update, test, and distribute AMIs**
- Can be scheduled to run at specific intervals, such as weekly or whenever packages are updated.
- **It's a free service, with charges only for the underlying resources used**

## **EFS - Elastic File System**
provides a **shared file system** for multiple servers in Amazon EC2.
- Shared access
- Elastic (auto-scaling)
- Multi-AZ availability
- Works with Linux EC2 instances
- Managed service
- Highly available, scalable, expensive (3x EBS gp2), pay per use, no capacity planning
##### **How it works**
1. Create an EFS file system
2. Mount it on EC2 instances (like mounting a drive)
3. All instances see the same files
#### **EFS Infrequent Access (EFS-IA)**
- Is a storage class optimized for cost, designed for files not accessed daily.
- Offers up to **92% lower cost compared to EFS Standard.**
- EFS automatically moves files to EFS-IA based on the last access time.
- Enable EFS-IA with a Lifecycle Policy, such as moving files not accessed for 60 days to EFS-IA.
- The transition to EFS-IA is transparent (Your app doesn’t know anything changed) to applications accessing EFS.

## **Amazon FSx: Managed File Storage**
Gives you **fully managed file systems—but specialized ones**, not the general-purpose type like EFS.
EFS is generic (Linux + NFS).  But some workloads need: Windows file system features - Extreme performance (HPC)
- **FSx for Windows File Server:**
    - Managed Windows-native file system.
    - Supports SMB protocol and Microsoft Active Directory.
- **FSx for Lustre:**
    - High-performance file system for compute-intensive workloads.
    - Integrates with Amazon S3 for data storage.
**Key Features:**
    - High performance, scalability, built-in backups.
    - Integration with AWS services.

---
**Elastic Load Balancing (ELB)** distributes incoming traffic across one or more Amazon EC2 instances.