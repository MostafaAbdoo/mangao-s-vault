## **AWS STS (Security Token Service)**
- AWS Security Token Service (STS) creates **temporary, limited-privilege credentials.**
- Credentials are **short-term and configured with an expiration period.**
- Use cases include identity federation and IAM roles for cross/same account and EC2 access.
## **Amazon Cognito**
- Amazon Cognito **manages user identity for web and mobile applications**, potentially for millions of users.
- Instead of creating IAM users, you create users in Cognito.
## **Microsoft Active Directory (AD)**
- Active Directory Domain Services (AD DS) is found on any Windows Server with AD DS installed.
- It serves as a **database of objects such as user accounts, computers, printers, file shares, and security groups.**
- AD DS enables centralized security management, allowing you to create accounts and assign permissions.
## **AWS Directory Services**
- **AWS Managed Microsoft AD**
    - Create your own AD in AWS, manage users locally, supports MFAEstablish “trust” connections with your on- premise AD
- **AD Connector**
    - Directory Gateway (proxy) to redirect to on- premise AD, supports MFAUsers are managed on the on-premise AD
- **Simple AD**
    - AD-compatible managed directory on AWS cannot be joined with on-premise AD
## **AWS IAM Identity Center**
- AWS Single Sign-On (SSO) provides **one login for all your AWS accounts in AWS Organizations.**
- It also offers single sign-on for business cloud applications like Salesforce, Box, and Microsoft 365.
- SAML 2.0-enabled applications are supported for single sign-on.
- You can use AWS SSO for logging into EC2 Windows Instances.
- Identity providers, including the built-in identity store in IAM Identity Center, are supported.
- Third-party identity providers like Active Directory (AD), OneLogin, and Okta can also be integrated.
## **Amazon WorkSpaces**
- Managed Desktop as a Service (DaaS) solution.
- Easily provision Windows or Linux desktops.
- Eliminates **the management of on-premise VDI (Virtual Desktop Infrastructure).**
- Fast and scalable to thousands of users.
- Secures data and integrates with KMS (Key Management Service).
- Pay-as-you-go service with monthly or hourly rates.
## **Amazon AppStream 2.0**
- Desktop application streaming service.
- **Deliver applications to any computer without acquiring or provisioning infrastructure.**
    - Applications are delivered from within a web browser.
## AWS IoT Core
AWS IoT Core: Connect IoT devices to AWS Cloud.
- **IoT: Internet of Things, network of internet-connected devices.**
- Serverless, secure, and scalable to billions of devices.
- Applications can communicate with devices even when offline.
- Integrates with various AWS services (Lambda, S3, SageMaker).
- Build IoT applications for data gathering, processing, analysis, and action.
## **AWS AppSync**
AWS AppSync: Store and sync data across mobile and web apps in real-time.
- Uses GraphQL (mobile technology from Facebook).
- Client code can be automatically generated.
- Integrates with DynamoDB and Lambda.
- Real-time subscriptions for data updates.
- Offline data synchronization, replacing Cognito Sync.
- Fine-grained security controls.
- AWS Amplify can utilize AWS AppSync in the background.
## **AWS Amplify**
- Comprehensive **toolkit for building and launching scalable web and mobile apps.**
- Includes authentication, storage, API (REST, GraphQL), CI/CD, PubSub, analytics, AI/ML predictions, monitoring.
- Integrates with AWS, GitHub, and other sources for seamless development and deployment.
## **AWS Infrastructure Composer**
- Design and build new modern applications. Streamline the composition of modern applications from multiple AWS services.
- Manage applications through IaC. Use the right tool for the task, including visual, code, or generative AI powered code suggestions in your IDE.
- Visualize your application architecture and configuration Quickly understand your application architecture and keep it current as you collaborate with your team.
- Orchestrate Step Functions workflows Integrate with Workflow Studio to visually orchestrate over 220 AWS services or public http endpoints with Step Functions workflows.
## **AWS Device Farm**
- Fully-managed service that **tests your web and mobile apps against desktop browsers, real mobile devices, and tablets**
- Run tests concurrently on multiple devices (speed up execution)Ability to configure device settings (GPS, language, Wi-Fi, Bluetooth, …)
## **AWS Backup**
- Fully managed service for centralized backup management and automation across AWS services.
- Offers **on-demand and scheduled backups for flexibility.**
- Supports Point-in-Time Recovery (PITR) for precise restoration.
- Features retention periods, lifecycle management, and customizable backup policies.
- Facilitates cross-region backup for enhanced data redundancy and disaster recovery.
- Enables cross-account backup using AWS Organizations for streamlined data protection across multiple accounts.
## **Disaster Recovery Strategies**

