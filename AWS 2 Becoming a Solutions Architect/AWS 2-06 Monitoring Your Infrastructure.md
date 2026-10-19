**Amazon CloudWatch** provides a reliable, scalable, and flexible monitoring solution. You no longer need to set up, manage, and scale your own monitoring systems and infrastructure.
Amazon CloudWatch is basically a metrics repository. An AWS service—such as Amazon EC2—puts metrics into the repository, and you retrieve statistics based on those metrics.
You can configure *alarm* actions to stop, start, or terminate an Amazon EC2 instance when certain criteria are met. In addition, you can create alarms that initiate Amazon EC2 Auto Scaling and Amazon Simple Notification Service (Amazon SNS) actions on your behalf.
**Amazon CloudWatch components**
- CloudWatch Metrics and Anomaly Detection
- CloudWatch Alarms
- Amazon EventBridge (formerly CloudWatch Events)
- CloudWatch Logs
- CloudWatch Insights
You can create CloudWatch dashboards using the following methods:
- **AWS Management Console**
- **AWS CLI**
- **PutDashboard API**
----
Metrics enable you to monitor a specific element of an application or resource over a period of time
**Metric collection frequency:**
- **Basic Monitoring (free):** EC2 metrics collected every **5 minutes** by default.
- **Detailed Monitoring (paid):** Metrics collected every **1 minute** — useful when you need faster Auto Scaling reactions.
- **High-Resolution Custom Metrics:** Publish custom metrics at **1-second granularity** by setting `StorageResolution=1`.
Custom metrics are **regional**, a metric in us-east-1 is completely separate from the same-named metric in eu-west-1.
**Exam tip:** CloudWatch does **not** collect RAM utilization or disk space from EC2 by default. You must install the **CloudWatch Agent** on the instance to capture these. This is one of the most commonly tested CloudWatch concepts on SAA-C03.

**Anomaly Detection**: It implement machine learning algorithms against your metrics data to help detect any activity that sits outside of the normal baseline parameters
**How it Works:**
1. **Baseline Establishment**
2. **Anomaly Identification**
3. **Alerting and Notification**
----
**CloudWatch Alarms**: Integrate with metrics and allow you to implement automatic actions based on specific thresholds that you can configure relating to each metric
**CloudWatch Alarms States**:
1. _**OK**_ – The metric or expression is within the defined threshold.
2. _**ALARM**_ – The metric or expression is outside of the defined threshold.
3. _INSUFFICIENT_DATA_ – The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state.
---
**Amazon CloudWatch Logs** lets you monitor, store, and access log files from EC2, CloudTrail, Route 53, Lambda, and other AWS services.
- **Log Group:** A container for log streams that share the same retention, access control, and monitoring settings. Example: `/aws/lambda/my-function`
- **Log Stream:** A sequence of log events from a single source within a log group. Example: each Lambda invocation gets its own stream.
- **Retention:** By default, logs are stored indefinitely. You can configure retention from 1 day to 10 years (or Never Expire).
> **Exam tip:** Logs never expire by default. For cost control, always set a retention policy on log groups.
---

CloudWatch Insights helps you gain deeper insights from your AWS environment through different lenses. There are **4 types of insights** within CloudWatch:

1. **Log Insights**
It is a powerful tool that enables you to analyze and visualize log data captured by CloudWatch Logs by using *interactive queries*
Use a SQL-like query language to **filter**, **aggregate**, and **analyze** log data.
2. **Container Insights**
It provides a comprehensive view of your containerized applications running on AWS services like EKS and ECS
3. **Lambda Insights**
It offers a deep dive into the performance and behavior of your AWS Lambda functions
4. **Application Insights**
Designed for monitoring .NET and SQL Server applications running on EC2. It automatically discovers application components, sets up monitoring, and creates dashboards for common issues like slow queries, OOM errors, and failed deployments.

