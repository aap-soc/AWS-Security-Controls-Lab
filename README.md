### AWS-Security-Controls-Lab - Using AWS 

- **Focus**: Cloud Security | IAM | KMS | S3 Encryption
- **Environment**: AWS Free Tier | Region: eu-west-2 (London)
- **Date**: July 2026
- **Author**: Andre Patterson | LinkedIn | GitHub

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📋 **Overview**

I present a mini hands-on AWS security lab completed as part of an active transition into Cloud Security and SOC Engineering. Each lab targets a core AWS security service -demonstrating practical knowledge of identity and access management, encryption key creation, and encrypted object storage.

All labs were completed live in the AWS Management Console on a Free Tier account in the eu-west-2 (London) region. Every step is documented with screenshots as verifiable evidence of hands on configuration.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------















--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🧠 Why These Services Matter in Security


|        **Service**          |           **Security Purpose**               |                           Real-World Use                                      |                                 
|-----------------------------|----------------------------------------------|-------------------------------------------------------------------------------|
|          **IAM**            |        Controls who can access what          |          Enforces least privilege across all AWS resources                    |
|                             |                                              |                                                                               |
|          **KMS**            |     Creates and manages encryption keys      |          Protects data at rest across S3, EBS, RDS, CloudTrail                |
|                             |                                              |                                                                               |
|     **S3 + SSE-KMS**        |          Encrypted object storage            |            Prevents data exposure if storage is compromised                   |



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

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 📚 Key Security Concepts Demonstrated

**Least privilege** - Security Audit policy gives read-only access, nothing more
**MFA enforcement** - root account protected with multi-factor authentication
**Group-based permissions** — policies attached to groups not individual users
**Customer-managed encryption keys** - full control over who can encrypt/decrypt
**Encryption at rest** - SSE-KMS applied to all objects in the S3 bucket
**Separation of duties** - key administrator and key user are distinct roles
