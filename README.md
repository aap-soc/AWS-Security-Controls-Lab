### 🔐 AWS-Security-Controls-Lab - Using AWS 

- **Focus**: Cloud Security | IAM | KMS | S3 Encryption
- **Environment**: AWS Free Tier | Region: eu-west-2 (London)
- **Date**: July 2026
- **Author**: Andre Patterson 

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📋 **Overview**

I present a mini hands-on AWS security lab completed as part of an active transition into Cloud Security and SOC Engineering. Each lab targets a core AWS security service -demonstrating practical knowledge of identity and access management, encryption key creation, and encrypted object storage.

All labs were completed live in the AWS Management Console on a Free Tier account in the eu-west-2 (London) region. Every step is documented with screenshots as verifiable evidence of hands on configuration.


**Evidence integrity**: The screenshots are taken from the completed console work and have been sanitised for a public GitHub repository. Account IDs, email addresses, ARNs, key IDs, bucket identifiers, sign-in information, IP addresses, and session identifiers are redacted.


**Project objective**

Build a small AWS security baseline that applies four core controls:

1- Identity control through IAM users, groups, and an AWS-managed audit policy.

2-Cryptographic control through a customer-managed symmetric KMS key.

3- Data protection through a private S3 bucket using SSE-KMS encryption.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🗂️ Repo Structure

aws-security-controls-lab/
- │
- ├── README.md                        ← Project overview
- ├── docs/
- │   └── lab-notes.md                 ← Step-by-step notes mirroring every screenshot
- └── screenshots/
-       ├── lab1-iam/                          ← 10 screenshots
-       ├── lab2-kms/                          ←  9 screenshots
-       ├── lab3-s3-encyrption/                ←  9 screenshots
-       └── lab 4- CloudTrail (Audit Logging)  ← 13 screenshots



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🧠 Why These Services Matter in Security


|        **Service**          |           **Security Purpose**               |                           Real-World Use                                      |                                 
|-----------------------------|----------------------------------------------|-------------------------------------------------------------------------------|
|          **IAM**            |        Controls who can access what          |          Enforces least privilege across all AWS resources                    |
|                             |                                              |                                                                               |
|          **KMS**            |     Creates and manages encryption keys      |          Protects data at rest across S3, EBS, RDS, CloudTrail                |
|                             |                                              |                                                                               |
|     **S3 + SSE-KMS**        |          Encrypted object storage            |            Prevents data exposure if storage is compromised                   |
|                             |                                              |                                                                               |
|      **CloudTrail**         |          Records every AWS API call          |            Primary SOC audit and forensics tool for incident investigation    |




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## ✅ Labs Completed

# Lab 1- IAM  Identity and Access Management
Controlling who can access what in AWS using least-privilege principles

**What I configured**:
- Reviewed IAM dashboard as root user - identified security recommendations
- Enabled MFA on root account - root account hardening best practice
- Created IAM user **andre-soc-analyst** with console access and forced password reset
- Encountered and resolved group naming error (see troubleshooting log)
- Created group **SOC_Analysts** with **SecurityAudit** managed policy attached
- Added user to group - with permissions inherited via group, not applied directly to user
- Confirmed user created with correct group membership and policy inheritance




**Key security concepts:**

- Least privilege access control
- Group-based permission management vs direct policy attachment
- MFA enforcement on privileged accounts
- IAM user, group and policy structure

  📁 **Screenshots → screenshots/lab1-iam/ 📄 Step-by-step notes → docs/lab-notes.md#lab-1**
  
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Lab 2 -  KMS (Key Management Service)

Creating and managing a customer-managed encryption key


**What I configured:**
- Navigated KMS dashboard and reviewed customer-managed key options
- Created symmetric KMS key - type: Encrypt and Decrypt
- Set alias: soc-lab-key
- Assigned key administrator: andre-soc-analyst
- Assigned key user: andre-soc-analyst
- Reviewed auto-generated key policy JSON confirming access controls
- Confirmed key status: Enabled and ready for use across AWS services



**Key security concepts**:
- Symmetric vs asymmetric encryption keys
- Customer-managed keys vs AWS-managed keys
- Key administrator vs key user separation of duties
- Key policy structure in JSON

📁 **Screenshots → screenshots/lab2-kms/ 📄 Step-by-step notes → docs/lab-notes.md#lab-2**

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Lab 3 - S3 Bucket with Encryption

