- Private Cloud
- Public Cloud
- Hybrid Cloud
### 1️⃣ Infrastructure as a Service (IaaS)
- Offers **basic infrastructure resources** such as:
    - Virtual machines (compute)
    - Storage
    - Networking
- Gives users **maximum flexibility and control**
- You are responsible for managing:
    - Operating systems
    - Applications
    - Runtime environments
**Examples**: Amazon EC2, Azure Virtual Machines, Google Compute Engine
### 2️⃣ Platform as a Service (PaaS)
- Offers a **platform and environment** to build, test, and deploy applications
- Abstracts away infrastructure management
- Developers can focus on **application logic**
**Examples**: AWS Elastic Beanstalk, Azure App Services, Google App Engine
### 3️⃣ Software as a Service (SaaS)
- Fully managed software that is **delivered over the internet**
- Users access it via web or apps
- No infrastructure or platform concerns
**Examples**: Google Workspace, Microsoft 365, Salesforce

## AWS Pricing Models
### Pay as You Go
Pricing is based on actual **resource consumption**
### Save When You Reserve
Ideal for **predictable workloads**, Commit to **1 or 3 years** of usage
### Pay Less by Using More
**Volume-based discounts**
### Pay Less as AWS Grows
Price reductions over time due to improved economies of scale and tech innovation, **operational efficiencies** 

---
## **AWS** **Cloud Adoption Framework (CAF)**
**assists in creating and executing a comprehensive plan for digital transformation using AWS.**
- AWS CAF identifies **key organizational capabilities essential for successful cloud transformations.**
- It organizes these capabilities into six perspectives:
    - **Business, People, Governance, Platform, Security, and Operations.**

### **Business**
Focuses on:
- making sure cloud supports business goals
- ROI (return on investment)
- cost reduction
- faster innovation
**“Why are we moving to cloud?”**
### **People**
Focuses on:
- training staff
- changing company culture
- leadership support
- organizational change
**“Are employees ready for cloud?”**
### **Governance**
Focuses on:
- compliance
- risk management
- financial control
- policies
**“How do we control cloud usage?”**
### **Platform**
Focuses on:
- infrastructure
- networking
- architecture
- migration tools
**“How do we build the cloud environment?”**
### **Security**
Focuses on:
- IAM
- data protection
- threat detection
- incident response
**“How do we secure the cloud?”**
### **Operations**
Focuses on:
- monitoring
- automation
- backups
- performance
- support
**“How do we run cloud daily?”**

### 1. Envision
- goals
- benefits
- business value
### 2. Align
- gaps
- risks
- needed skills
### 3. Launch
- pilot projects
- migrations
### 4. Scale
- optimization
- automation
- continuous improvement
---
## AWS Well-Architected Framework
is a set of **best practices from** AWS for designing cloud systems
Built around six pillars—**operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability**
**“Is my cloud architecture built correctly?”**

### **Best practice for “Identity and Access Management”**
- Control access to your serverless API.
- Control access to your serverless application.

**Controlling access to your serverless API:**
APIs are common attack targets due to the valuable operations and data they expose. To secure access:
- Use **Amazon Cognito** user pools for user authentication.
- Implement **API Gateway Lambda authorizers** to control authorization.
- Apply **API Gateway resource policies** to define who can access the API. Understanding and configuring these mechanisms correctly is essential to secure your API.

**Control access to your serverless application:**
When configuring AWS Lambda, follow **least-privileged access** principles. Grant only the permissions required for each function's operation to avoid unnecessary risks.
Smaller, well-scoped functions are also recommended to enhance security and maintain a well-architected application

## **Best practice for "Data Protection”**

**1. Encrypt Data in Transit and at Rest**
- **API Gateway**: Encrypt sensitive data **client-side** before sending HTTP requests. Avoid exposing sensitive information in request headers.
- **Lambda & API Integrations**: Encrypt data before processing to prevent exposure in logs (**Amazon CloudWatch Logs**) or persistent storage.
- **Storage Services (S3, DynamoDB, OpenSearch)**: Enable encryption at rest. Avoid logging or storing sensitive data in unencrypted form.

**2. Implement Application Security**
- **Input Validation**: Validate inbound events to prevent malicious input.
- **API Gateway Validation**: Use built-in request validation for JSON schema, URL parameters, and headers.
- **Advanced Validation**: Use Lambda functions, libraries, or external services for deeper security checks

## **Best practice for "Failure Management”**

**Use a Dead-Letter Queue (DLQ) Mechanism**
- **AWS Lambda**: Configure a **dead-letter queue (DLQ)** in **Amazon SQS** to capture failed transactions for analysis and retries.
   - Amazon SQS is a **message queue service**. Its job is to let different parts of your system communicate **asynchronously** instead of directly. `App → SQS Queue → Lambda → Database`
- **Amazon Kinesis & DynamoDB Streams**: When a batch processing error occurs, the entire batch is retried, which may block processing. Configure Lambda to isolate "poison-pill" messages by forwarding them to a **DLQ** to prevent system-wide failures.

**Roll Back Failed Transactions**
- **AWS Step Functions**: Use state machines to **automate rollback procedures** when a failure occurs.
- **Transaction Consistency**: Ensure workflows either fully complete or properly revert to maintain data integrity.

## **Best practice for "Cost-effective Resources”**

- **Use AWS Lambda efficiently**: Since Lambda's **CPU, network, and storage IOPS** scale with memory allocation, optimize memory settings to balance cost and performance. > more memory = faster
- **Reduce cold starts**: Optimize function initialization to **lower execution time**, as Lambda charges in **1-ms increments**.
- **Minimize unnecessary function calls**: Use **direct integrations** with AWS services to cut costs.

## **Best practice for "Optimization”**

- **Use Direct AWS Service Integrations**
    - **Avoid unnecessary Lambda functions**: If a function **only forwards data**, replace it with a **direct service integration** (e.g., API Gateway → Kinesis Data Firehose).
    - **Reduce operational overhead**: Leverage built-in **AWS Step Functions, EventBridge, and Lambda Destinations** instead of writing custom logic. > Do not write extra code for tasks AWS can already do for you.
![[Pasted image 20260502033503.png]]
