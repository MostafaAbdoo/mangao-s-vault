**Reliability** is the probability that a system performs its intended function for a specified period across hardware, firmware, and software.
**Two common measures:**
1. **MTBF (Mean Time Between Failures)** = Total time in service ÷ Number of failures
2. **Failure Rate** = Number of failures ÷ Total time in service

**Reliability**: How long a resource performs correctly without failure (MTBF, Failure Rate)
**Availability**: What percentage of time the resource is operational
Availability = Normal Operation Time ÷ Total Time

**What is high availability?**
High availability (HA) means your application stays operational and accessible with minimal downtime ideally without any human intervention needed to recover. **Fault Tolerance**, **Scalability**, **Recoverability**.
![[Pasted image 20261009183734.png]]
**Exam tip:** On the SAA-C03, HA questions almost always involve deploying across **multiple Availability Zones** (not just multiple instances in one AZ). Deploying 3 EC2 instances in a single AZ is _not_ high availability.
#### Amazon EC2 Auto Scaling
designed to automatically launch or terminate EC2 instances to ensure you have the exact amount of compute capacity needed to handle your application's current workload.
**Scaling Triggers:**
1. **Health checks** – Replaces unhealthy instances.
2. **User-defined policies** – Based on:
    - **CloudWatch metrics**
    - **Scheduled scaling**
    - **Programmatic triggers**
    - **Manual adjustments** (set-desired-capacity)
In order to use EC2 Auto Scaling you need to use another feature of EC2 to help Auto Scaling with the needed configuration for the EC2 instances
**Launch Template**: defines the configuration for instances that the ASG will launch. It specifies:
- AMI (Amazon Machine Image)
- Instance type
- Security group(s)
- Key pair
- Storage (EBS volumes)
- IAM role
- User data (bootstrap scripts)

**Amazon EC2 Auto Scaling Group**
- Logical group of EC2 instances.
- Automatically scale between:
    - Minimum
    - Desired (optional)
    - Maximum
- Integration with Elastic Load Balancing (optional).
- Health checks to maintain group size.
- Distribute and balance instances across Availability Zones.
---
**Dynamic scaling**
Scales the capacity of your Auto Scaling group as traffic changes occur.
- **Target tracking scaling** increase and decrease the current capacity of the group based on a Amazon CloudWatch metric and a target value.
- **Step scaling** Increase and decrease the current capacity of the group based on a set of scaling adjustments, known as _step adjustments_, that vary based on the size of the alarm breach.
- **Simple scaling** increase and decrease the current capacity of the group based on a single scaling adjustment, with a cooldown period between each scaling activity.

**Scheduled Scaling for EC2 Auto Scaling**
- Automates capacity adjustments based on predictable load patterns.
- Uses scheduled actions to modify desired instance count at set times.
	**Example:**
	- **Increase capacity:** Wednesday morning (higher midweek traffic).
	- **Decrease capacity:** Friday evening (lower weekend traffic).

**Predictive Scaling for EC2 Auto Scaling**
- Uses historical data to forecast traffic and adjust capacity proactively.

**Cooldown Period** (simple scaling)
- Prevents excessive scaling by waiting before initiating new scaling activities.
- Allows the Auto Scaling group to stabilize after launching or terminating instances.

**Instance Warmup** (step scaling)
- Defines the time for a new instance to be fully operational.
- During warmup, the instance is excluded from aggregated EC2 metrics.
- Scaling policies only trigger if active (non-warming) instances exceed the alarm threshold.
- Prevents excessive scaling by counting warming instances in desired capacity calculations.

**Default value**
If no value is set, then the scaling policy will use the default value, which is the value for the default instance warmup defined for the group. If the default instance warmup is null, then it falls back to the value of the default cooldown.

**Amazon EC2 Auto Scaling termination policy**
- Determines which instance is terminated when scaling in
- Determines termination order based on these factors:
    - Availability Zone with largest number of instances
    - Multiple policies (the policies will be executed in the order they are listed)
![[Pasted image 20261009192507.png|601]]
**Exam tip:** The exam sometimes describes a scale-in scenario and asks which instance gets terminated. Remember: ASG first balances across AZs, _then_ applies the termination policy within the over-provisioned AZ