Encrypted object storage using a customer-managed KMS key

**What I configured:**
- Created S3 bucket: **andre-soc-security-lab1-416689419646-eu-west-2**
- Blocked all public access on the bucket
- Enabled default encryption: SSE-KMS using **soc-lab-key** from Lab 2
- Uploaded test file: **andre-soc-analyst.txt**
- Verified encryption via bucket Properties - SSE-KMS confirmed with correct KMS key ARN
- Confirmed Bucket Key enabled  - reduces KMS API calls and cost




**Key security concepts**:

- S3 public access blocking
- Server-side encryption with KMS (SSE-KMS)
- Customer-managed key vs AWS-managed key for S3
- Verifying encryption configuration via bucket properties

📁 **Screenshots → screenshots/lab3-s3-encryption/ 📄 Step-by-step notes → docs/lab-notes.md#lab-3**

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Lab 4 - CloudTrail (Audit Logging)
Recording every AWS API call for SOC investigation and compliance

**What I configured**:

- Navigated to CloudTrail and reviewed service capabilities to Capture, Store and Monitor
- Configured trail **soc-audit-trail-2** with dedicated log storage bucket **andre-cloudtrail-logs2**
- Enabled Log file SSE-KMS encryption using **soc_lab_key** — encrypting all audit logs at rest
- Enabled Log file validation - generates cryptographic digest files for tamper detection
- Configured management events: All API activity (Read + Write) with KMS events included
- Confirmed trail status: 🟢 Logging - active and recording all AWS management API calls
- Encountered and resolved a real **InsufficientEncryptionPolicyException** error (see troubleshooting log below)


**Key security concepts**:

- CloudTrail as the primary SOC audit and forensics tool in AWS
- KMS key policy vs IAM policy separation - ensure services have explicit key policy permissions
- Service principals in KMS — **cloudtrail.amazonaws.com** must be explicitly authorised
- Log file validation for cryptographic tamper evidence (required by PCI DSS Requirement 10)
- SSE-KMS for audit logs - only authorised principals can read encrypted log data
- Multi-region trail — prevents audit blind spots across all AWS regions
- Least privilege for CloudTrail — granting only kms:GenerateDataKey* and kms:DescribeKey

📁 Screenshots → screenshots/lab4-cloudtrail/ 📄 Step-by-step notes → docs/lab4-cloudtrail-notes.md

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🔧 Troubleshooting Log


|        **Lab**         |           **Issue**          |                        **Cause**                      |               Resolution                         |                               
|------------------------|------------------------------|-------------------------------------------------------|--------------------------------------------------|
|          Lab 1         |     Group Creation failed    |   **SOC-Analysts** name contained invalid characters  |   Renamed to **SOC_Analysts** using underscore   |                       
|                        |                              |                                                       |                                                  |




|     **Lab**      |           **Issue**                       |                        **Cause**                      |                       Resolution                                 |                               
|------------------|-------------------------------------------|-------------------------------------------------------|------------------------------------------------------------------|
|      Lab 4       | **InsufficientEncryptionPolicyException** |    **soc_lab_key** KMS key policy did not include     | Navigated to KMS → edited soc_lab_key key policy → added          |                  |                                            cloudtrail.amazonaws.com as a service principal- IAM     CloudTrail service principal with kms:GenerateDataKey*          
|                  |                                           | permissions alone are insufficient to grant a service | and kms:DescribeKey permissions → recreated trail successfully   
                                                                       
                                                                      




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📚 Key Security Concepts Demonstrated

- **Least privilege** - Security Audit policy gives read-only access, nothing more
- **MFA enforcement** - root account protected with multi-factor authentication
- **Group-based permissions** - policies attached to groups not individual users
- **Customer-managed encryption keys** - full control over who can encrypt/decrypt
- **Encryption at rest** - SSE-KMS applied to all objects in the S3 bucket
- **Separation of duties** - key administrator and key user are distinct role
- **Audit logging** - every AWS API call recorded via CloudTrail with tamper-evident validation
- **KMS service principals** - AWS services require explicit key policy authorisation separate from IAM

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🔗 Related Projects

- 🖥️ Virtualisation Lab - Ubuntu on VirtualBox (https://github.com/aap-soc/Virtualisation-Lab)
- 🔐 Linux Security & Log Handling Portfolio (https://github.com/aap-soc/linux-security-portfolio)
