- **IaaS (e.g., EC2):** You manage the OS, runtime, and application — more customer responsibility.
- **PaaS (e.g., RDS, Elastic Beanstalk):** AWS manages the platform; you manage data and access.
- **SaaS (e.g., S3, DynamoDB):** AWS manages almost everything; you manage data and access policies.
#### Design principles for the security pillar
- Implement a strong identity foundation
- Protect data in transit and at rest
- Apply security at all layers
- Keep people away from data
- Maintain traceability
- Prepare for security events
- Automate security best practices
**SAA Exam Tip:** Defense-in-depth is a recurring theme. The exam often tests whether you're applying security at the right layer e.g., Security Groups for instance-level traffic control, WAF for application-layer threats, and KMS for data encryption.

- Use **IAM Access Analyzer** to identify overly permissive policies and validate that policies grant only intended access.
- Use **AWS Organizations Service Control Policies (SCPs)** to enforce permission guardrails across all accounts in your organization SCPs act as a ceiling on what any identity in an account can do, regardless of their IAM policies.
- Key monitoring services: **CloudTrail** (who did what), **GuardDuty** (threat detection), **AWS Config** (compliance), **Security Hub** (centralized findings).

**SAA Exam Tip:** An explicit Deny in an SCP overrides any Allow in an IAM policy. This is a common exam trap even if a user has `AdministratorAccess`, an SCP deny will block the action.

- Email address and password **is associated with** Associated with AWS account (root)
- IAM user name and password **is associated with** accessing the AWS Management Console.
- Access ID and secret access keys **is associated with** AWS Command Line Interface (AWS CLI) and programmatic  requests, like application programming interfaces (APIs) and software development  kits (SDKs).
- Multi-factor authentication (MFA) **is associated with** Extra layer of security which can be enabled for AWS account root user and IAM users
- Key pairs **is associated with** specific AWS services like Amazon EC2.

Policies can be assigned to:
- **IAM Users**: Individuals with their own set of credentials.
- **IAM Groups**: Collections of users that share the same permissions.
- **IAM Roles**: Assigned to entities like AWS services or users in another account, enabling temporary access.
**SAA Exam Tip:** When an application running on EC2 needs to access S3 or DynamoDB, the correct answer is **always an IAM role attached to the EC2 instance** never hardcode credentials, never store access keys on the instance. This is one of the most commonly tested patterns.
For workforce access, prefer **IAM Identity Center** over individual IAM users.

**IAM roles** grant permissions without attaching them to specific users or groups. Instead, users or services assume roles and receive **temporary credentials** issued by **AWS STS (Security Token Service)** these credentials expire automatically, which is why roles are more secure than long-term access keys.
**Use cases include:**
- **EC2 instance accessing AWS services** attach a role to the instance, no credentials stored
- **Cross-account access** allow a user in Account A to assume a role in Account B
- **Federated access** allow external identities (corporate SSO, SAML, OIDC) to assume roles
- **Service-to-service** Lambda invoking DynamoDB, ECS tasks accessing S3, etc.
#### IAM policies

**Types of IAM policies:**
1. **Identity-based:** Attach to an IAM user, group, or role defines what that identity can do.
2. **Resource-based:** Attach directly to an AWS resource (e.g., S3 bucket policy, KMS key policy) defines who can access that resource.
3. **AWS managed policies:** Predefined by AWS, reusable across accounts. Examples: `AdministratorAccess`, `ReadOnlyAccess`, `AmazonS3FullAccess`.
4. **Customer managed policies:** Created and managed by you more granular control than AWS managed policies.
5. **Inline policies:** Embedded directly into a single user, group, or role. Deleted when the identity is deleted. Avoid for most use cases.
6. **Permission boundaries:** An advanced feature that sets the maximum permissions an identity-based policy can grant. Useful when delegating permission management.
7. **Service Control Policies (SCPs):** Applied at the AWS Organizations level. Set permission guardrails across entire accounts or OUs they limit what any identity in the account can do, regardless of their IAM policies.
![[Pasted image 20260911050455.png]]

IAM uses three simple rules to decide if an action is allowed:
1. **Explicit Deny** → Always blocks the action, no exceptions.
2. **Explicit Allow** → Permits the action if no Deny overrides it.
3. **Implicit Deny** → Everything is denied by default if no Allow exists.

**Evaluation order:** SCPs → Resource-based → Identity-based → Permission boundaries → Session policies
**SAA Exam Tip:** Deny always wins. Even `AdministratorAccess` can be blocked by an SCP.

For **cross-account access**, both sides must allow it: the identity needs an allow in their account, and the resource needs an allow on its policy.
Ayman's identity-based policy allows GET, PUT, and LIST on bucket X. But the bucket's resource-based policy only allows GET and LIST and **explicitly denies PUT**. Result: Ayman cannot PUT objects, even though his own policy allows it.
This is the **Deny wins** rule in action: the resource-based deny overrides the identity-based allow.
- Resource-based policies can grant cross-account access directly without needing the external principal to assume a role.
- Identity-based policies alone cannot grant cross-account access you need either a resource-based policy or a role assumption.

