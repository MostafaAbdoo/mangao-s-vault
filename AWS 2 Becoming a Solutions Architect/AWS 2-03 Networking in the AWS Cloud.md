![[Pasted image 20260912042821.png]]
**Peering connections**: Use a VPC peering connection to route traffic between the resources in two VPCs.
**Traffic Mirroring**: Copy network traffic from network interfaces and send it to security and monitoring appliances for deep packet inspection.
**Transit gateways**: Use a transit gateway, which acts as a central hub, to route traffic between your VPCs, VPN connections, and AWS Direct Connect connections.
- without Transit gateways u need $\frac{N(N - 1)}{2}$ peering connections.
**VPC Flow Logs**: A flow log captures information about the IP traffic going to and from network interfaces in your VPC.
**VPN connections**: Connect your VPCs to your on-premises networks using AWS Virtual Private Network (AWS VPN).

**SAA-C03 Exam Tips**
- VPC CIDR blocks must be between **/16** (65,536 IPs) and **/28** (16 IPs). You cannot change a VPC CIDR after creation, but you can add **secondary CIDR blocks**.
    - A /28 subnet only has 16 total IPs, and AWS reserves 5 of them leaving only 11 usable hosts
- AWS creates a **default VPC** (172.31.0.0/16) in every region with a default internet gateway already attached. Instances launched in the default VPC get a public IP by default.
- A VPC is region-scoped; subnets are AZ-scoped.
![[Pasted image 20260912044257.png]]
If your VPC CIDR block is 10.0.0.0/16, you could create two subnets:
- Subnet 1: 10.0.0.0/24 (256 total, 251 usable)
- Subnet 2: 10.0.1.0/24 (256 total, 251 usable)
**Key Considerations:**
- **IP Address Range**: Choose a CIDR block that accommodates future growth. Prefer /16 for production VPCs.
- **No Overlapping CIDRs**: If you plan to peer VPCs, their CIDR blocks must not overlap.
- **Subnet Design**: Separate public-facing resources (web servers) from private ones (databases) using different subnets.
- **NAT**: Instances in private subnets need a NAT gateway to reach the internet.
     - Private subnet -> NAT            Public subnet -> Internet gateway

in a subnet with CIDR block 10.0.0.0/20, AWS reserves the following five IP addresses for:
- 10.0.0.0: Network address
- 10.0.0.1: VPC local router
- 10.0.0.2: DNS resolution
- 10.0.0.3: Future use
- 10.0.15.254: Network broadcast address
When designing subnets, it's important to allocate enough IP addresses to support your resource needs.

**Internet gateway**: An internet gateway permits communication from VPC to the internet.
It will have the route as 0.0.0.0/0 and the target as IGW-xxxxx in the routing table
For an EC2 instance to communicate over the internet, it must have a public IPv4 or an Elastic IP address.

**NAT gateway**: A NAT gateway permits instances in the private subnet to connect outside the VPC. However, anything outside the VPC cannot initiate a connection. It will be sent a RESET flag.

A **NAT gateway** is a fully managed AWS service that scales automatically and requires no maintenance. It needs an **Elastic IP (EIP)** and must be in a **public subnet** for Internet access. AWS assigns it a private IP from the subnet. For redundancy, deploy NAT gateways in multiple **availability zones**.
![[Pasted image 20260912045215.png]]
1. **Private Subnet Route Table:**
    - Contains a route directing all outbound internet traffic (`0.0.0.0/0`) to the **NAT Gateway** in the public subnet.
2. **NAT Gateway (in Public Subnet):**
    - Must have an **Elastic IP (EIP)** attached.
    - Translates the private IP address of your internal instance to its own public Elastic IP address.
    - Forwards the request out to the Internet Gateway.
3. **Public Subnet Route Table:**
    - Contains a route directing all outbound traffic (`0.0.0.0/0`) to the **Internet Gateway (IGW)**
4. **Internet Gateway (IGW):**
    - Performs the final network address translation between the public IP space and the AWS infrastructure to route packets out to the public internet

A **NAT instance** is an EC2 instance using a preconfigured Linux AMI. It works like a NAT gateway but with key differences. It **doesn't auto-scale**, so you must manually select and upgrade the instance type.
Unlike NAT gateways, NAT instances have **ENIs** (**Elastic Network Interface**) and require **security groups** and **public IPs**. You must also **disable the source/destination check** for traffic forwarding.

