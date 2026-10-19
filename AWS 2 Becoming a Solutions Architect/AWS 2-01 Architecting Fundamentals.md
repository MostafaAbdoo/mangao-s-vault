https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html
#### AWS Global Infrastructure
- Regions
- Availability Zones
- AWS Local Zones
- AWS Data Centers
- Edge Locations
- Regional Edge Caches
An AWS Region is a physical geographical location with **at least three Availability Zones**
Availability Zones, consist of one or more data centers

Regions are connected to ISPs and a private global network, which reduces latency compared to the public internet
Availability Zone is interconnected with other Availability Zones in a Region using high-speed private links.
**Selecting a region**:
When selecting an AWS region, consider these factors:
- Latency: Choose a region close to your users for lower network latency.
- Cost: Pricing varies by region, so opt for one that fits your budget.
- Compliance: Ensure the region meets regulatory and data residency requirements.
- Service Availability: Not all AWS services are available in every region.

**SAA Exam Tip:** AZ letter identifiers (a, b, c) are mapped per AWS account — `us-east-1a` in your account may not be the same physical data center as `us-east-1a` in another account. AWS shuffles these to distribute load evenly.


**Local Zones** are AWS infrastructure deployments that bring AWS services like compute, storage, and databases closer to areas with a major population. They are designed for latency-sensitive applications, offering single-digit millisecond latency for use cases like real-time gaming, media content creation, and machine learning.
Each Local Zone is an extension of an AWS Region, allowing you to run applications nearby while maintaining a secure, high-bandwidth connection to your in-Region workloads

**SAA Exam Tip:** Don't confuse these three infrastructure extension types — the exam tests the distinction:
- **Local Zones** — metro-area latency for cities without a full Region (e.g., Los Angeles Local Zone)
- **Wavelength Zones** — embed AWS compute inside telecom 5G networks for ultra-low latency mobile/edge workloads
- **AWS Outposts** — fully managed AWS rack deployed in your on-premises data center; same APIs, same console
If a scenario mentions 5G or mobile edge → Wavelength. On-prem/data center extension → Outposts. Low-latency for a major metro city → Local Zone.
#### Availability
- **Multiple Availability Zones (AZs)**: AWS regions are divided into isolated Availability Zones, which are separate data centers. By distributing resources across multiple AZs, applications can continue running even if one AZ fails.
- **Load Balancing**: AWS Elastic Load Balancers distribute traffic across healthy instances in multiple AZs, ensuring continued availability during failures or traffic spikes.
- **Auto Scaling**: AWS Auto Scaling automatically adjusts the number of instances based on demand, maintaining performance and availability.
- **Fault Tolerance**: Services like Amazon S3, RDS, and DynamoDB are designed with built-in fault tolerance, ensuring data availability and reliability.
![[Pasted image 20260830040958.png]]
**Edge locations** in AWS are data centers located globally that serve as endpoints for AWS services, primarily for content delivery and caching. They are part of Amazon CloudFront and are designed to reduce latency by bringing content closer to users.
- **Content Delivery**
- **Reduced Latency**
- **Global Reach**
**SAA Exam Tip:** Edge locations are used by **CloudFront**, **Route 53**, and **AWS Global Accelerator**

**Regional edge caches**:
CloudFront also has regional edge caches that bring more of your content closer to your viewers, even when the content is not popular enough to stay at a POP, to help improve performance for that content.
Regional edge caches are used by default with CloudFront. They are used when you have content that is not accessed frequently enough to remain in an edge location. Regional edge caches absorb this content and provide an alternative to fetching the content from the origin server.

**SAA Exam Tip:** High availability = multiple AZs in the same Region. Disaster recovery = multiple Regions
**SAA Exam Tip:** On the exam, you are essentially being tested as a cloud architect. Each scenario question gives you a set of business/technical requirements and asks you to pick the architecture that best meets them. Thinking in terms of "what does this customer actually need" — not "what is the most sophisticated solution" is the right mindset.

