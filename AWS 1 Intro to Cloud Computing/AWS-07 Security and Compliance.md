# **Identity Access Management (IAM)**
Identity Access Management allows you to specify who or what can access services and resources in AWS, centrally manage fine-grained permissions, and analyze access to refine permissions across AWS.
- **Global service** for managing user identities and access to AWS resources.
- Default root account is created but **shouldn't be utilized or shared for security reasons.**
    - **Users** represent individuals within the organization and **can be organized into groups.**
        - Groups logical grouping of users to simplify permission management.
            - Users are **not** **required to belong to a group** and can **be members of multiple groups simultaneously.**
- **Users or groups, or roles** can be assigned JSON documents known as policies.
- **Policies** define the permissions granted to users.
    📍 AWS follows the **least privilege principle**
## **Identity Access Management: Policies**
- IAM policies are **JSON documents that define permissions for users, groups, or roles.**
- Each policy consists of one or more statements, each with specific access permissions.
- Statements contain elements such as actions (the actions allowed or denied), resources (the AWS resources to which the actions apply), and conditions (optional criteria that must be met for the policy to be applied).
- Policies can be **attached to IAM identities (users, groups, or roles) to grant or restrict access to AWS resources.**
- AWS provides predefined policies for common use cases, and custom policies can be created to tailor permissions according to specific requirements.
## **Identity Access Management: Policy Structure**
- The policy consists of:
    - **Version**: The version of the policy language, always set to "2012-10-17."
    - **Id:** An optional identifier for the policy.
    - **Statement:** One or more individual statements (required).
- Each statement consists of:
    - **Sid:** An optional identifier for the statement.
    - **Effect:** Specifies whether the statement allows or denies access (Allow, Deny).
    - **Principal:** The account, user, or role to which the policy is applied.
    - **Action:** A list of actions that the policy allows or denies.
    - **Resource:** A list of resources to which the actions are applied.
    - **Condition:** Optional conditions for when the policy is in effect.
## **Identity Access Management: Shared Responsibility Model**
- **AWS handles:**
    - Infrastructure (global network security)
    - Configuration and vulnerability analysis
    - Compliance validation
- **You, the customer, handle:**
    - Users, Groups, Roles, Policies management and monitoring
    - Enable MFA on all accounts
    - Rotate all your keys often
    - Use IAM tools to apply appropriate permissions
    - Analyze access patterns & review permissions
## **IAM : Password Policy**
- Setting a **strong password enhances the security of your AWS account.**
- **AWS allows you to establish a password policy, enabling you to:**
    - Specify a minimum password length.
    - Mandate the inclusion of specific character types, such as uppercase letters, lowercase letters, numbers, and non-alphanumeric characters.
    - Permit all IAM users to change their passwords.
    - Enforce password expiration, prompting users to change their passwords after a defined period.
    - Prevent the reuse of passwords to enhance security further.

Multi-Factor Authentication (MFA) adds an **extra layer of security by requiring users to provide both a password (something they know) and a security device (something they own)**, **reducing the risk of unauthorized access, even if passwords are compromised.**

## Access Keys in AWS
- You can access AWS in three ways:
    1. **AWS Management Console (protected by password + MFA)**
    2. **AWS Command Line Interface (CLI):** protected by access keys
    3. **AWS Software Developer Kit (SDK) - for code:** protected by access keys
- Access Keys are generated via the **AWS Management Console.**
- Users are responsible for managing their own access keys.
- Access Keys are treated as secrets and **should not be shared.**
- The Access Key ID serves a similar purpose as a username.
- The Secret Access Key serves a similar purpose as a password.

