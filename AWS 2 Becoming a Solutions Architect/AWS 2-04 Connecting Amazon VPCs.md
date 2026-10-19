In this the coming sections we introduce the different ways to connect to and from Amazon VPCs
- **VPC Peering** Private, direct connection between two VPCs (same or different accounts/regions).
- **AWS Direct Connect** Dedicated private line from your data center to AWS. Best for consistent, high-bandwidth workloads.
- **AWS VPN** Encrypted connection over the public internet. Two types: Site-to-Site (branch office to AWS) and Client VPN (remote users).
- **AWS Transit Gateway** Hub-and-spoke model to connect many VPCs and on-premises networks through a single gateway.
**SAA-C03 Exam Tip:** The exam frequently asks you to choose the _right_ connectivity option for a given scenario. Focus on the trade-offs: VPC Peering (simple, no transitive routing) vs. Transit Gateway (scalable, supports transitive routing) vs. Direct Connect (dedicated, consistent performance) vs. VPN (quick to set up, goes over public internet)
#### VPC Peering
A VPC peering connection is a networking connection between two VPCs that lets you route traffic between them privately.
- Each VPC supports up to **125 active** and **125 pending** peering connections.
- Peering is **one-to-one** you must create an explicit connection for every pair of VPCs that need to communicate.
- The requester initiates the connection; the accepter has **7 days** to accept before it expires.
- Both requester and accepter must update their **subnet route tables** to point traffic toward the peering connection.
**Non-valid peering configuration**:
- **Overlapping CIDR Blocks** – VPC peering cannot be established if IPv4 CIDR ranges overlap, even if only IPv6 is used.
- **No Transitive Peering** – Traffic cannot pass through an intermediary VPC. Each direct connection must be explicitly defined.
- **No Edge-to-Edge Routing** – Peering does not extend to VPNs, Direct Connect, internet gateways, NAT devices, or VPC endpoints.
- **Solution for Scaling** – AWS Transit Gateway is a better alternative for large-scale, multi-VPC connectivity without these limitations.
**Same-region peering traffic is not encrypted** a fact the exam can use in security scenarios.

#### Direct Connect
- **Direct Connect** is an AWS service that establishes a dedicated network connection between your premises and AWS(VPC). You can create this private connectivity to reduce network costs, increase bandwidth, and provide more consistent network experience compared to regular internet-based connections.
- You need to setup a **Virtual Private Gateway** on your VPC
- Use Cases:
	• Increase bandwidth throughput - working with large data sets – lower cost
	• More consistent network experience - applications using real-time data feeds
	• Hybrid Environments (on prem + cloud)
**Connection Types**:
![[Pasted image 20260915063240.png]]
Data in transit is not encrypted but is private -> **AWS Direct Connect + VPN** provides an IPsec-encrypted private connection
In case Direct Connect fails, you can set up a **backup Direct Connect** connection (expensive), or a **Site-to-Site VPN** connection
**Direct Connect Gateway**: If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway
#### AWS VPN Services
- **AWS Site-to-Site VPN** enables you to securely connect your on-premises network to Amazon VPC, for example your branch office site.
- **AWS Client VPN** enables you to securely connect users to AWS or on-premises networks, for example remote employees.
##### AWS Site-to-Site VPN
Based on IPsec technology, AWS Site-to-Site VPN uses a VPN tunnel to pass data from the customer network to or from AWS.
One AWS Site-to-Site VPN connection consists of two tunnels. Each tunnel terminates in a different Availability Zone on the AWS side, but it must terminate on the same customer gateway on the customer side.