**Instance Refresh**
Instance Refresh lets you rolling-replace instances in your ASG for example, after updating your Launch Template (new AMI, new user data, etc.).
- You set a **minimum healthy percentage** (e.g., 80%) ASG ensures that percentage of instances remain healthy during the refresh.
- Instances are replaced in batches; the refresh pauses if health checks fail.
- No need to manually terminate instances or create a new ASG.
> **Example:** You updated your AMI to apply a security patch. Trigger an Instance Refresh with 80% min-healthy ASG replaces instances in waves while keeping at least 80% running at all times.

> **Exam tip:** Instance Refresh is the recommended way to deploy a new AMI to an existing ASG without downtime. If the exam asks how to update instances in an ASG to use a new launch template version, the answer is **Instance Refresh**.

#### Elastic Load Balancing
automatically distributes your incoming traffic across multiple targets. It monitors the health of its registered targets, and routes traffic only to the healthy targets, scales your load balancer capacity automatically in response to changes in incoming traffic.
**Types of Load Balancers:**
1. **Application Load Balancer (ALB)** – Layer 7, routes based on HTTP/S rules.
2. **Network Load Balancer (NLB)** – Layer 4, handles high-performance TCP/UDP traffic.
3. **Gateway Load Balancer (GLB)** – Routes traffic through **third-party appliances**.
**ALB, NLB, and GLB** use **target groups** to manage targets.

**Cross-zone load balancing**: controls whether a load balancer node distributes traffic to targets in all enabled AZs, or only to targets in its own AZ.

**Enabled:** Traffic is spread evenly across all targets in all AZs.
→ Example: 2 targets in Zone A, 8 in Zone B → each target receives **10% of traffic**.

**Disabled:** Each LB node routes requests only to targets in its own AZ.
→ Example: Zone A node splits 50% between its 2 targets (25% each); Zone B node splits 50% between its 8 targets (6.25% each). Zone A targets are overwhelmed.
![[Pasted image 20261009195105.png]]
**Exam tip:** The SAA-C03 frequently tests the cross-zone defaults. Remember: ALB always distributes evenly across AZs at no extra cost. For NLB, enabling cross-zone load balancing incurs inter-AZ data transfer charges.
**Exam tip:** The NLB is the only load balancer that supports **static IP addresses** per AZ and can be used as a target for another load balancer (ALB behind NLB pattern). Use NLB when you need a fixed IP for whitelisting.

**How ELB Routes Traffic**
- **Listeners**: Define how the ELB handles inbound connections by specifying **ports** and **protocols**. Listeners route requests based on rules and are linked to **target groups**.
- **Target Groups**: Collections of **EC2 instances** or other resources that receive traffic from the ELB. Multiple target groups allow routing different types of traffic efficiently.
- **Rules**: Define **routing decisions** based on conditions like IP range, request type, or path. Rules are **prioritized** to ensure logical processing.
#### ELB Configuration & Features
**Sticky Sessions (Session Affinity)**
By default, a load balancer routes each request independently to any healthy target. **Sticky sessions** bind a user's session to a specific target for the duration of their session.
**How it works:** ELB uses a cookie to identify the user. All subsequent requests from that user go to the same target.
- **ALB sticky sessions:** Uses a cookie called `AWSALB`. Duration is configurable (1 second to 7 days).
- **NLB sticky sessions:** Based on source IP (no cookie needed). 5-tuple hash (source IP, source port, destination IP, destination port, protocol).
**When to use it:** Stateful applications that store session data locally on the instance (e.g., a shopping cart cached in memory).
**Anti-pattern:** Don't rely on sticky sessions as a substitute for proper session management. Prefer storing session state in ElastiCache or DynamoDB so any instance can serve any user.
**Exam tip:** If a question describes users losing their session data when they hit a different server, the fix could be sticky sessions _or_ offloading session state to ElastiCache. The better long-term answer is ElastiCache, but sticky sessions is the simpler/faster fix.

**Connection Draining (Deregistration Delay)**
When you deregister a target (or it becomes unhealthy), ELB needs time to complete in-flight requests before stopping traffic to that instance.
- **Connection Draining** is the term used for Classic Load Balancers.
- **Deregistration Delay** is the term used for ALB, NLB, and GWLB.
**Default:** 300 seconds. Configurable from 0 to 3600 seconds.
During the delay period:
- ELB stops sending _new_ requests to the deregistering target.
- _In-flight_ requests are allowed to complete.
- After the delay expires, remaining connections are forcibly closed.