#### Route tables
![[Pasted image 20260912051326.png]]
**Destination:** The destination is an IP address and CIDR range (for example, 0.0.0.0/0, which is the internet).
**Target:** A target is either a gateway or network interface. It is for the destined traffic

- Each subnet can be associated with **only one route table**, but a single route table **can be shared across multiple subnets**.
- If no custom route table is assigned to a subnet, it automatically uses the **main VPC route table**.
![[Pasted image 20260912051500.png]]
**Subnet sizing**: Multiple subnets **cannot overlap** in **CIDR blocks**.

To enable **internet access** for a **public subnet**:
1. **Attach an internet gateway** to the VPC (one per VPC).
2. **Launch an EC2 instance** with a **public IP** in the subnet.
3. **Update the subnet's route table** to send **all traffic (0.0.0.0/0)** to the **internet gateway**.

**Exam Tip**: One internet gateway per VPC. An IGW alone isn't enough the route table must also have a 0.0.0.0/0 route pointing to it, and the instance must have a public IP.
![[Pasted image 20260912052219.png]]

###### Private subnets
**Custom route tables**: Best practice to create a **separate route table** per subnet.
**Default behavior**: If no route table is assigned, the **main VPC route table** is used.
**Route table rules**: One subnet **can have only one route table**, but a route table **can be shared**
For **IPv6 private subnets**, use an **egress-only internet gateway** to allow outbound traffic while blocking inbound connections
**NAT device function**: Hides the **private IP** of internal servers by replacing it with a **public IP**.
Process steps:
    1. **Server sends request** (source: **10.0.0.17**) → NAT device.
    2. **NAT replaces source IP** (to **89.89.0.100**) → forwards to destination.
    3. **Destination responds** (destination: **89.89.0.100**) → NAT device.
    4. **NAT maps back to server** (destination: **10.0.0.17**) → forwards response.

![[Pasted image 20260912052638.png]]
#### Security layers of defense
**Security Layers in the Diagram**:
1. **Network ACL (NACL) Layer**: Stateless firewall that filters traffic at the **subnet level**. (**stateless firewall**)
    - **Default ACL allows all traffic by default**, **Custom ACL denies all traffic by default** 
    - **Stateless**: If inbound traffic is allowed, the outbound response **must** have an explicit rule.
    - **Rules are processed in order** (lowest to highest number).
    - **Asterisk rule**: The final, implicit rule that blocks all traffic **unless explicitly allowed**.
    - Acts as a **fail-safe**, ensuring unintended traffic is denied.
2. **Security Group Layer**: Stateful firewall that controls inbound/outbound traffic **at the instance level**. (**stateful firewall**) ........ operates at the **ENI (network interface) level** — not just EC2.
      -  **Inbound traffic**: **Blocked** by default, **Outbound traffic**: **Allowed** by default
      - Allow traffic based on **protocols & ports**
      - if you allow inbound traffic, the return traffic is automatically allowed, regardless of outbound rules.
      - You can also **reference other security groups** as the source/destination in a rule (e.g., allow traffic from the web-tier SG to the app-tier SG)
###### **AWS Network Firewall Overview**
![[Pasted image 20260912071503.png]]
AWS Network Firewall is a **stateful, managed firewall** that enhances security by filtering traffic **before it reaches VPC resources**.
- **Stateful Protection**
    - Tracks active connections.
    - Automatically allows return traffic for established connections.
- **Deployment in Firewall Subnets**
    - Placed **between external sources (e.g., Internet Gateway) and application subnets**.
    - Ensures traffic is **inspected before reaching VPC resources**.
- **Route Table Configuration**
    - **Route Table 1**: Directs inbound traffic **to the firewall** for inspection.
    - **Route Table 2**: Ensures **only filtered traffic** from the firewall reaches private subnets.
- **Traffic Filtering & Security**
    - Custom rules inspect and block malicious traffic.
    - Adds an **extra security layer** alongside **security groups and NACLs**.
----
#### How to connect to managed AWS services
Your EC2 instance in a private subnet needs access to an Amazon S3 bucket in the same Region, but S3 is outside the VPC. Direct internet access increases costs and exposure.