**The role of a cloud architect includes:**
- Engaging with decision-makers to identify business goals and areas needing improvement.
- Ensuring that the technology deliverables of a solution are aligned with the business objectives.
- Collaborating with delivery teams to ensure the technology features being implemented are suitable.
Cloud architects manage an organization's cloud computing architecture. They possess in-depth knowledge of architectural principles and services to:
- Develop a technical cloud strategy based on business needs.
- Assist with cloud migration efforts.
- Review workload requirements.
- Provide guidance on addressing high-risk issues.
#### Pillars of the Well-Architected Framework
The AWS Well-Architected Framework serves as a guide for consistently evaluating cloud architectures and offers guidance for implementing designs. It outlines a set of foundational questions and best practices that can help you determine if a specific architecture aligns with cloud best practices.
- Operational Excellence
- Security
- Reliability
- Performance Efficiency
- Cost Optimization
- Sustainability
**SAA Exam Tip:** All six pillars are fair game on the SAA-C03. The exam most frequently tests **Security**, **Reliability**, and **Cost Optimization,** usually through scenario questions that ask which design decision violates or upholds a given pillar. When you see a scenario describing a failure or outage, think Reliability. When you see unused instances or over-provisioned resources, think Cost Optimization. When you see data exposed without encryption, think Security.

**Operational Excellence**:
The Operational Excellence pillar focuses on the ability to run systems efficiently and gain insights into their operations to deliver business value. It also emphasizes the continuous improvement of supporting processes and procedures.
**SAA Exam Tip:** Operational Excellence services to know: **CloudWatch** (monitoring + alarms), **CloudFormation/CDK** (IaC), **AWS Systems Manager** (automated runbooks, patch management), and **AWS X-Ray** (distributed tracing). If a scenario asks how to reduce manual operational overhead or improve deployment consistency, this pillar is in play.

**Security**:
The Security pillar focuses on the ability to protect information, systems, and assets while delivering business value by conducting risk assessments and implementing mitigation strategies.

**Reliability**:
The Reliability pillar focuses on a system's ability to recover from infrastructure or service disruptions and dynamically scale computing resources to meet demand. It also covers the system's capacity to mitigate disruptions like misconfigurations or transient network issues.
**SAA Exam Tip:** Reliability = Multi-AZ architecture. Key services: **RDS Multi-AZ** (automatic failover for databases), **Elastic Load Balancer** (distribute traffic across healthy instances), **Auto Scaling Groups** (replace unhealthy instances), **Route 53 health checks** (DNS failover), and **S3** (11 nines of durability through redundant storage)

**Performance Efficiency**:
When focusing on performance, the goal is to maximize efficiency in using computational resources while maintaining that efficiency as demand fluctuates
**SAA Exam Tip:** Performance Efficiency is about choosing the _right_ resource, not the most powerful one. Key decision points: **EC2 instance families** (compute-optimized C-family, memory-optimized R-family, storage-optimized I-family), **ElastiCache** for offloading repeated database reads, **CloudFront** for serving content with low latency, **RDS Read Replicas** for scaling read-heavy workloads

**Cost Optimization**:
Cost optimization is an essential, ongoing aspect of any well-designed architecture. This process is iterative and should be continuously refined and improved throughout the production lifecycle. Evaluating the efficiency of your current architecture in relation to your goals can help eliminate unnecessary expenses.
**SAA Exam Tip:** Know the EC2 pricing models cold this is one of the most frequently tested topics: **On-Demand** (no commitment, full price best for unpredictable/short workloads), **Reserved Instances / Savings Plans** (1 or 3 year commitment, up to 72% savings best for steady baseline workloads), **Spot Instances** (up to 90% savings, can be interrupted best for fault-tolerant batch or stateless workloads). Anti-pattern: using On-Demand for a 24/7 production database instead of Reserved. Also know: **AWS Cost Explorer** (visualize spend), **AWS Budgets** (set alerts), **Compute Optimizer** (right-sizing recommendations).

**Sustainability**:
The Sustainability pillar focuses on building architectures that maximize efficiency and reduce waste. The discipline of sustainability considers the long-term environmental, economic, and societal impacts of your business activities. It's important to understand the impact of your workloads and take steps to reduce their downstream effects.
**SAA Exam Tip:** Sustainability was added as the 6th pillar in **November 2021** know this date, as the exam may test awareness that the framework was updated.

**AWS Well-Architected Tool (WA Tool)**: It enables you to review workloads, compare them to AWS best practices, and receive an action plan with step-by-step guidance for improvement. By answering questions across all **six pillars operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability** 