**SAA Exam Tip:** Only resource-based policies have a **Principal** field. If you see `"Principal"` in a policy JSON, it's resource-based.

Example of IAM policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:*",
        "s3:*"
      ],
      "Resource": [
        "arn:aws:dynamodb:eu-west-3:123456789:table/projectx-table",
        "arn:aws:s3:::projectx-bucket",
        "arn:aws:s3:::projectx-bucket/*"
      ]
    },
    {
      "Effect": "Deny",
      "Action": [
        "dynamodb:*",
        "s3:*"
      ],
      "NotResource": [
        "arn:aws:dynamodb:eu-west-3:123456789:table/projectx-table",
        "arn:aws:s3:::projectx-bucket",
        "arn:aws:s3:::projectx-bucket/*"
      ]
    }
  ]
}
```
This IAM policy grants specific permissions to interact with DynamoDB and S3 resources related to the "projectx" project:
**Allow Statement:**
- Grants permissions to perform any DynamoDB and S3 action on the specified resources:
    - DynamoDB table: `projectx-table`
    - S3 bucket: `projectx-bucket` and all its objects

**Deny Statement:**
- Denies all DynamoDB and S3 actions on any resources **except** the ones explicitly allowed in the previous statement. This ensures that access is limited to the specified resources.
---
**Policy version:** Always use `"2012-10-17"` this is the current and only supported policy language version. Older policies may use `"2008-10-17"` but you should always specify the latest.
![[Pasted image 20260911065032.png]]

**Example: Identity-based policy**
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*",
                "s3:Describe*",
                "s3-object-lambda:Get*",
                "s3-object-lambda:List*"
            ],
            "Resource": "*"
        }
    ]
}
```
**Breakdown:**
- **Effect**: "Allow" – Grants permission.
- **Action**: Specifies the allowed actions:
    - "s3:Get*" – Grants all Get operations on S3 (e.g., s3:GetObject, s3:GetBucketLocation).
    - "s3:List*" – Grants all List operations on S3 (e.g., s3:ListBucket).
    - "s3:Describe*" – Allows any action related to describing S3 resources.
    - "s3-object-lambda:Get*" – Grants all Get operations for S3 Object Lambda.
    - "s3-object-lambda:List*" – Grants all List operations for S3 Object Lambda.
- **Resource**: "*" – This grants access to **all S3 buckets and objects** across the account.

**Example: resource-based policy**
```json
{
    "Version": "2012-10-17",
    "Id": "Policy1727115263739",
    "Statement": [
        {
            "Sid": "Stmt1727115259915",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::132456789:user/ali"
            },
            "Action": "s3:GetObject",
            "Resource": [
                "arn:aws:s3:::alibucket",
                "arn:aws:s3:::alibucket/*"
            ]
        }
    ]
}

```
**Breakdown:**
- **Effect**: "Allow" – Grants permission.
- **Principal**: The IAM user ali (from AWS account 132456789) is given access.
- **Action**: "s3:GetObject" – Grants permission to read objects in the bucket.
- **Resource**:
    - "arn:aws:s3:::alibucket" – Refers to the S3 bucket itself.
    - "arn:aws:s3:::alibucket/*" – Refers to all objects within the bucket.

#### IAM policy with a condition added
Common condition keys:
- **`aws:SourceIp`** Restrict access by source IP or CIDR range
- **`aws:RequestedRegion`** Restrict actions to specific AWS Regions
- **`aws:MultiFactorAuthPresent`** Require MFA to be active for sensitive actions
- **`aws:PrincipalOrgID`** Restrict access to principals within your AWS Organization only
- **`s3:prefix`** Restrict S3 access to specific key prefixes

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*",
                "s3:Describe*",
                "s3-object-lambda:Get*",
                "s3-object-lambda:List*"
            ],
            "Resource": "*",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": [
                        "192.168.1.0/24",
                        "10.0.0.0/16"
                    ]
                }
            }
        }
    ]
}
```

**Using conditions to allow access to a specific region**:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::my-bucket/*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-1"
                }
            }
        }
    ]
}
```

```json
"aws:RequestedRegion": ["us-east-1", "us-west-2"] // for multiple regions
```

**Key Services for Security**
- **CloudTrail** logs all API calls (who did what, when, from where)
- **IAM Access Analyzer** dentifies policies that grant access outside your account/organization
- **IAM Policy Simulator** test what a policy allows before deploying
- **GuardDuty** threat detection (unusual API calls, suspicious behavior)
- **AWS Config** tracks configuration changes and compliance