A better solution is using an **S3 VPC Endpoint (Gateway or Interface)**:
- **Gateway Endpoint** (recommended for S3): Routes traffic directly via AWS backbone, avoiding the internet.
- **Interface Endpoint** (ENI-based): Uses private IPs but incurs additional costs.
**The Problem Endpoint Solves**:
By default, Amazon S3 is a public service. To reach an S3 bucket from a private subnet without a VPC endpoint, traffic must route through a **NAT Gateway** in a public subnet, out through the **Internet Gateway**, across the internet, and into S3.
**Cost:** AWS charges per GB for data processed through NAT Gateways. Transferring large datasets or database backups to S3 this way quickly becomes expensive.

#### Interface VPC endpoints
Below is a diagram of an AWS VPC with an interface VPC endpoint. The VPC contains a private subnet with an elastic network interface. The EC2 instance inside the private subnet can access other AWS managed services and Amazon S3 using the elastic network interface belonging to the interface VPC endpoint. The interface VPC endpoint has an IAM policy attached.

![img](https://d3q5r8dtv7rjiz.cloudfront.net/learning_path_images/1781993510-11D0DEBC6FAEF437.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9kM3E1cjhkdHY3cmppei5jbG91ZGZyb250Lm5ldC9sZWFybmluZ19wYXRoX2ltYWdlcy8xNzgxOTkzNTEwLTExRDBERUJDNkZBRUY0MzcucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzg5NDM1MDYwfX19XX0_&Signature=RpzBV4K7DOKCyVe5Sk7czCVLQODVEnb0wpqxKNlzh2PT8Zwp~MpVsL7~maTVAvnD4NwMklPSu~ItKOlnWg5RvofAZhksOYZ0G8sxXTZ6ZUCQJ7whNEtNumW~qoumqjF~NUFhge5pTOUT3hKGzgXph7bKumFsar7rLGoJQBzndG5mCQVVp-MVT1HXwpZlRSmDOGfCnEvq9UBadYTeDWtjLKfAkRJHoLjgKiYjogJNxzKEOp8rW1pE85HMxk~NCpKFir7oJy62Ve0QCXr3MyoB0yU7W37EFiQrkGJmeBZk~OB7O5JUthBI934LL-eUN0G2a29-~KCkmnojjCDOSgVSXA__&Key-Pair-Id=K3HIRDOJCEHHR3)
Types of VPC Endpoints:
1. **Gateway VPC Endpoint** (Recommended for S3 & DynamoDB)
    - Routes traffic within AWS without needing an ENI.
    - No additional cost.
    - Defined in the route table.
2. **Interface VPC Endpoint** (AWS PrivateLink)
    - Creates an **Elastic Network Interface (ENI)** with a private IP in the subnet.
    - Supports IAM policies for fine-grained access control.
    - Incur hourly and data processing charges.
**Best practice**: Use a **Gateway Endpoint** for S3 to reduce costs and improve security. Use **Interface Endpoints** when connecting to services requiring **private IP-based access**
![[Pasted image 20260914043515.png]]
#### Gateway VPC endpoints
![img](https://d3q5r8dtv7rjiz.cloudfront.net/learning_path_images/1781993986-83065A5A4B4D8EA5.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9kM3E1cjhkdHY3cmppei5jbG91ZGZyb250Lm5ldC9sZWFybmluZ19wYXRoX2ltYWdlcy8xNzgxOTkzOTg2LTgzMDY1QTVBNEI0RDhFQTUucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzg5NDM1MDYwfX19XX0_&Signature=G2PAJGyo7JEEJyLbDuMT0j~hP85SUIPkkgkwDmHB5vkkm4M~etB8xXu-3AAaTnfBJxQHy2aQ496S42O3b0ny3hX397MerdURZcFMGNqe6fEoR8eFm4-dTuVr6P2sC2-ScVLrIzmBWHXjZM~BaOvFsPzY0yXuWYqEgO9tsCMhHIKMjqpvqUqs0WbUWqtDMaeaccPeFvrk~8zzcGty2G6YuUoePzJTcdUdi7joWLbhduiCLtTp3C89DCWdvIGiXS3bYoGGSj4kDdq9j7EkkM3JZ0VtkucxV7yPkHSSgRbDBACqnmFJ4lyZ7YU55XnXIwUhjFRWaUZc-E5Clj4VQTeQ1g__&Key-Pair-Id=K3HIRDOJCEHHR3)

**Gateway VPC Endpoints for S3 & DynamoDB**
- **Direct connectivity** via route tables (not AWS PrivateLink).
- **Supported only for Amazon S3 & DynamoDB**.
- No additional cost, no throughput or packet processing limits.
- Uses **prefix lists** to group CIDR blocks for routing
#### Gateway Load Balancer endpoint
![img](https://d3q5r8dtv7rjiz.cloudfront.net/learning_path_images/1781994092-B2E4410F43EFCE4E.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9kM3E1cjhkdHY3cmppei5jbG91ZGZyb250Lm5ldC9sZWFybmluZ19wYXRoX2ltYWdlcy8xNzgxOTk0MDkyLUIyRTQ0MTBGNDNFRkNFNEUucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzg5NDM1MDYwfX19XX0_&Signature=PxbwZLsVvtez2MAaupYD7-cPgcIJZCcDu~XwrleswfQUwfN8MuEtpmHaNNc5UOxxp2gL8mN8aXaptJ4YUHxSV0OTgr93BKFxMv5hurZ1WYsBJH7xKNbci6i8XkKOKndwKEuylX7~ox9Jb29cJluCV398KPXGJ8WcUM1Bvfg0qudABYUfROgL28b7r~L6os8n6vAQDlli2oJC4kunkoZKFR8g4blanaS2oU~xDMHRG34UofuqxJSdyzPMLNInruGDIT7N0eNV8EFST1yVgUlEAisevZZHWEahdpgc4II-n2w9ArK2YDknhrCTtJK3ulKb6MrMI4UL9qH7mLMOVJSMMA__&Key-Pair-Id=K3HIRDOJCEHHR3)

**Gateway Load Balancer Endpoint**
- **Provides private connectivity** for security appliances across VPCs.
- **Uses AWS PrivateLink** to forward traffic between **Customer VPC (VPC 1)** and **Security Service VPC (VPC 2)**.

**Traffic Flow (Steps)**
1. **Inbound traffic** enters **VPC 1** via the **Internet Gateway**, then routes to the **Gateway Load Balancer Endpoint**.
2. Traffic is sent to the **Gateway Load Balancer in VPC 2**, which forwards it to **security appliances** for inspection.
3. The inspected traffic is returned and routed to the **EC2 application instance** in **VPC 1**.

- **Centralized Security Management:** Instead of running virtual firewalls inside every single application VPC, security teams manage one centralized "Inspection VPC".
- **Auto-Scaling Firewalls:** The Gateway Load Balancer scales the fleet of security appliances up or down seamlessly as traffic spikes.
#### Amazon VPC flow logs
**VPC Flow Logs** capture **packet-level traffic details** for analysis.
**Key Features**
- Can **capture all, accepted, or rejected traffic**.
- Can be enabled at the **VPC, subnet, or ENI** level.
- Logs are delivered to **Amazon CloudWatch, S3, or Kinesis Data Firehose**.
**Log Destinations & Use Cases**
- **CloudWatch Logs** → View & filter logs in **AWS Console**.
- **S3 Storage** → Query logs using **Amazon Athena** (supports **plain text & Parquet** formats).
- **Kinesis Data Firehose** → Send logs to **Amazon OpenSearch** (dashboard) or **third-party tools like Splunk**.
**Flow logs operate outside your VPC**, so they **don't impact network latency or performance**.
**What flow logs do NOT capture:**
- Traffic to/from the **Amazon metadata service** (169.254.169.254)
- **DNS** queries to the Route 53 Resolver
- **DHCP** traffic
- Traffic to the **Windows license activation server**
- Traffic between an **NLB network interface** and an instance
> **Exam Tip**: Flow logs don't capture DNS traffic to Route 53 or the instance metadata endpoint. If you need visibility into DNS queries, use **Route 53 Resolver Query Logging** instead.

#### More VPC troubleshooting tools
**Reachability Analyzer**
- **Performs static, logical path analysis** between two VPC resources — it does **not send actual network packets**.
- Checks if **two VPC resources can connect** based on security group rules, NACLs, route tables, and gateways.
- Identifies **blocking components** and shows the exact step where connectivity fails.
**Network Access Analyzer**
- Identifies **unintended network access**.
- Ensures **security & compliance**.
- Example: Verify **credit card data systems** are **isolated**.
**VPC Traffic Mirroring**
- **Copies network traffic** for analysis.
- Used for **security monitoring & troubleshooting**.
- Example: Mirror **TCP/UDP traffic** to security appliances for **deep packet inspection**.
**Exam Tip**: Reachability Analyzer is for **troubleshooting** it tells you why two resources can't talk. Network Access Analyzer is for **compliance** it tells you if resources are unintentionally reachable.