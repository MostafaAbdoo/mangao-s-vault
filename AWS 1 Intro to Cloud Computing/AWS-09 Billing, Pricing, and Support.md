## AWS Organizations
AWS Organizations = management service that **enables you to consolidate multiple AWS accounts into an _organization_ that you create and centrally manage.** AWS Organizations includes account management and consolidated billing capabilities that enable you to better meet the budgetary, security, and compliance needs of your business.

- Manages multiple AWS accounts, acts as the central master account
- **Cost Benefits:**
    - **Consolidated Billing = Single payment method across all accounts**
    - Discounts from **aggregated usage** (e.g., EC2, S3)
    - Reserved Instance Pooling → **Optimal savings with pooled EC2 instances**
- API available for automating AWS account creation
- Control Access: Restrict account privileges using Service Control Policies (SCP) (which you will see next slide)
### **Multi Account Strategies**
- Create **separate accounts for each department, cost center, and environment (dev/test/prod).**
- Implement Service Control Policies (SCP) for regulatory compliance.
- Use VPCs for resource isolation within each account.
- Manage separate service limits for each account.
- Establish a dedicated logging account.
- Choose between multi-account or single account with multiple VPCs based on needs.
- Standardize tagging for efficient billing management.
- Enable AWS CloudTrail in all accounts; direct logs to a central S3 bucket.
- Centralize CloudWatch Logs to a dedicated logging account.
### **Organizational Units**
- Organizational Units = You can simplify account management by grouping related accounts into Organizational Units (OUs) within AWS Organizations, **allowing for uniform policy administration and streamlined operations.**
- **Useful whitepaper here [_https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_ous.html_](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_ous.html)**

### **Service Control Policies (SCP)**
- SCPs = whitelist or blacklist IAM actions.
- Applied at the OU or Account level, excluding the Master Account.
- Affects all Users and Roles, including the Root user.
- Does not impact service-linked roles.
- SCPs **must have explicit Allow rules.**
- Use cases include **restricting access to specific services and enforcing compliance standards such as PCI.**
### **AWS Organizations – Consolidated Billing**
- When activated, **AWS Organizations** offers **consolidated billing**, which allows for:
    - **Combined Usage:** Aggregates usage across all accounts within the organization to leverage volume pricing, Reserved Instances, and Savings Plans discounts collectively.
    - **Single Bill:** Consolidated billing into one invoice for all AWS accounts within the organization.
    - **Control over Discounts:** The management account can disable the sharing of Reserved Instances discounts for any account within the organization, including itself.
## AWS Control Tower
AWS Control Tower = offers a streamlined solution for **establishing and managing a secure and compliant multi-account AWS environment, incorporating industry best practices.**
- Advantages include:
    - Automates environment setup with **minimal configuration steps.**
    - Automates **ongoing policy enforcement using guardrails to maintain compliance.**
    - Identifies **policy breaches** and **initiates corrective actions automatically**.
    - Provides an interactive dashboard for **monitoring and ensuring adherence to compliance standards.**