![img](https://d3q5r8dtv7rjiz.cloudfront.net/learning_path_images/1782046559-3752B47484B412E0.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9kM3E1cjhkdHY3cmppei5jbG91ZGZyb250Lm5ldC9sZWFybmluZ19wYXRoX2ltYWdlcy8xNzgyMDQ2NTU5LTM3NTJCNDc0ODRCNDEyRTAucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzg5NTE5ODYzfX19XX0_&Signature=hGP2chrsXEcHul9BrWqEMuPtVZ2vi~qQ35OC~kLD-fLQ3rnQkEIpd6NHsvllX7l-XvCzW8nhGcMpz3Vj4~RRYVVBrnkiQ1F56~EYpNj4adia8q8Ds6XymFSx5BH02B3tZEjLs4wRfpgf0E9DGviQzFi9hXz6j5R8mzK4T9IRnwJ6YwfFx6eSCulwRi0UcNb~VKMGWSYpJsqI3HIs9VAXqZnwyYnKHDH~qO85oBERIgsQlpB3o7hEA3ipOFTwOq5GfenT0cLGQQc3NlUiWTWMePUHSDlWX4mevIelnv3VQyVRgAtttLW~mPr7ZgY3RV1oZyo7~SCdbHBArMMEjMSwMg__&Key-Pair-Id=K3HIRDOJCEHHR3)

**Customer gateway**
A resource you create and configure in AWS that represents your on-premise gateway device. The resource contains information about the type of routing used by the Site-to-Site VPN, BGP, ASN and other optional configuration information.
**Customer gateway device**
A customer gateway device is a physical device or software application on your side of the AWS Site-to-Site VPN connection
- If it’s behind a NAT device that’s enabled for NAT traversal (NAT-T), use the public IP address of the NAT device
**Virtual private gateway**
A virtual private gateway is the VPN concentrator on the Amazon side of the AWS Site-to-Site VPN connection. You use a virtual private gateway or a transit gateway as the gateway for the Amazon side of the AWS Site-to-Site VPN connection.
-  **Important step**: enable **Route Propagation** for the Virtual Private Gateway in the route table that is associated with your subnets
  - **Route Propagation** is an automated AWS VPC feature that automatically injects (propagates) routes from a Virtual Private Gateway (VGW) directly into your VPC route tables.
**AWS VPN CloudHub**
Provide secure communication between multiple sites, if you have multiple VPN connections. To set it up, connect multiple VPN connections on the same VGW, setup dynamic routing and configure route tables
**Transit gateway**
A transit gateway is a transit hub that can be used to interconnect your VPCs and on-premises networks. You use a transit gateway or virtual private gateway as the gateway for the Amazon side of the AWS Site-to-Site VPN connection.
**AWS Site-to-Site VPN limitations**
- **IPv6 on Virtual Private Gateway** IPv6 traffic is not supported for VPN connections on a Virtual Private Gateway. However, IPv6 **is** supported when the VPN terminates on a **Transit Gateway**.
- **No Path MTU Discovery** AWS VPN connections do not support Path MTU Discovery.
- **Throughput cap** Each VPN tunnel supports a maximum of **1.25 Gbps**. Since each connection has two tunnels, effective throughput can reach 2.5 Gbps with ECMP (requires Transit Gateway).
- **Use non-overlapping CIDRs** — When connecting multiple VPCs to a shared on-premises network, CIDR overlap causes routing failures
**Accelerated VPN:** Uses AWS Global Accelerator to route traffic from your on-premises location to the nearest AWS edge location, then over the AWS private backbone to your VPC. This reduces jitter and improves throughput for latency-sensitive workloads.

#### Client VPN
Based on OpenVPN technology, Client VPN is a managed client-based VPN service that lets you securely access your AWS resources and resources in your on-premises network.
**Client VPC endpoint** Your Client VPN administrator creates and configures a Client VPN endpoint in AWS. Your administrator controls which networks and resources you can access when you establish a VPN connection.
**VPN client application**
This is the software application that you use to connect to the Client VPN endpoint and establish a secure VPN connection.
**Client VPN endpoint configuration file**
This is a configuration file that is provided to you by your Client VPN administrator. The file includes information about the Client VPN endpoint and the certificates required to establish a VPN connection. You load this file into your chosen VPN client application.
**Client VPN monitoring**
- End-user usage reporting is possible through Amazon CloudWatch Logs.
- You can use a client connect handler to do basic posture assessment with Lambda because Client VPN does not support native posture assessment.
- Client VPN publishes metrics to CloudWatch for your Client VPN endpoints. Metrics are published to CloudWatch every five minutes.
Client VPN pricing is based on two factors: the number of active client connections per hour and the number of subnets associated with the VPN endpoint per hour.
#### SAA-C03 Exam Tips
- **VPN vs. Direct Connect:** VPN is fast to set up (minutes to hours), lower cost, but goes over the public internet. Direct Connect takes weeks to provision but delivers consistent throughput and doesn’t traverse the internet. For compliance or latency-sensitive use cases, Direct Connect is the right answer.
- **Two tunnels = HA.** Every Site-to-Site VPN connection comes with two tunnels terminating in different AZs on the AWS side. Both should be active to avoid single points of failure.
- **VPN as a Direct Connect backup** is a common design pattern on the exam. Use BGP route preferences to make Direct Connect the primary path.
    - BGP is the standard routing protocol used to exchange routing information between different networks across the internet.
- **Throughput limit:** Each VPN tunnel maxes out at **1.25 Gbps**. If you need more, use ECMP over multiple VPN connections attached to a **Transit Gateway**.(ECMP = Equal-cost multi-path routing)
    - ECMP is a networking feature that allows a router to load-balance traffic across **multiple parallel network paths** that have the exact same cost/speed. Instead of sending 100% of your data down a single lane, ECMP opens multiple lanes simultaneously and splits traffic evenly across them. **ex**) 4 * 1.25 = 5 Gbps