---
**AWS CloudTrail** is a service that monitors and logs API activity within your *AWS account*
>**Exam tip:** CloudTrail is **enabled by default** for all AWS accounts and retains the last **90 days** of management events in the Event History at no extra cost. To retain logs longer or capture more event types, create a Trail.

AWS CloudTrail will:
- Log, continuously monitor, and retain account activity across your AWS infrastructure
- Record API calls for most AWS services
- Automatically push logs to Amazon S3 when a Trail is configured
**CloudTrail Event Types**
![[Pasted image 20261005185710.png]]
A **Trail** is a configuration that delivers CloudTrail events to an S3 bucket, with optional delivery to CloudWatch Logs and Amazon EventBridge.
**Trail scope options:**
- **Single-region trail:** Logs events only in the region where it was created.
- **Multi-region trail:** Logs events from all regions into a single S3 bucket — recommended for comprehensive auditing.
- **Organization trail:** Created in the management account of AWS Organizations to capture activity across all member accounts.
Configure CloudWatch Logs to receive logs from CloudTrail (optional) to be able to filter them

---
*AWS Config* is a fully managed service that enables you to assess, audit, and evaluate the configuration of your *AWS resources*.
- **CloudTrail** answers: _Who did what?_ (API calls, user actions)
- **AWS Config** answers: _What does my resource look like now, and how has it changed?_ (configuration state over time)
**Exam tip:** AWS Config is **per-region and per-account** by default. Use a **Config Aggregator** to aggregate compliance data across multiple accounts and regions into a single view.
```
#### How AWS Config addresses configuration management challenges?

1. Reporting Current System Status and Configuration
    - Configuration Items (CIs): AWS Config continuously records the configuration state of your AWS resources. This includes details like security group rules, IAM roles, and S3 bucket permissions.
    - Historical Records: It maintains a historical record of these configurations, allowing you to track changes over time and identify potential issues.
    - Custom Rules: You can create custom rules to assess your resources against specific compliance standards or security best practices. These rules can be configured to trigger alerts or take automated actions when violations are detected.
2. Controlling Configuration History
    - Version History: AWS Config keeps a version history of each resource's configuration, enabling you to compare past and present states.
    - Change Tracking: It tracks changes to your resources and provides detailed information about who made the change, when it was made, and what the previous configuration was.
    - Auditing and Compliance: By analyzing configuration history, you can conduct regular audits to ensure compliance with security and regulatory standards.
3. Visibility into Human Errors and Change Management
    - Configuration Change Notifications: AWS Config can send notifications when changes are made to your resources, allowing you to monitor and control the change process.
    - Automated Remediation: You can automate remediation actions for certain configuration violations, reducing the risk of human error.
    - Centralized Configuration Management: By centralizing your configuration management with AWS Config, you can improve visibility and control over your AWS environment.
```
**AWS Config rules** check the configuration state of your AWS resources against specific compliance standards or security best practices. **Managed or Custom Rules** using lambda

A **Conformance Pack** is a collection of Config rules bundled together as a single deployable unit. AWS provides ready-made conformance packs for standards like CIS AWS Foundations, HIPAA, and PCI-DSS. You can also build custom ones.

**Remediation Actions**: When AWS Config detects a non-compliant resource, it can trigger automatic remediation using **AWS Systems Manager (SSM) Automation** runbooks

---
**Amazon EventBridge** Event-Driven Automation

- Serverless event bus that routes events from AWS services, custom apps, and SaaS partners
- Three bus types: Default (AWS events), Custom (your apps), Partner (third-party SaaS)
- Rules match events and fan-out to up to 5 targets; supports event transformation
- **Archive & Replay** lets you store and re-process past events useful for testing and recovery
---
> **SAA-C03 Exam Anti-Patterns to Avoid:**
- Using CloudTrail to track _what_ a resource looks like → that's **Config**
- Expecting CloudWatch to monitor RAM/disk on EC2 without the **CloudWatch Agent**
 - Using SNS when you need to replay events → use **EventBridge Archive & Replay**
- Creating separate trails per account instead of an **Organization trail**