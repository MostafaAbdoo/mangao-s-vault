#### Key Benefits of Amazon EC2
- **Virtual Machines in the Cloud**
- **Launch in Minutes** 
- **Scale Automatically** 
- **Pay-As-You-Go** 
**Exam Tip (SAA-C03):** EC2 gives you _full OS control_ this is the key differentiator vs. serverless (Lambda) or managed container services (ECS Fargate).

Choose EC2 when you need:
- **Full OS & processor control**
- **Cost flexibility** 
- **Any workload** 

#### Key steps to provision EC2 instance
Launching an EC2 instance from the AWS Console involves **9 key decisions** made in order:

1. **AMI** – Defines the OS and pre-installed software.
2. **Instance Type** – Sets CPU, memory, storage, and network capacity.
3. **Storage** – Root volume + any additional data volumes.
4. **User Data** – Bootstrap scripts that run automatically at first launch.
5. **Network Settings** – VPC, subnet, and whether to assign a public IP.
6. **IAM Role** – Grants the instance permissions to call other AWS services.
7. **Tags** – Key-value metadata for billing, automation, and access control.
8. **Security Group** – Acts as a virtual firewall controlling inbound/outbound traffic.
9. **Key Pair** – SSH access for Linux; used to decrypt the admin password for Windows.
Each step is covered in detail in the following sections.
**Exam Tip:** The SAA-C03 frequently tests the **IAM role + security group** combination. Key rule: always attach an IAM role to grant EC2 access to AWS services **never store AWS access keys on an instance** (this is a critical anti-pattern and a security violation).
## Amazon Machine Image (AMI)
#### Select an Amazon Machine Image (AMI)
- Is a template that is used to create an EC2 instance (Virtual Machine) that runs in the AWS cloud
- Often has some software pre-installed
**AMI choices**
- Quick start - Linux and Windows AMIs provided by AWS
- My AMIs - Any AMIs that you create
- AWS Marketplace - Pre-configured templates from third parties
- Community AMIs - AMIs shared by others

When launching an EC2 instance, consider these five key factors:

**Region** – AMIs are Region-specific; choose one in your target Region or copy it as needed.
**OS** – Select from AWS-provided Windows or Linux variants.
**Root Storage** – Choose **EBS-backed** for persistent data or **instance store-backed** for temporary storage.
**Architecture** – Choose **x86_64** (Intel/AMD) or **ARM64** (Graviton) based on workload needs. Note: 32-bit is no longer supported on current-generation instances.
**Virtualization** – Always prefer **HVM** (Hardware Virtual Machine) over PV (Paravirtual) — HVM supports hardware extensions for better performance.

An AMI provides the information that is needed to launch an instance, including the following:
- A template for the root volume: Contains the guest operating system (OS) and perhaps other installed software
- Launch permissions: Controls who can access the AMI
- Block device mappings: Specifies any storage volumes to attach to the instance
**AMI benefits**: Repeatability - Reusability - Recoverability

**Exam Tip:** **Instance store-backed** instances cannot be stopped — they can only be rebooted or terminated. This is a commonly tested distinction. Also, when an EBS-backed instance is stopped, you are **not charged for compute** but continue to pay for EBS storage.

**AMI Lifecycle & Deployment Across Regions**:
1️⃣ **Create Source AMI** – Start with an AWS Quick Start AMI or import a VM using VM Import/Export.
2️⃣ **Launch & Customize** – Deploy an instance and configure it with required OS and application settings.
3️⃣ **Capture as AMI** – Create a new AMI from the modified instance.
4️⃣ **Deploy Across Regions** – Optionally, copy the AMI to other AWS Regions for broader availability.

**Tip:** Use **EC2 Image Builder** for automation and manage AMIs across accounts with **AWS Organizations**.
**Key Benefits:**
**Minimalist Images** – Include only essential software & settings, reducing security risks.
**Automated Validation** – Test images using AWS-provided or custom tests before production.
**Version Control** – Manage revisions efficiently.

#### Selecting the Right EC2 Instance Type
| **Instance Name** | **vCPU** | **Memory (GB)** | **Storage** |
| ----------------- | -------- | --------------- | ----------- |
| t3.nano           | 2        | 0.5             | EBS-Only    |
| t3.micro          | 2        | 1               | EBS-Only    |
| t3.small          | 2        | 2               | EBS-Only    |
| t3.medium         | 2        | 4               | EBS-Only    |
| t3.large          | 2        | 8               | EBS-Only    |
| t3.xlarge         | 4        | 16              | EBS-Only    |
| t3.2xlarge        | 8        | 32              | EBS-Only    |
The table above shows you the configuration for many instance types of the "t" family. As the size of the instance type increases, the amount of vCPU, memory, and storage also increases.
To make the right choice, you need to consider the instance’s workload performance needs and cost requirements. Then, choose the instance type with the configuration that best matches those parameters.

