# 🛡️ AWS IAM & Security Hardening Project

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws)
![IAM](https://img.shields.io/badge/IAM-Security-blue)
![CloudTrail](https://img.shields.io/badge/CloudTrail-Logging-green)
![GuardDuty](https://img.shields.io/badge/GuardDuty-Threat%20Detection-red)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📌 Project Overview

This project demonstrates a real-world **AWS Cloud Security Hardening** implementation simulating an enterprise environment. It covers Identity & Access Management (IAM), security monitoring, compliance, and automated alerting — core responsibilities of a **Cloud Administrator**.

The project follows the **principle of least privilege**, ensuring users only have the permissions they need and nothing more.

---

## 🏗️ Architecture

```
AWS Account
├── IAM
│   ├── Groups
│   │   ├── Admins        → AdministratorAccess
│   │   ├── Developers    → EC2 + S3 (restricted to specific bucket)
│   │   └── ReadOnly      → ViewOnlyAccess
│   ├── Users
│   │   ├── admin-user    → MFA Enabled
│   │   ├── developer-user → MFA Enabled
│   │   └── readonly-user  → MFA Enabled
│   └── Custom Policies
│       └── DeveloperS3BucketPolicy → Restricts access to specific S3 bucket
│
├── Security & Monitoring
│   ├── CloudTrail        → Logs all API calls (multi-region)
│   ├── AWS Config        → Compliance rules & resource tracking
│   ├── GuardDuty         → Threat detection & anomaly detection
│   └── CloudWatch        → Metric filters & alarms
│
└── Alerting
    ├── SNS Topic          → security-alerts (email notifications)
    └── CloudWatch Alarms
        ├── RootAccountLoginAlert
        └── UnauthorizedAPICallAlert
```

---

## ☁️ AWS Services Used

| Service | Purpose |
|---|---|
| **IAM** | Identity & Access Management, least privilege |
| **S3** | Storage for CloudTrail & Config logs |
| **CloudTrail** | API call logging across all regions |
| **AWS Config** | Compliance monitoring & rules |
| **GuardDuty** | Intelligent threat detection |
| **CloudWatch** | Metric filters & security alarms |
| **SNS** | Email alerting for security events |

---

## 🔐 IAM Setup

### Groups & Permissions

| Group | Policy Attached | Access Level |
|---|---|---|
| `Admins` | AdministratorAccess | Full access |
| `Developers` | EC2FullAccess + S3FullAccess + Custom Policy | Limited access |
| `ReadOnly` | ViewOnlyAccess | View only |

### Custom Policy — DeveloperS3BucketPolicy

Restricts Developer users to only one specific S3 bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::dev-project-bucket-som",
        "arn:aws:s3:::dev-project-bucket-som/*"
      ]
    }
  ]
}
```

### MFA Enforcement
- ✅ MFA enabled on all IAM users using Google Authenticator
- ✅ Prevents unauthorized access even if passwords are compromised

---

## 📋 AWS Config Rules

| Rule | Purpose |
|---|---|
| `iam-user-mfa-enabled` | Checks MFA is enabled on all IAM users |
| `root-account-mfa-enabled` | Checks root account has MFA |
| `s3-bucket-public-read-prohibited` | Prevents public S3 buckets |
| `access-keys-rotated` | Ensures access keys are rotated every 90 days |

---

## 🚨 CloudWatch Alarms & Alerts

### Metric Filters Created

| Filter | Pattern | Purpose |
|---|---|---|
| `RootAccountUsageFilter` | Root account activity | Detects root login |
| `UnauthorizedAttemptsFilter` | AccessDenied errors | Detects unauthorized access |

### Alarms

| Alarm | Trigger | Action |
|---|---|---|
| `RootAccountLoginAlert` | Root account used | Email via SNS |
| `UnauthorizedAPICallAlert` | Unauthorized API call | Email via SNS |

---

## ✅ Security Testing Results

| Test | Expected Result | Actual Result |
|---|---|---|
| `readonly-user` tries to create S3 bucket | Access Denied | ✅ Access Denied |
| `readonly-user` MFA enforcement | MFA required | ✅ MFA prompted |
| `developer-user` restricted to specific bucket | Access to other buckets denied | ✅ Verified |
| Root account login alarm | Email alert triggered | ✅ Alarm triggered |

---

## 🗂️ Project Structure

```
aws-iam-security-hardening/
├── README.md
├── policies/
│   └── developer-s3-policy.json
└── screenshots/
    ├── 01-iam-groups.png
    ├── 02-iam-users.png
    ├── 03-mfa-enabled.png
    ├── 04-custom-policy.png
    ├── 05-cloudtrail.png
    ├── 06-config-rules.png
    ├── 07-guardduty.png
    ├── 08-sns-topic.png
    ├── 09-cloudwatch-alarms.png
    └── 10-access-denied-test.png
```

---

## 📸 Screenshots

> Screenshots of all implemented components and test results are available in the `/screenshots` folder.

---

## 💡 Key Learnings

- Implementing **least privilege access** using IAM groups and custom policies
- Setting up **multi-region CloudTrail** for complete audit logging
- Configuring **AWS Config rules** for continuous compliance monitoring
- Enabling **GuardDuty** for intelligent threat detection
- Creating **CloudWatch metric filters** from CloudTrail logs
- Building **automated alerting** using SNS and CloudWatch alarms
- Testing and **validating security controls** in a real AWS environment

---

## 🎯 Skills Demonstrated

- AWS IAM (Users, Groups, Policies, MFA)
- Cloud Security & Compliance
- Security Monitoring & Logging
- Automated Alerting
- Least Privilege Principle
- AWS Free Tier Cost Management

---

## 👤 Author

**Som Athghara**
- 🎓 BCA Final Year — Sri Sri University
- 💼 Aspiring Cloud Administrator / DevOps Engineer
- 🌐 [Portfolio](https://som15.onrender.com)
- 🔗 [LinkedIn](https://www.linkedin.com/in/som-athghara-71262122b/)
- 🐙 [GitHub](https://github.com/Somgit001)

---

## 📅 Project Date

March 2026

---

> ⚠️ **Note:** All AWS resources were deleted after project completion to avoid charges. Screenshots are available as proof of implementation.