## AWS Command Line Interface
- Facilitates interaction with AWS services **via commands in your command-line shell.**
- Provides **direct access to the public APIs of AWS services.**
- Enables the development of scripts for resource management.
- Open-source tool available at [**https://github.com/aws/aws-cli](https://github.com/aws/aws-cli).**
- Serves as an alternative to using the AWS Management Console for AWS resource management tasks.

## **AWS Software Development Kit (SDK)**
**The AWS Software Development Kit (AWS SDK) = a collection of language-specific APIs (libraries) that enable you to programmatically access and manage AWS services.**
- It can be embedded within your application and supports:
    - SDKs: For languages like JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, and C++.
    - Mobile SDKs: For platforms like Android and iOS.
    - IoT Device SDKs: For platforms like Embedded C and Arduino.
- An example of the AWS SDK in use is the AWS CLI, which is built on the AWS SDK for Python.
## **IAM Roles for Services**
- Certain AWS services require permission to perform actions on your behalf.
- To grant permissions, **IAM roles are assigned to AWS services.**
- **Common roles include:**
    - EC2 Instance Roles
    - Lambda Function Roles
    - Roles for CloudFormation
## **IAM Security Tools**
- **IAM Credentials Report (Account-Level):** Provides a comprehensive list of **all users in your AWS account along with the status of their various credentials.**
    - Enables tracking and monitoring of IAM users and their associated credentials, including access keys and passwords.
- I**AM Access Advisor (User-Level):** Displays the **service permissions granted to an IAM user.**
    - Shows the timestamp of the user's last access to each service.
    - Helps in reviewing and revising IAM policies based on actual service usage to ensure adherence to the principle of least privilege.
# **Security Tools**
## **AWS Secrets Manager**
- A recently introduced service designed for **storing secrets securely.**
    - Passwords are an example of some of the secrets you may have!
- Allows enforcing rotation of secrets at regular intervals.
- Automates the generation of secrets during rotation, leveraging Lambda functions.
- Integrates seamlessly with **Amazon RDS (MySQL, PostgreSQL, Aurora) for enhanced security.**
- Ensures encryption of secrets using KMS (Key Management Service).
- Primarily tailored for RDS integration, providing robust secret management capabilities.
## **AWS Artifact**
**AWS Artifact =** An online platform providing customers with **instant access to AWS compliance documentation and agreements.**
- **Artifact Reports =**  enable downloading of AWS security and compliance documents from third-party auditors, including ISO certifications, Payment Card Industry (PCI) reports, and System and Organization Control (SOC) reports.
- **Artifact Agreements =**  facilitate reviewing, accepting, and tracking the status of AWS agreements like the Business Associate Addendum (BAA) or Health Insurance Portability and Accountability Act (HIPAA) within individual accounts or organizations.
    - Supports **internal audit or compliance processes by offering comprehensive access to necessary documentation and agreements.**
## **Amazon GuardDuty**
 **Amazon GuardDuty =** An **intelligent threat discovery solution** to safeguard your AWS Account.
- Utilizes Machine Learning algorithms, anomaly detection techniques, and third-party data sources.
- Activation is simplified with a one-click enablement process (30 days trial), eliminating the need for software installation.
- Input data sources encompass various AWS services:
    - **CloudTrail Events Logs:** Detects unusual API calls and unauthorized deployments.
    - **CloudTrail Management Events:** Monitors activities like VPC subnet creation and trail setup.
    - **CloudTrail S3 Data Events:** Tracks actions such as object retrieval, listing, and deletion.
    - **VPC Flow Logs**: Identifies unusual internal traffic patterns and IP addresses.
- Optional features extend coverage to additional services such as EKS Audit Logs, RDS, Aurora, EBS, Lambda, and S3 Data Events.
- Offers integration with **EventBridge** rules for proactive notifications in case of findings, which can be directed to AWS Lambda or SNS.
- Provides protection against **cryptocurrency attacks, with a dedicated "finding" for this threat vector.**
## **Amazon Inspector**
📍 **Amazon Inspector** = Automated Security Assessments streamline security evaluations across various AWS resources.
- **AWS resources you can use Inspector on:**
    - **EC2 Instances:**
        - Utilizes the AWS Systems Manager (SSM) agent for assessment.
        - Analyzes unintended network accessibility and checks for known vulnerabilities in the running OS.
    - **Container Images in Amazon ECR:**
        - Conducts assessments of container images upon push to Amazon Elastic Container Registry (ECR).
    - **Lambda Functions:**
        - Identifies software vulnerabilities in function code and package dependencies.
        - Assesses functions during deployment.
## **AWS Config**
📍 **AWS Config** = facilitates **auditing and maintaining compliance** for AWS resources.
- **You can do the following with Config:**
    - Recording configurations and changes over time.
    - Option to store configuration data in S3 for analysis with Athena.
    - Resolving inquiries such as security group SSH access restrictions and bucket public access.
    - Receiving alerts (SNS notifications) for any changes made.
    - Operating as a per-region service but can be aggregated across regions and accounts for comprehensive analysis.
## **AWS CloudTrail**
- Provides **governance, compliance, and auditing capabilities** for your AWS account.
    - CloudTrail is enabled by default, offering a history of events and API calls made within your AWS account through various interfaces such as the Console, SDK, CLI, and AWS services.
- Logs from CloudTrail can be stored in CloudWatch Logs or S3 for further analysis and monitoring.
- **A trail can be configured to apply to all regions (default) or a single region for more granular control.**
- In case of resource deletion in AWS, CloudTrail should be the first place to investigate to understand the actions taken.
**AWS Config = “What changed?”**  
**AWS CloudTrail = “Who did it?”**

## **AWS Macie**
**AWS Macie** = Amazon Macie is a fully managed data security and privacy service **leveraging machine learning and pattern matching to identify and safeguard sensitive data within AWS.**
- Detects and notifies users about sensitive data, such as personally identifiable information (PII).
**Macie = Protects sensitive data**, “Is sensitive data exposed?”
**GuardDuty = Detects attackers and suspicious behavior**
## **AWS Security Hub**
**Amazon Security Hub** = serves as a centralized security management tool, enabling the oversight of security across multiple AWS accounts and automating security checks.
- Integrated **dashboards provide real-time security and compliance status, facilitating swift actions.**
- Automatically aggregates alerts from various AWS services and partner tools, including:
    - Config, GuardDuty, Inspector, Macie, IAM Access Analyzer, Systems Manager, Firewall Manager, AWS Health, and AWS Partner Network Solutions.
- Initial activation of the AWS Config Service is required to utilize Security Hub's capabilities.
## **AWS Detective**
📍 **AWS Detective** = leverages machine learning and graph analysis to analyze, investigate, and rapidly identify the underlying cause of security concerns or suspicious activities.
- Automatically gathers and processes events from various sources like VPC Flow Logs, CloudTrail, and GuardDuty, creating a comprehensive and unified view.
- Visualizations provided by Amazon Detective offer detailed insights and contextual information, aiding in the identification and resolution of security incidents.
## **IAM Access Analyzer**
- Identify shared **resources outside your defined zone of trust** within AWS by examining:
    - S3 Buckets
    - IAM Roles
    - KMS Keys
    - Lambda Functions and Layers
    - SQS Queues
    - Secrets you store _**inside**_ Secrets Manager
- Establish a zone of trust, typically an AWS Account or AWS Organization, and flag any access to resources outside this zone as findings for further investigation.
## **Root User Privilege**
📍 **Root user =** is the AWS account owner, automatically created during account setup
- Possesses unrestricted access to all AWS services and resources
    - _**Lock away your AWS root user access keys**_
    - _**Don’t use your root account for everyday purposes**_
- **Actions that can be performed only by the root user:**
    - Change account settings (account name, email address, root user password, root user access keys)
    - View certain tax invoices
    - Close your AWS account
    - Restore IAM user permissions
    - Change or cancel your AWS Support plan
    - Register as a seller in the Reserved Instance Marketplace
    - Configure an Amazon S3 bucket to enable MFA
    - Edit or delete an Amazon S3 bucket policy that includes an invalid VPC ID or VPC endpoint ID
    - Sign up for GovCloud
## **AWS Abuse**
📍 **AWS Abuse** = If you suspect AWS resources are being used for abusive or illegal activities, report them promptly to the AWS Abuse team.
- Abusive and prohibited behaviors include spam, port scanning, DoS or DDoS attacks, intrusion attempts, hosting objectionable or copyrighted content, and distributing malware.
# **Cloud Monitoring**
## **AWS CloudWatch Metrics**
📍 **CloudWatch** offers metrics for every service in AWS, providing valuable insights into resource utilization and performance.
- **A metric** is a variable to monitor, such as CPU utilization, network traffic, and more, with each metric having associated timestamps.
- You can create CloudWatch dashboards to visualize and track these metrics over time for effective monitoring and analysis
## **AWS CloudWatch Alarms**
- Alarms serve to initiate notifications based on various metrics.
- **Alarm actions include:**
    - Auto Scaling: adjusting the desired count of EC2 instances.
    - EC2 Actions: halting, terminating, restarting, or restoring an EC2 instance.
    - SNS notifications: dispatching a notification to an SNS topic.
- Several options such as sampling, percentage, maximum, minimum, etc.
- Users can select the evaluation period for an alarm.
- For instance, one can set up a billing alarm on the CloudWatch Billing metric.
- Alarm states include: OK, INSUFFICIENT_DATA, and ALARM.
## **Amazon CloudWatch Logs**
- **CloudWatch Logs can collect log from:**
    - Elastic Beanstalk: collection of logs from application
    - ECS: collection from containers
    - AWS Lambda: collection from function logs
    - CloudTrail based on filter
    - CloudWatch log agents: on EC2 machines or on-premises servers
    - Route53: Log DNS queries
    - Enables real-time monitoring of logs
- Adjustable CloudWatch Logs retention
- **By default, logs from your EC2 instance do not go to CloudWatch.**
    - To send logs to CloudWatch, you need to run a CloudWatch agent on EC2.
    - Ensure correct IAM permissions for the agent.
    - The CloudWatch log agent can also be set up on-premises.
## **Amazon EventBridge**
- AWS EventBridge is a **serverless event bus that helps AWS services and applications communicate using events.**
- It routes events from AWS services, custom applications, and SaaS products to over 90 AWS targets like Lambda, SQS, and SNS.
- The service decouples application components, simplifying the management of event-driven architectures and enhancing flexibility.
- Key use cases include application orchestration and real-time data processing.
- To get started with EventBridge, set up event rules, configure security with IAM roles, and conduct pattern testing.
- Best practices include efficient event filtering and robust error handling in event-driven setups.
## **AWS X-Ray**
- Usually, debugging in production follows this approach:
    - Test locally.
    - Add log statements throughout the code.
    - Re-deploy in production.
    - Log formats vary across applications, making log analysis challenging.
    - Debugging is relatively straightforward for monolithic applications but complex for distributed services.
    - Lack of common views of the entire architecture hampers understanding.
**X-Ray is an AWS service that provides a visual analysis of our applications**
- **What it can do for you and your AWS resources:**
    - Troubleshooting performance (bottlenecks)
    - Understand dependencies in a microservice architecture
    - Pinpoint service issues
    - Review request behavior
    - Find errors and exceptions
        - Are we meeting time SLA?
        - Where I am throttled?
    - Identify users that are impacted
## **Amazon CodeGuru**

- Amazon CodeGuru is an ML-powered service for automated code reviews and application performance recommendations.
- It provides two functionalities:
    - **CodeGuru Reviewer:** Automated code reviews for static code analysis during development.
    - **CodeGuru Profiler:** Visibility and recommendations about application performance during runtime in production.
### **Amazon CodeGuru Reviewer**
- Identifies critical issues, security vulnerabilities, and hard-to-find bugs.
    - Examples include common coding best practices, resource leaks, security detection, and input validation.
    - Uses Machine Learning and automated reasoning.
    - Draws from hard-learned lessons across millions of code reviews on thousands of open-source and Amazon repositories.
    - Supports Java and Python.
    - Integrates with GitHub, Bitbucket, and AWS CodeCommit.
### **Amazon CodeGuru Profiler**
**Amazon CodeGuru Profiler** helps understand the **runtime behavior of your application.**
- Example: identify if your application is consuming excessive CPU capacity on a logging routine.
- **Features:**
    - Identify and remove code inefficiencies.
    - Improve application performance (e.g., reduce CPU utilization).
    - Decrease compute costs.
    - Provides heap summary to identify which objects are using up memory.
    - Includes anomaly detection.
    - Supports applications running on AWS or on-premise.
    - Minimal overhead on the application.
## **AWS Health Dashboard - Service History**
- Displays the health of all regions and services.
- Provides historical information for each day.
- Offers an RSS feed for subscription. Previously known as AWS Service Health Dashboard.
## **AWS Health Dashboard –Your Account**
- Same service as AWS Health Dashboard, **but this time just for _your_ account**
    - **Shows how AWS outages directly impact you & your AWS resources**
- Global service
- Alert, remediation, proactive, scheduled activities

## **AWS X-RAY**
X-Ray helps you:
- **Trace requests** through your application
- **Debug issues**
- Find **performance bottlenecks**
- Analyze **errors and latency**

## **AWS CodeCommit**
> A **fully managed Git-based source code repository**

It is used to:
- Store source code
- Manage version control (Git)
- Access repositories securely over the internet
- Collaborate with teams
## **AWS Shield**
AWS Shield helps:
> Detect and block malicious traffic automatically.

It absorbs the attack traffic before it takes down your application.