- **Client VPN** is based on OpenVPN and is for remote users, not branch offices. Site-to-Site VPN is for fixed network-to-network connectivity

#### Transit Gateway
For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection
- A Transit Gateway is deployed within a specific AWS Region. However, you can peer two Transit Gateways located in different regions.
- Instead of creating separate gateways in every AWS account, you create one Transit Gateway in a central Networking/Management account. You then share it across multiple accounts in your AWS Organization using **AWS RAM**.
- **Route Tables**: limit which VPC can talk with other VPC
- Works with Direct Connect Gateway, VPN connections
- Supports IP Multicast (not supported by any other AWS service)
- **Attachment limits** A single Transit Gateway supports up to **5,000 attachments** (VPCs, VPN connections, Direct Connect gateways, and peering connections combined).
![[Pasted image 20260915065213.png]]
AWS Transit Gateway supports an MTU of 8,500 bytes for everything except VPN connections -> 1,500 bytes
##### **Route propagation**
route propagation determines how the TGW's central route table automatically learns network paths from connected resources.
- **VPC attachments**: The VPC's CIDR is automatically propagated to the Transit Gateway route table. However, you must also add a route to the VPC's **subnet route tables** pointing traffic destined for other VPCs toward the Transit Gateway attachment.
- **VPN and Direct Connect gateway attachments** Routes are propagated from the Transit Gateway to your on-premises router using BGP.
- **Peering attachments** Static routes must be manually added to the Transit Gateway route table (on both sides) pointing to the peering attachment.
![[Pasted image 20260915071921.png|464]]
## VPC Design Patterns
**Challenges with VPC Peering & VPN Mesh**:
**Complex Routing** – Hard to manage as the network grows.
**Operational Overhead** – No central control over traffic flow.
**Increased Downtime** – Misconfigurations cause service disruptions.

**Solution: AWS Transit Gateway (Hub-and-Spoke Model)**:
**Centralized Management** – Simplifies routing by consolidating VPC and on-prem connections.
**Improved Scalability** – Supports large networks without exponential growth in connections.
**Reduced Complexity** – Eliminates peer-to-peer mesh, reducing configuration conflicts.

#### Hybrid network
**Highly available hybrid network connections**

AWS guarantees Direct Connect service at **99.99% availability** only when you deploy connections across two separate Direct Connect Partner locations. The guarantee covers from the AWS router inward not your on-premises circuits or partner-side hardware.
A single Direct Connect location with a single router creates two single points of failure: the router itself and the WAN circuit.

**Removing the single points of failure:**
- Add a **second router** at the Direct Connect Partner location, with each WAN circuit terminating on a different router and cross-connected to the AWS router separately.
- Add a **second WAN circuit** at each data center, terminating at the same (or different) Partner location.
![[Pasted image 20260921022154.png]]
#### Cross-regional VPC peering

Deploying your application in multiple Regions enables:
- **Low-latency routing** Route 53 latency-based routing sends users to the nearest Region.
- **Load distribution** Spread traffic across Regions; if one degrades, shift workload to another.
- **Redundancy** If one Region has a service interruption, users connect to an alternate Region.

In this pattern, Amazon Route 53 routes users to the Region with the lowest latency. Databases in each Region stay in sync via an **Inter-Region VPC peering connection**. If a user's session is interrupted and they reconnect to a different Region, they can resume seamlessly because the data is already replicated.
**Why Inter-Region VPC peering?** It keeps replication traffic on the AWS global backbone never over the public internet. This reduces DDoS exposure and keeps latency predictable.![[Pasted image 20260921022518.png]]

----
**SAA-C03 Decision Framework**
**Choose VPC Peering when:**
- You need to connect 2–3 VPCs with simple, direct private connectivity.
- Cost is a concern (free within the same AZ).
- Remember: non-transitive, no overlapping CIDRs.
**Choose Transit Gateway when:**
- You have 4+ VPCs, or need transitive routing between VPCs.
- You need a shared on-premises connection for multiple VPCs.
- You need ECMP for VPN bandwidth scaling.
**Choose Direct Connect when:**
- You need consistent, high-bandwidth, low-latency, or compliance-driven private connectivity.
- Pair with a backup VPN for resilience.
- Use Transit VIF + DX Gateway for multi-VPC/multi-region access.
**Choose VPN when:**
- You need fast setup (minutes to hours) for on-premises connectivity.
- Budget is limited or workloads tolerate variable internet latency.
- Use as a failover path alongside Direct Connect.