**SSL Server Certificates**
To serve HTTPS traffic, your load balancer needs an SSL/TLS certificate to decrypt requests before forwarding them to targets.
- **TLS is the modern successor to SSL** — the terms are used interchangeably, but TLS (1.2, 1.3) is what's actually in use today.
- ELB uses **X.509 certificates** from a trusted Certificate Authority (CA).
- **AWS Certificate Manager (ACM)** is the recommended way to provision and manage certificates. ACM certificates are free when used with ELB.
**Certificate options:**
- ACM-issued certificate (recommended)
- Imported third-party certificate (uploaded to ACM or IAM)
**SNI (Server Name Indication):** ALB and NLB support multiple SSL certificates on a single listener using SNI. This lets one load balancer serve multiple HTTPS-enabled domains. Classic Load Balancers do _not_ support SNI.
Note: When u use ACM on ALB and NLB u need to use two; one from ELB to user and another from ELB to EC2. But in case of NLB u can use 1 certificate if u r gonna use a third party issuer
> **Exam tip:** If a question asks how to host multiple HTTPS domains on a single load balancer, the answer is ALB or NLB with **SNI**. If a question asks about free SSL certificate management, the answer is **ACM**.

#### Application Load Balancer
The **AWS Application Load Balancer (ALB)** operates at **Layer 7 (Application Layer)** of the OSI model. It's the right choice when you need intelligent HTTP/HTTPS routing based on request content think microservices, container-based apps, and modern web architectures.

- **Path-Based Routing**
	Routes requests to different target groups based on the URL path.
	_Example:_ Requests to `/api/*` go to your API servers; requests to `/static/*` go to your CDN-backed servers all behind a single ALB.
- **Host-Based Routing**
	Routes traffic based on the `Host` header in the HTTP request.
	_Example:_ `api.example.com` → API target group. `www.example.com` → web app target group. Both share the same ALB.
- **Lambda Functions as Targets**
	ALB can invoke Lambda functions directly as a target type. This enables fully serverless HTTP backends without any EC2 instances.
	_Example:_ A lightweight REST API served by Lambda, load balanced and protected by ALB's WAF integration.
- **Dynamic Port Mapping**
	ALB assigns unique ports to containerized tasks on the same host. Essential for ECS where multiple tasks share an EC2 instance but each needs its own port.
- **Protocol Support**
	ALB handles HTTP, HTTPS, HTTP/2, WebSocket, and gRPC covering both traditional web apps and modern real-time/streaming use cases.
- **Deletion Protection**
	Prevents accidental deletion of the ALB. Enable it on production load balancers.
- **Request Tracing**
	ALB injects an `X-Amzn-Trace-Id` header into each request. Useful for end-to-end debugging across microservices.
- **Enhanced Metrics and Access Logs**
	ALB provides detailed CloudWatch metrics (latency, request counts, error rates) and access logs stored in S3 for auditing.
- **Health Checks**
	ALB performs health checks per target group. You configure the path (e.g., `/health`), port, and expected HTTP response codes. Unhealthy targets are automatically removed from rotation.

> **Exam tips — ALB**
> - ALB is the **only** load balancer that can route to **Lambda functions** as a target.
> - ALB supports **SNI** — multiple SSL certificates on one listener for multiple domains.
> - ALB **cannot** be assigned a static P address (use NLB for that). If you need a fixed IP _and_ Layer 7 features, put NLB in front of ALB.
> - Sticky sessions on ALB use the `AWSALB` cookie. Duration: 1 second to 7 days.
> - ALB integrates with **AWS WAF** for Layer 7 protection (NLB does not).

#### Network Load Balancer (NLB)
The **AWS Network Load Balancer (NLB)** operates at **Layer 4 (Transport Layer)** of the OSI model. It's built for extreme performance handling millions of requests per second with ultra-low latency. Choose NLB when you need raw TCP/UDP throughput, static IP addresses, or client IP preservation.