- Use cases include restricting access to specific services and enforcing compliance standards such as PCI.
## **AWS Resource Access Manager (AWS RAM)**
- **Share AWS resources with other AWS accounts or within your organization**
- Prevent resource duplication and foster collaboration
- Supported resources include Aurora databases, VPC subnets, Transit Gateway attachments, Route 53 hosted zones, EC2 Dedicated Hosts, and License Manager configurations
- Choose between sharing with specific accounts or within your AWS Organization
- Enhance resource utilization, streamline management, and improve collaboration with shared resources
- Ensure security with robust IAM policies and regular resource usage monitoring
## **AWS Service Catalog**
- New AWS users often **face numerous options, resulting in non-compliant or inconsistent stacks.**
- Some users prefer a simple self-service portal for accessing pre-approved products.
- AWS Service Catalog provides a solution by **offering access to pre-defined virtual machines, databases, storage options, etc.**
## Pricing Calculator
AWS Pricing Calculator = a free web-based planning tool that you can use to **create cost estimates for using AWS services.** You can use AWS Pricing Calculator for the following use cases:
- Model your solutions before building them
- Explore AWS service price points
- Review the calculations behind your estimates
- Plan your AWS spend
- Find cost saving opportunities
## Billing Dashboard
📍 The AWS Bills page provides a monthly view of your AWS charges. For monthly billing periods that have not yet closed, the Bills page will display the most recent estimated charges based on services metered to date.
[_**Source**_](https://aws.amazon.com/blogs/startups/getting-started-with-aws-cost-management/)
### **AWS Free Tier Dashboard
- Check out all the free services AWS provides
    - Elastic BeanStalk, CloudFormation, and AutoScaling groups → you have to pay for the resources you create!
[_**Source**_](https://aws.amazon.com/blogs/aws/new-monitor-your-aws-free-tier-usage/)
## Cost Allocation Tags
- Use **cost allocation tags to track your AWS costs on a detailed level**
### **Tagging and Resource Groups**
- **Tags for Organization: Utilize tags to organize various resources such as:**
    - **EC2 instances, images, load balancers, and security groups.**
    - Apply tags to RDS, VPC resources, Route 53, IAM users, etc.
    - **Resources created by CloudFormation are tagged uniformly.**
- **Common tags include Name, Environment, Team.**
- **Use tags to create and manage groups of resources with common tags, aka Resource Groups**
    - Create, maintain, and view a collection of resources that share a common tag
    - **Utilize the Tag Editor for efficient tag management.**
## Cost and Usage Reports
- Gain deeper insights into your AWS costs and usage.
- **The AWS Cost & Usage Report** offers **comprehensive data on AWS cost and usage, including additional metadata on AWS services, pricing, and reservations like Amazon EC2 Reserved Instances (RIs).**
- It itemizes AWS usage for each service category used by an account and its IAM users in hourly or daily line items, along with any tags activated for cost allocation purposes. Integration options include Athena, Redshift, or QuickSight for enhanced analysis.
## Cost Explorer
- Gain insight into your **AWS costs and usage trends over time.**
- Generate custom reports to analyze cost and usage data according to your needs.
- Analyze data at various levels: from total costs and usage across all accounts to detailed monthly or hourly breakdowns at the resource level.
- Optimize costs by selecting the most suitable Savings Plan based on your usage patterns.
- **Forecast usage for up to 12 months** based on historical usage data.
## **Billing Alarms in CloudWatch**
- CloudWatch stores billing data metrics in the us-east1 region.
- This **data encompasses the total worldwide AWS costs incurred.**
- It reflects actual costs, not projected costs.
- While suitable for basic alarms, it **lacks the robust features of AWS Budgets for comprehensive cost management.**
## **AWS Budgets**
- Create budget and **send alarms when costs exceeds the budget**
- 4 types of budgets: Usage, Cost, Reservation, Savings Plans
- For Reserved Instances (RI)
    - Track utilization
    - Supports EC2, ElastiCache, RDS, Redshift
    - Up to 5 SNS notifications per budget
    - Can filter by: Service, Linked Account, Tag, Purchase Option, Instance
- Type, Region, Availability Zone, API Operation, etc…
- Same options as AWS Cost Explorer!
- 2 budgets are free, then $0.02/day/budget
## **AWS Cost Anomaly Detection**
- Utilize **machine learning to continuously monitor your cost and usage.**
- ML algorithms learn your historical spending patterns to detect anomalies, including one-time spikes or continuous increases, without the need for predefined thresholds.
- Monitor various aspects such as AWS services, member accounts, cost allocation tags, or cost categories.
- Receive anomaly detection reports with root-cause analysis.
- Choose to be notified with individual alerts or daily/weekly summaries via SNS.
## **AWS Service Quotas**
- Notify you when **you’re close to a service quota value threshold**
    - Create CloudWatch Alarms on the Service Quotas console
    - Example: Lambda concurrent executions
    - Request a quota increase from AWS Service Quotas or shutdown resources before limit is reached
## **Trusted Advisor**
- Conduct a **comprehensive assessment of your AWS accounts without the need for installation.**
- Receive recommendations across six key categories: cost optimization, performance, security, fault tolerance, service limits, and operational excellence.
- Available for users with Business or Enterprise Support plans.
- Includes a full set of checks and offers programmatic access via the AWS Support API.
## Pricing Models Summary

- **Compute Pricing (EC2):** Pay for computing power by the hour or second, with options for on-demand, reserved, and spot instances.
- **Storage Pricing (S3):** Pay for storage space used in Amazon S3, with options for different storage classes and data transfer.
- **Database Pricing (RDS):** Pay for database usage based on DB instance type, storage, and I/O requests, with options for on-demand or reserved instances.
- **AWS Support Plans Pricing:** Choose from Basic, Developer, Business, Enterprise On-Ramp, and Enterprise Support plans, with varying levels of technical support and response times.
## **AWS Total Cost of Ownership (TCO) Calculator**
Compare the cost of running IT infrastructure on-premises vs. AWS Cloud