---
#### Best practices for building solutions on AWS
##### **Design trade-offs**:
No system optimizes everything at once. You must balance speed, cost, reliability, and complexity.
##### **Implementing scalability**:
Ensure that your architecture can handle changes in demand.
*SAA Exam Tip:* Know the three Auto Scaling policy types: **Target Tracking** (simplest maintain a metric like 60% CPU), **Step Scaling** (react to thresholds in steps), and **Scheduled Scaling** (predictable traffic patterns). Target Tracking is the recommended default.
##### **Automating your environment**:
Automate the provisioning, termination, and configuration of resources.
AWS offers built-in monitoring and automation tools to help infrastructure respond quickly to changes. Without these tools, failures must be detected and resolved manually, increasing downtime.
![[Pasted image 20260902040029.png]]
###### **Using IaC**:
*Infrastructure as Code (IaC)* automates infrastructure deployment, reducing manual effort and errors. It enables rapid deployment of identical environments using templates, eliminating repetitive tasks.
**AWS IaC options:**
- **AWS CloudFormation** AWS-native, JSON/YAML templates, supports StackSets for multi-account/multi-Region deployments
- **AWS CDK (Cloud Development Kit)** Define infrastructure in Python, TypeScript, or Java compiles to CloudFormation
- **Terraform** Multi-cloud IaC, widely used in production environments

**SAA Exam Tip:** The exam focuses on **CloudFormation**. Know that StackSets allow you to deploy the same stack across multiple accounts and Regions in one operation. This is the answer for any multi-account governance question.
##### **Treating resources as disposable**
It means managing infrastructure like software rather than hardware. Instead of overprovisioning costly physical resources, replace instances as needed. It improves flexibility, cost efficiency, and responsiveness to changing capacity demands.
##### **Using loosely coupled components**
Design architectures with independent components by using managed solutions like **load balancers** and **message queues** as intermediaries
##### **Designing services, not servers**
This means choosing managed and serverless solutions over traditional EC2 instances when appropriate.
##### **Choosing the right database solution**
- Match technology to the workload, not the other way around
- Read and write needs
- Total storage requirements
- Typical object size and nature of access to these objects
- Durability requirements
- Latency requirements
- Maximum concurrent users to support
- Nature of queries
- Required strength of integrity controls
**SAA Exam Tip:** Database selection is one of the highest-frequency topics on the SAA-C03. Quick decision guide:
- **RDS / Aurora** → relational data (SQL), managed, automated backups and failover. Aurora is up to 5× faster than standard MySQL.
- **DynamoDB** → key-value / document, single-digit millisecond latency at any scale, fully serverless.
- **Redshift** → analytics and data warehousing (OLAP workloads, not OLTP).
- **ElastiCache (Redis or Memcached)** → in-memory caching to offload read pressure from databases.
- **Neptune** → graph database (social networks, fraud detection, recommendation engines).
- **DocumentDB** → MongoDB-compatible document database.
##### **Avoiding single points of failure and caching**
**SAA Exam Tip:** Know your caching layers: **CloudFront** for static/dynamic content at the edge, **ElastiCache (Redis)** for session data and database query results, **DAX** for DynamoDB read acceleration.
##### **Optimizing for cost**
- Are my resources the right size and type for the job?
- Which metrics should I monitor?
- How do I turn off resources that are not in use?
- How often will I need to use this resource?
- Can I replace any of my servers with managed services?

**SAA Exam Tip:** Cost optimization levers, from most to least frequently tested:
1. **Spot Instances** up to 90% savings; use for stateless, fault-tolerant batch workloads (Spot can be interrupted with 2-min notice).
2. **Reserved Instances / Savings Plans** 1 or 3 year commitment; best for steady baseline workloads. Savings Plans are more flexible (apply across instance families/regions).
3. **Right-sizing** **AWS Compute Optimizer** analyzes CloudWatch metrics and recommends the optimal instance type. **Trusted Advisor** also flags underutilized resources.
4. **S3 Intelligent-Tiering** automatically moves objects between tiers based on access patterns; ideal when you can’t predict access frequency.
5. **Scheduling** stop/start non-production environments on a schedule using **AWS Instance Scheduler** or Lambda.