1. Backup and Restore - Allows for regular backups of data and applications to prevent data loss. - Enables quick restoration of data and applications in case of failure or disaster. - Ensures business continuity and data integrity.
    
2. Pilot Light - Maintains a minimal version of the infrastructure (e.g., database, essential services) in a standby state. - Enables rapid scaling up of infrastructure to full capacity in case of a disaster. - Helps reduce costs compared to maintaining a fully active backup infrastructure.
    
3. Warm Standby Cost - Involves maintaining a partially active secondary site with scaled-down infrastructure. - Incurs higher costs compared to pilot light due to the partially active state. - Provides faster recovery times and greater capacity compared to pilot light.

4. Multi-Site/Hot Site** - Involves maintaining a fully redundant and active secondary site in a different geographic location. - Offers the highest level of resilience and availability. - Incurs higher costs but provides the quickest recovery times and minimal downtime in case of a disaster.
## **AWS Elastic Disaster Recovery (DRS)**
- _Used to be named “CloudEndure Disaster Recovery”_
- Enables **rapid recovery of servers into AWS in case of data loss or disaster.**
- Provides protection for critical databases and enterprise applications. Uses continuous block-level replication for real-time data protection.
## **AWS DataSync**
- Facilitates the transfer of **large amounts of data from on-premises to AWS.**
- Supports synchronization with various AWS storage services like S3, EFS, and FSx for Windows.
- Offers scheduled replication tasks, with **incremental updates** after the initial full load.
## **AWS Application Discovery Service**
- Helps plan migration projects **by collecting data from on-premises data centers.**
    - **Agentless discovery** → VM inventory, configuration, and performance history.
    - **Agent-based discovery** → System configuration, performance, running processes, and network connection details. Resulting data is viewable in AWS Migration Hub.
- Resulting data can be viewed within **AWS Migration Hub**
## **AWS Application Migration Service (MGN)**
- **Lift-and-shift (rehost) solution which simplifies migrating applications to AWS**
- Converts your physical, virtual, and cloud-based servers to run natively on AWS
- Supports wide range of platforms, Operating Systems, and databases
- Minimal downtime, reduced costs
## **AWS Migration Evaluator**
- Assists in creating a **data-driven business case for migrating to AWS.**
- Offers a detailed view of the current on-premises environment. Installs an Agentless Collector for broad-based discovery, capturing on-premises footprint and server dependencies.
- Analyzes the current state, defines the target state, and helps develop a migration plan.
## **AWS Migration Hub**
- Acts as a **central repository for collecting servers and applications inventory data for assessing, planning, and tracking migrations to AWS.**
- Accelerates migration to AWS and automates lift-and-shift processes.
- **AWS Migration Hub** → ****Provides pre-built templates through **AWS Migration Hub** Orchestrator
    - Saving time and effort for migrating enterprise applications such as SAP and Microsoft SQL Server.
    - Supports migration status updates from Application Migration Service (MGN)
## **AWS Step Functions**
- Allows you to build **serverless visual workflows to orchestrate Lambda functions.**
- Features include sequence, parallel execution, conditions, timeouts, and error handling.
- Integrates with various AWS services such as EC2, ECS, on-premises servers, API Gateway, and SQS queues.
    - Can implement human approval features.
- Use cases include order fulfillment, data processing, web applications, and any workflow automation needs.