**EC2 instance type naming and sizes**: 
- Family
- Generation
- Processor family
- Additional capabilities
- Size
Example: **c6gn.medium**
**Which instance type suits your needs?**
![[Pasted image 20260921031919.png]]
**Exam Tip:** Know the five main instance family categories and a representative use case for each: **General Purpose (M/T)**, **Compute Optimized (C)**, **Memory Optimized (R/X)**, **Storage Optimized (I/D)**, **Accelerated Computing (P/G/Inf/Trn)**.
What if you need to create a new instance?
- In the EC2 console, use the Instance Types page to filter by characteristics that you choose.
- Recommendation: The latest generation in an instance family typically has a better price-to-performance ratio.

You can get recommendations for optimizing the instance type by using the AWS Compute Optimizer.
##### **AWS Compute Optimizer**
AWS Compute Optimizer analyzes historical utilization metrics for EC2 instances, Auto Scaling groups, Lambda functions, and EBS volumes to suggest cost and performance improvements. It uses machine learning to recommend right-sized instance types across all major families.

**Exam Tip:** Compute Optimizer is the AWS-recommended tool for **right-sizing** EC2 instances. It needs at least **14 days of CloudWatch metrics** to make recommendations. It does not make recommendations for instances in a stopped state.
**Findings Categories:**
**Under-provisioned** – Needs more resources.
**Over-provisioned** – Uses more than required.
**Optimized** – Well-configured.
**None** – Not enough data or unsupported instance type.

#### Amazon EC2 storage overview
EC2 instances have four main storage options:
- Instance store
- Amazon Elastic Block Store (Amazon EBS)
- Amazon Elastic File System (Amazon EFS)
- Amazon FSx for Windows File Server
![[Pasted image 20260921032611.png|530]]
An **instance store** provides temporary block storage using physically attached disks. Best for scratch data, buffers, caches, and temp files.
- **Instance-Specific**: Physically attached to one host; not network-based.
- **Persistence**: Data survives **reboots** but is **permanently lost** on stop, termination, or hardware failure.
- **Limitations**: Instance store-backed instances **cannot be stopped** — only rebooted or terminated.

**Amazon EBS** volumes provide network-attached persistent storage to an EC2 instance.
**Characteristics:**
- Is persistent block-level storage
- Can attach to any instance in the same Availability Zone
- Uses HDD or SSD
- Can be encrypted
- Supports snapshots that are persisted to S3
- Data persists independently from the life of the instance
#### Amazon EBS Volume Types
###### **Amazon EBS SSD-backed volume types**
Amazon Elastic Block Store (EBS) offers SSD-backed volumes optimized for transactional workloads that require frequent, small I/O operations where IOPS (Input/Output Operations Per Second) are crucial.
**Current SSD-backed EBS Volume Types:**
- **General Purpose SSD (gp3):** The **current default** and recommended for most workloads. Decouples IOPS and throughput from storage size (no wasted storage on performance)
   - **Use Cases:** Transactional workloads, virtual desktops, medium-sized single-instance databases, low-latency interactive applications, boot volumes, development, and test environments
- **Provisioned IOPS SSD (io2 / io2 Block Express):** For I/O-intensive workloads like large relational databases.
  - **Use Cases:** I/O-intensive database workloads, latency-sensitive transactional applications requiring sub-millisecond latency
**Exam Tip:** **gp3** is now the default EBS volume type in the Console. gp2 is still available but being phased out in favor of gp3. If a question asks about provisioning IOPS independently of storage size, the answer is **gp3** (or io2 for extreme IOPS needs).

###### **Amazon EBS HDD-backed volume types**
Amazon Elastic Block Store (EBS) offers HDD-backed volume types optimized for large, sequential workloads where throughput, measured in MiB/s, is a key performance metric.
**HDD-backed EBS Volume Types:**
1. **Throughput Optimized HDD (st1):** Ideal for frequently accessed, throughput-intensive workloads with large datasets and large I/O sizes.
    -  **Use Cases:** Big data, data warehouses, log processing, and ETL workloads
2. **Cold HDD (sc1):** Provides the lowest cost per GB of all EBS volume types, suitable for less-frequently accessed workloads with large, cold datasets.
    - **Use Cases:** Infrequently accessed data, such as large, cold datasets