- **Protocol Support**
    - Handles **TCP, UDP, and TLS** traffic. Ideal for real-time apps (gaming, financial trading, VoIP, IoT).
    - **TLS Termination**: NLB offloads TLS decryption, reducing CPU load on backend servers.
- **Static IP per AZ**
    - NLB assigns one static IP address per Availability Zone or you can bring your own **Elastic IP**.
    - This makes NLB the right choice when clients need a fixed, white listable IP address.
    - ALB does _not_ support static IPs; only NLB does.
- **Client IP Preservation**
    - NLB forwards the client’s real IP address to backend targets without a proxy layer.
    - Critical for apps that need IP-based filtering, logging, or compliance.
- **Extreme Performance**
    - Handles millions of requests per second with single-digit millisecond latency.
    - Auto-scales transparently no pre-warming needed.
- **Cross-Zone Load Balancing**
    - **Disabled by default.** When enabled, inter-AZ data transfer charges apply.
    - Compare: ALB always has cross-zone enabled at no extra cost.
- **Sticky Sessions**
    - NLB uses **5-tuple hash** (source IP, source port, dest IP, dest port, protocol) for session stickiness.
    - No cookie is involved stickiness is purely network-based.
- **PrivateLink Integration**
    - NLB is the entry point for **AWS PrivateLink**, enabling private service exposure to other VPCs or accounts without traffic traversing the public internet.
- **Health Checks**
    - Supports TCP, HTTP, and HTTPS health checks.
    - Routes traffic only to healthy targets.
- **Monitoring**
	- CloudWatch metrics: active connections, new connections, processed bytes.
	- VPC Flow Logs for detailed traffic analysis.
> **Exam tips NLB**
> - NLB is the **only** load balancer with a static IP per AZ. ALB uses a DNS name and changes IPs.
> - NLB can be a **target of another load balancer** (an ALB registered as an NLB target group).
> - Cross-zone load balancing is **OFF by default** on NLB; enabling it incurs data transfer charges.
> - NLB does **not** integrate with AWS WAF. For WAF, use ALB.
> - NLB preserves **client source IP** backend security groups must allow the client's IP range, not the NLB's.

#### Gateway Load Balancer (GWLB)
The **AWS Gateway Load Balancer (GWLB)** is purpose-built for one job: transparently routing traffic through **third-party virtual appliances** (firewalls, IDS/IPS, deep packet inspection) before it reaches your applications. It operates at **Layer 3 (Network Layer)** and uses the **GENEVE protocol on port 6081** to encapsulate traffic as it passes through appliances.
- **Transparent Inspection** Traffic flows through appliances without the application knowing. No IP address or routing changes required on the application side.
- **GENEVE Encapsulation** GWLB uses GENEVE (port 6081) to preserve original packet context while forwarding to appliances.
- **Elastic Scaling** Scales the appliance fleet horizontally as traffic grows.
- **High Availability** Distributes traffic across multiple appliance instances in multiple AZs. Automatically reroutes around unhealthy appliances.
- **Gateway Load Balancer Endpoints (GWLBe)** PrivateLink-based endpoints in the consumer VPC that intercept traffic and forward it to the GWLB in the security service VPC.
- **Health Checks & Monitoring** Integrates with CloudWatch; routes only to healthy appliance instances.
**When to use Gateway Load Balancer**
- **Network Security** Run all VPC traffic through a firewall or IDS/IPS before it reaches your instances.
- **Compliance** Regulatory requirements mandate traffic inspection (e.g., PCI-DSS, HIPAA).
- **Centralized Security** One security service VPC inspects traffic for multiple consumer VPCs via GWLB endpoints.
- **Third-party appliances** You use a marketplace security product (Palo Alto, Fortinet, Check Point) and need to scale it without architectural changes.
![[Pasted image 20261010075521.png|574]]
> **Exam tips GWLB**
> - GWLB is the answer when you need to insert a **third-party security appliance** transparently into the network path.
> - It uses **GENEVE protocol on port 6081** this detail can appear in protocol-matching questions.
> - GWLB operates at **Layer 3**, unlike ALB (Layer 7) and NLB (Layer 4).
> - Traffic flow: Internet Gateway → **GWLB Endpoint** (in consumer VPC) → **GWLB** (in security VPC) → Appliance → back to GWLB → back to consumer VPC → EC2.