###### **Amazon EBS-optimized instances**
EBS-optimized instances provide **dedicated network bandwidth** to EBS volumes, eliminating I/O contention with general instance traffic.
- **Dedicated Bandwidth** – Ranges from 425 Mbps to 14,000 Mbps, depending on instance type.
- **Enabled by Default** – All current-generation instance types are EBS-optimized by default at no extra cost.
- **AWS Nitro System** – Nitro-based instances offload EBS and network I/O to dedicated hardware, significantly improving performance and security.
   - replace the process background management tasks—like encryption, networking, and **storage I/O for EBS volumes** by installing a physical, dedicated chip inside the server
**Exam Tip:** For high-throughput database workloads, always pair **io2** with an **EBS-optimized instance**. The SAA-C03 may ask which combination delivers the highest sustained IOPS.
![[Pasted image 20260921053127.png|335]]
###### **Amazon Elastic File System (Amazon EFS)**
What if you have multiple instances that must use the same storage?
Amazon EFS and Amazon FSx for Windows File Server provide a shared file system for Linux instances and Windows instances.
**Amazon EFS Characteristics:**
- Provides file system storage for Linux-based workloads.
- Fully managed elastic file system
- Scales automatically up or down as files are added and removed
- Petabytes of capacity
- Supports Network File System (NFS) protocols
- Mounts the file system to the EC2 instance
- Compatible with all Linux-based AMIs for Amazon EC2
**Amazon EFS with Standard class storage**
![img|441](https://d3q5r8dtv7rjiz.cloudfront.net/learning_path_images/1782052532-A56946C6486A53F2.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9kM3E1cjhkdHY3cmppei5jbG91ZGZyb250Lm5ldC9sZWFybmluZ19wYXRoX2ltYWdlcy8xNzgyMDUyNTMyLUE1Njk0NkM2NDg2QTUzRjIucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzkwMDQyODM3fX19XX0_&Signature=ihFENP3w7xMEsFLQult079F4djzBs2qC3xkDX-LqXzU0exAQ-ZKketgXe1hgrpxCC9vJcuGe6kxzGrzynZtHx07dMggKr4VoIiIxi0eaGdtVx2FV~WgMYSyjIQnKRoparr9e9Xz1qi5IOs~2z~RP3xZM7CzNd135AcVVXV5tC21tzYttDr~1pdOqi3zlSbptrPtQYLq~zmpvOM-4hi70dG-GByF5hcX0bdKYQyDexkZYndix0UA1OVZDv8ugBC~KqdyHGdUBRcBseGAVMplFlxA4k4O63OZLJRtG-UaJBzX3X9MkFgSzU66n0dPo5iDZHrKJlY5xwsQSPcqb49AVJw__&Key-Pair-Id=K3HIRDOJCEHHR3)

The diagram illustrates an Amazon EFS file system with standard class storage, accessible from multiple Availability Zones. Each zone includes a mount target with an IP address, allowing EC2 instances to connect to the file system.
Within a VPC, mount targets enable access to the file system. Standard storage classes support a mount target in each Availability Zone, while One Zone storage classes limit mount targets to a single zone.
**Amazon EFS with One Zone class storage**

![img|422](https://d3q5r8dtv7rjiz.cloudfront.net/learning_path_images/1782052577-4A01A21B281CB8F4.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9kM3E1cjhkdHY3cmppei5jbG91ZGZyb250Lm5ldC9sZWFybmluZ19wYXRoX2ltYWdlcy8xNzgyMDUyNTc3LTRBMDFBMjFCMjgxQ0I4RjQucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzkwMDQyODM3fX19XX0_&Signature=NFZo740zjbaHpX6eOvqpgoG3Q~BCB3ip4Ig9ejTG1Ypq1bYaWTYvEaAq-0PXIOZLsfHKk5QrHcEyx1337Wq1JaAe968cWsrHi59uxtiLdJUyoO2Sw3~G9xhv7GioGAvzuem9-VNHCELWk5kZT-jxuRGlBQSuW0T4SUZ4~Smo8bGzoU5dO18X9Qx0AFyTtWFs3FJX9pua53PBx5mdH3YxYzQqHeGSNN8YjJTM4cQFDv1Ve-BpOsJB~AyIyIcrmiRV5qB9ezn9nL6cWH1N4ew8K2HRr~3bUzvfxqsU7zn3RKowL7B6tURK6WrX1wnto~g14nbO8bwa-YFQNAzE9YZRGg__&Key-Pair-Id=K3HIRDOJCEHHR3)

The VPC has two Availability Zones, each with one subnet. The file system uses One Zone storage classes, so it can only have a single mount target. For better performance and cost, we recommend that you access the file system from a mount target in the same Availability Zone as the EC2 instance that you're mounting it on.
In this example, the EC2 instance in the other Availability Zone will pay EC2 data access charges for accessing a mount target in a different Availability Zone.

**Hint for cost reduction:**
Use Amazon EFS with a Standard storage class file system if your data is actively accessed and requires the highest durability and availability. For data that is less frequently accessed and doesn’t require the highest levels of durability and availability, use One Zone storage classes. For workloads that don’t require Multi-AZ resilience, One Zone storage file system can result in a 47 percent lower price compared to file systems using Standard storage classes.

###### **Amazon FSx for Windows File Server**
Amazon FSx for Windows File Server provides fully managed Windows file storage for EC2 instances, using a native Windows file system.
Built on Windows Server, it supports key Windows features like **NTFS**, **SMB** for network access, **DFS** for organizing distributed file shares, and **Active Directory** for resource management.
FSx leverages SSD storage for high performance, ensuring low latency, high throughput, and optimized IOPS while reducing administrative overhead.

#### EC2 instance user data
When launching an EC2 instance, you can pass user data to initialize it. This allows automated tasks like updating installed software, installing license keys, or adding new applications.
User data scripts, using shell commands or cloud-init directives, run with root or Administrator privileges during the **first boot** of the instance, after the instance has network access.

**Exam Tip:** User data runs **only once** at first launch by default. To make it run on every boot, use a cloud-init `boothook` or AWS Systems Manager. User data is **limited to 16 KB** in size.

**Retrieving instance metadata**
Every EC2 instance exposes metadata about itself through the **Instance Metadata Service (IMDS)** — a link-local endpoint accessible only from within the instance. AWS offers two versions:
**IMDSv1 (legacy):** A simple GET request, no authentication required:
```bash
curl http://169.254.169.254/latest/meta-data/instance-id
```
**IMDSv2 (recommended — security best practice):** Session-oriented. Requires a PUT request to get a short-lived token first, then uses that token for all metadata calls. This blocks SSRF attacks where a compromised app could silently call the metadata endpoint on behalf of an attacker.
```bash
# Step 1: Get a session token (TTL = 6 hours)
TOKEN=$(curl -sX PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

# Step 2: Use the token to retrieve metadata
curl -sH "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id
```
**Exam Tip (SAA-C03):** IMDSv2 protects against **SSRF (Server-Side Request Forgery)** attacks. To enforce IMDSv2 on new instances, set `HttpTokens: required` in the instance launch configuration or via AWS Config rules. This is tested in security-focused architecture scenarios.

User data passed at launch is readable at `http://169.254.169.254/latest/user-data/` — useful for scripts that need to inspect their own configuration.
#### Custom AMIs as a base configuration
To ensure all EC2 instances in your organization have a predefined software setup, you can create a **custom AMI** as a base configuration.
**Enforcing compliance:**
- Regularly scan running instances.
- Terminate any instance not using the approved AMI.
----
![[Pasted image 20260921093714.png|505]]
You can configure EC2 instances at boot time using **user data scripts**. This method works well alongside **custom AMIs**, where some configurations are built into the base image, while others are applied dynamically at launch.

**Factors to Consider:**
- **Build times:** More pre-installed configurations increase AMI build time.
- **Boot times:** A minimal AMI takes longer to boot as installations occur at launch.
- **Shelf life:** More dependencies in an AMI increase the need for frequent updates to avoid security risks.
**Approaches and Tradeoffs:**
- **Full AMI:** Fast boot, long build time, short lifespan. (Gold AMI)
- **Partially Configured AMI:** Balanced boot speed and build time, easier rollback. (Silver AMI)
- **OS-only AMI:** Fast build, slow boot, highly configurable
----
**Placement groups** give you control over where instances run relative to each other, affecting latency and fault tolerance. Choose based on your workload:
![[Pasted image 20260921095246.png]]
**Exam Tip:** **Spread** is for maximum fault isolation of a small set of instances (limit: 7/AZ). **Cluster** is for maximum network performance. Instances with a tenancy of **dedicated host cannot** be in a placement group.

---
##### **Tags**
- A tag is a label that you can assign to an AWS resource
- Consists of a _**Key _and an optional_** value_
- Tagging is how you can attach metadata to an EC2 instance
- Potential benefits of tagging: filtering, automation, cost allocation, access control

##### **Key pair**
At instance launch, you specify an _exiting_ _key pair_ or create a _new key pair_
A _key pair_ consists of:
    - A _**public**_ key that AWS stores
    - A _**private**_ key that you store
It enables secure connection to the instance
- For windows instances:
    - Use the key to obtain the administrator password
- For Linux instances:
    - Use the private key to securely connect to the instance using SSH protocol
![[Pasted image 20260921100018.png|481]]
The example command should be similar to this:
```bash
ssh -i "AWS-HQ.pem" ec2-user@ec2-15-237-248-77.eu-west-3.compute.amazonaws.com
 
Or using the public ip address:
 
ssh -i "AWS-HQ.pem" ec2-user@15.237.248.77
```

#### Amazon EC2 purchase models
- **On-Demand** — Pay per second (Linux/Ubuntu) or per hour with no commitment. Best for: unpredictable, spiky, or experimental workloads. Highest unit price but maximum flexibility.
- **Reserved Instances (RI)** — 1-year or 3-year commitment in exchange for up to 72% discount over On-Demand. Two key types:
    - **Standard RI:** Fixed instance type/family/Region — highest discount, least flexible.
    - **Convertible RI:** Can change instance type/family/OS during the term — up to 54% discount.
- **Savings Plans** — Commit to a $/hour spend for 1 or 3 years; AWS applies the discount automatically. Two types:
  - **Compute Savings Plans:** Broadest flexibility — applies across EC2, Lambda, and Fargate, any region/family/OS. Up to 66% savings.
  - **EC2 Instance Savings Plans:** Specific to one instance family in one Region. Up to 72% savings (similar to Standard RI).
> **Exam Tip:** If a question says "flexible workloads across multiple instance families or services," Compute Savings Plans is the answer.
- **Spot Instances** — Use AWS spare capacity at up to 90% off On-Demand. AWS can reclaim the instance with a **2-minute warning** when capacity is needed. Best for: fault-tolerant, stateless, batch, or containerized workloads that can handle interruption.
#### Amazon EC2 Capacity Reservations
The second option is Capacity Reservations and it lets you reserve compute capacity for Amazon EC2 instances in a specific Availability Zone.
- **On-Demand Capacity Reservations**
	This guarantees that you always have access to EC2 capacity when you need it, for as long as you need it.
	Recommended use cases:
	- Workloads that need to meet regulatory requirements for high availability
	- Workloads that require capacity assurance
- **Amazon EC2 Capacity Blocks for ML**
	Reserve GPU instances for a future date to run any of your machine learning (ML) workloads.
	Recommended use cases:
	- Training and fine-tuning ML models
	- Running experiments and building prototypes
	- Planning for future surges in demand for ML applications
#### Amazon EC2 dedicated options
- Dedicated Instances
    - Per-instance billing
    - Automatic instance placement
    - Isolates the hosts that run your instances
- Dedicated Hosts
    - Per-host billing
    - Visibility of sockets, cores, and host ID
    - Affinity between a host and an instance
    - Targeted instance placement
    - Add capacity by using an allocation request
    - Lets you to use your server-bound software licenses and address compliance requirements
Please notice that: **Dedicated Hosts** give you visibility and control over how instances are placed on a physical server. You can consistently deploy your instances to the same physical server over time. As a result, Dedicated Hosts let you to use your existing server-bound software licenses, and to address corporate compliance and regulatory requirements.
----
#### Applying the AWS Well-Architected Framework principles to compute resources
###### **Security pillar**
**Automate compute protection**
Use EC2 Image Builder to produce hardened, minimal AMIs on a schedule — reducing the window of exposure to unpatched vulnerabilities. Automate patching using AWS Systems Manager Patch Manager rather than relying on manual SSH access.
EC2 features that support this best practice: **EC2 Image Builder**, **User data scripts**, **IMDSv2**

**Control traffic at all layers**
Apply a **defense in depth** approach: use security groups (stateful, instance-level) alongside Network ACLs (stateless, subnet-level). Never open `0.0.0.0/0` on SSH or RDP ports to production instances — use AWS Systems Manager Session Manager instead, which requires no open inbound ports
###### **Performance Efficiency pillar**
**Select the right compute for your workload**
**Right-size compute resources**
###### **Reliability pillar**
**Deploy across multiple Availability Zones**
**Use EBS for persistent storage**
###### **Cost Optimization pillar**
**Match pricing model to workload pattern**
**Right-size before you commit**
###### **Operational Excellence pillar**
**Use infrastructure as code**
Provision EC2 infrastructure via AWS CloudFormation or CDK to make deployments repeatable and auditable. Avoid manual console changes in production.
**Enable observability**
###### **Sustainability pillar**
**Best practice:** Use the most efficient instance for your workload and avoid idle capacity. AWS Graviton instances offer better performance-per-watt than equivalent x86 instances, reducing carbon footprint. Use Auto Scaling to turn off capacity during low-demand periods.