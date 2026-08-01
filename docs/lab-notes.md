# 📄 Lab Notes: AWS Security Controls Lab

- **Author**: Andre Patterson 
- **Date**: July 2026 
- **Environment**: AWS Free Tier | eu-west-2 (London)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Lab 1 - IAM (Identity and Access Management) {#lab -1}

**Objective**: Create a user, group, and apply least privilege permissions using AWS managed policies.




**Step 1- IAM Dashboard (Root Account)**
![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/eaaf2b184a5723c8bf841e7ff3a7c800b9e2be25/screenshots/Lab%201-iam/lab1-MFA%2001.png)

Opened the IAM dashboard as root user. The console immediately displayed security recommendations - a warning that the root account has no MFA enabled and that IAM best practices are not yet applied.

**What this shows**:

- Root account has unrestricted access to every AWS service and resource
- AWS flags missing security controls on the dashboard automatically
- This is the starting point — before any security controls are applied

Security note: Root accounts should never be used for day-to-day tasks. They exist only for initial setup and emergency access.








**Step 2- IAM Service Search**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/7537deac8d1ac9c69ffeda3bc2ada7e925ee7f3b/screenshots/Lab%201-iam/01-search-iam-service.png)
Located Identity and Access Management from the AWS service search bar above.








**Step 3- IAM Features Overview**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/7537deac8d1ac9c69ffeda3bc2ada7e925ee7f3b/screenshots/Lab%201-iam/02-open-iam-users.png)
Reviewed the IAM service feature set: Users, Groups, Roles, Policies and Identity Providers. This gives the full picture of how AWS structures identity and access management.










**Step 4- IAM Users List**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/eaaf2b184a5723c8bf841e7ff3a7c800b9e2be25/screenshots/Lab%201-iam/03-start-create-user.png)
Opened the IAM Users section before creating the new user. Best practice is one named user per person and never share credentials between individuals.











**Step 5- Creating User: andre-soc-analyst**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/eaaf2b184a5723c8bf841e7ff3a7c800b9e2be25/screenshots/Lab%201-iam/04-specify-user-details.png)
Created IAM user with the following user detail configurations:

|       **Setting**             |                 **Value**                    |
|-------------------------------|----------------------------------------------|
|User name                      |            **andre-soc-analyst**             |
|AWS Management Console Access  |                  Enabled                     |
|Password type                  |              Custom password                 | 
|Require password reset         |                 Enabled                      |     


**Why force password reset**: The initial password is set by the administrator. Forcing a reset on first login ensures the user sets their own credentials — the admin never needs to know the final password.








**Step 6- Group Creation Error**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/778f46d27150c2ded07f6e12c686b97930552cc8/screenshots/Lab%201-iam/05-group-name-validation-error.png)

**Error encountered**:
"User group was not created. The specified value for groupName is invalid. It must contain only alphanumeric characters and/or the following: +=,@_-"

**Cause**: The group name SOC-Analysts triggered AWS character validation rules.

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/778f46d27150c2ded07f6e12c686b97930552cc8/screenshots/Lab%201-iam/06-create-soc-analysts-group.png)
**Fix**: Renamed to SOC_Analysts using an underscore.

Learning point: AWS naming conventions vary by service and resource type. Always check character restrictions. This is a common real world mistake even for experienced engineers.







**Step 7- Creating Group: SOC_Analysts with SecurityAudit Policy**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/778f46d27150c2ded07f6e12c686b97930552cc8/screenshots/Lab%201-iam/06-create-soc-analysts-group.png)

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/778f46d27150c2ded07f6e12c686b97930552cc8/screenshots/Lab%201-iam/07-group-created-securityaudit.png)

Successfully created group **SOC_Analysts** and attached the **SecurityAudit** AWS managed policy.

**Why SecurityAudit**:

- Provides read-only access to security services: CloudTrail, IAM, Config, GuardDuty, S3 bucket policies, Security Hub
- Mirrors what a real SOC analyst account looks like in a production environment
- Analyst can view security data and investigate alerts but cannot modify configurations
- Follows least-privilege: only grant access the role actually requires







**Step 8- Review Before Creating**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/778f46d27150c2ded07f6e12c686b97930552cc8/screenshots/Lab%201-iam/08-review-user-and-group.png)

Reviewed the full user configuration summary before confirming:

- Username: **andre-soc-analyst** ✅
- Group: **SOC_Analysts** ✅
- Policy inherited: **SecurityAudit** ✅







**Step 9- User Created Successfully**
![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/778f46d27150c2ded07f6e12c686b97930552cc8/screenshots/Lab%201-iam/09-user-created-successfully.png)

User **andre-soc-analyst** confirmed created. Console shows:

- User ARN
- Sign-in URL for the account

Lab 1 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Lab 2 - KMS (Key Management Service) {#lab-2}

**Objective**: Create a customer-managed symmetric encryption key, assign administrators and users, and confirm the key is active.





**Step 1- KMS Service Landing Page**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/01-search-kms-service.png)

Navigated to AWS Key Management Service. KMS allows creation and management of cryptographic keys used to encrypt data across AWS services including S3, EBS, RDS, CloudTrail, and SSM Parameter Store.

**Customer-managed vs AWS-managed keys**:
- AWS-managed: AWS controls the key policy -  you cannot audit or restrict who uses them
- Customer-managed: You define exactly who can administer and use the key - full auditability







**Step 2- KMS Dashboard**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/02-start-create-key.png)

Reviewed the KMS customer-managed keys dashboard, the central location where all lab encryption keys will be managed.







**Step 3- Configure Key Type**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/03-configure-symmetric-key.png)

Selected key configuration:

|       **Setting**             |                 **Value**                    |                **Reason**                               |
|-------------------------------|----------------------------------------------|---------------------------------------------------------|
|Key type                       |                 Symmetric                    |   Same key for encrypt and decrypt — correct for S3     |
|Key usage                      |             Encrypt and Decrypt              |   Required for S3 SSE-KMS and CloudTrail log encryption |







**Step 4- Add Key Alias and Label**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/04-set-key-alias.png)

Set the key alias and description:

**Alias**: soc-lab-key
Description: Customer-managed key for SOC security lab

**Why aliases matter**: KMS key ARNs contain long strings. An alias makes the key human-readable and easy to reference when configuring S3, CloudTrail or SSM.







**Step 5- Assign Key Administrators**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/05-select-key-administrator.png)

Assigned **andre-soc-analyst** as key administrator.

**Key administrator permissions**:

- Can enable, disable, and schedule key deletion
- Can update the key policy
- Cannot use the key to encrypt or decrypt data by default

Separation of duties: The person who manages the key is separate from the person who uses the key, this is a core security control in enterprise environments.







**Step 6-Assign Key Users**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/06-select-key-user.png)

Assigned **andre-soc-analyst** as key user.

**Key user permissions**:

- Can call **kms:Encrypt**, **kms:Decrypt**, **kms:ReEncrypt**, **kms:GenerateDataKey**, **kms:DescribeKey**
- These permissions are required for S3 and CloudTrail to encrypt and decrypt objects using the key
- In production: AWS service roles (S3, CloudTrail) would be assigned here, not IAM users






**Step 7-Assign Key Users**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/aa0e63dc285f36ae4128aceb5f02d57b58cc0946/screenshots/Lab%202-kms/07-review-key-configuration.png)

Verified the immutable key type, usage, alias, and permission selections via Key Configuration.







**Step 8-Key Successfully Created and Listed**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/8531ffc3b487c7d977d729ec70ed1d99eb24effb/screenshots/Lab%202-kms/08-key-created-successfully.png)

Key **soc-lab-key** confirmed in the KMS customer-managed keys list:

Alias: soc-lab-key ✅
Status: Enabled ✅
Key type: Symmetric ✅
Key Usage: Encrypt and Decrypt ✅







**Step 9-Key Successfully Created and Listed**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/8531ffc3b487c7d977d729ec70ed1d99eb24effb/screenshots/Lab%202-kms/09-review-key-policy-clean.png)

Reviewed the auto-generated key policy. The JSON policy explicitly defines:

- Root account emergency access
- Administrator actions (Create, Delete, Disable, Rotate)
- User actions (**Encrypt**, **Decrypt**, **GenerateDataKey**, **DescribeKey**)

SOC relevance: Key policies are audit artifacts,  they prove who had access to encrypted data and when. In an incident investigation, KMS CloudTrail logs show every encryption/decryption event.

Lab 2 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Lab 3 — S3 Bucket with Encryption {#lab-3}

**Objective**: Create a private, encrypted S3 bucket using the **soc-lab-key** KMS key, upload a file and verify encryption is applied to all objects.




**Step 1-S3 Service Overview**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/42e7e968f29dab2609e6fba463bf6b5a0338fa54/screenshots/Lab%203-s3-encryption/01-search-s3-service.png)

Navigated to Amazon S3. S3 is the most widely used object storage service in AWS and one of the most frequently misconfigured. **Public** S3 buckets and **unencrypted** data have been responsible for some of the largest cloud data breaches in history.

Common S3 misconfigurations that cause breaches:

- Public read access left enabled on sensitive buckets
- No default encryption - data stored in plaintext
- No access logging - no audit trail of who accessed what







**Step 2-Create Bucket Configuration**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/42e7e968f29dab2609e6fba463bf6b5a0338fa54/screenshots/Lab%203-s3-encryption/02-start-create-bucket.png)

Created bucket with full security settings applied from the start:
                                                                                                                                       

|       **Setting**             |                 **Value**                    |                **Reason**                               |
|-------------------------------|----------------------------------------------|---------------------------------------------------------|
|Bucket name                    |       andre-soc-security --------- (private) |           Globally unique, descriptive                  |
|Region                         |             eu-west-2 (London)               |          Data residency - keeps data in UK              |
|Block all public access        |                 Enabled ✅                   |                                                         |
|Default encryption             |                  SSE-KMS                     |     Encrypts every object automatically at rest         |
| KMS key                       |              **soc-lab-key**                 |       Customer-managed - full key control               |


I have internationally shortened my bucket name displayed above for security posture.







**Step 3-Bucket Created Successfully**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/ce348b338fe0266fd1495b37b90a0388117240bf/screenshots/Lab%203-s3-encryption/03-bucket-created.png)

Bucket appears in the S3 console. No public access indicator, the bucket is fully private.







**Step 4-Upload File: andre-soc-analyst.txt**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/ce348b338fe0266fd1495b37b90a0388117240bf/screenshots/Lab%203-s3-encryption/04-add-test-object.png)

Uploaded test file **andre-soc-analyst.txt** to the encrypted bucket.

**What occurs during upload**: Because SSE-KMS is set as default encryption, AWS automatically calls KMS to generate a data encryption key, encrypts the file before writing it to disk, and stores only the encrypted version. The plaintext data never touches S3 storage unencrypted.







**Step 5-Bucket Properties and SSE-KMS Encryption Confirmed**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/ce348b338fe0266fd1495b37b90a0388117240bf/screenshots/Lab%203-s3-encryption/06-encryption-arn-redacted.png)

Navigated to the bucket Properties tab to verify encryption settings are applied at bucket level.

In addition, Bucket Properties page confirms encryption is active:

|       **Property**            |                      **Value**                             |
|-------------------------------|------------------------------------------------------------|
|Encryption type                |  Server-side encryption with AWS KMS keys (SSE-KMS) ✅     |
|Bucket Key                     |                    Enabled                                 |







**Step 6- txt file Upload Success**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/ce348b338fe0266fd1495b37b90a0388117240bf/screenshots/Lab%203-s3-encryption/07-upload-succeeded.png)

Upload confirmed - **andre-soc-analyst.txt** successfully stored in the encrypted bucket. The file is now at rest, encrypted with **soc-lab-key**.










**Step 7- Confirmation of**

![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/ce348b338fe0266fd1495b37b90a0388117240bf/screenshots/Lab%203-s3-encryption/09-confirm-block-public-access.png)

**Why block public access at bucket level**: Even if an object-level ACL accidentally grants public access, the bucket-level block prevents it from taking effect.

Lab 3 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Troubleshooting Log

**Issue 1 - IAM Group Name Invalid (Lab 1)**

- **Error**: The specified value for **groupName** is invalid 
- **Cause**: Group name contained characters outside the allowed set
- **Fix**: Changed SOC-Analysts to **SOC_Analysts**
- **Lesson**: Check AWS naming constraints before creating any resource as they differ by service

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Resources Created - Full Summary

|       **Resource**            |                 **Name**                     |                **Purpose**                              |
|-------------------------------|----------------------------------------------|---------------------------------------------------------|
|IAM User                       |            andre-soc-security                |           Least-privilege SOC analyst account           |
|IAM Group                      |               SOC_Analysts                   |        Group with **SecurityAudit** read-only policy    |
|IAM Policy                     |           **SecurityAudit** (managed)        |           Read-only access to security services         |
|KMS key                        |              **soc-lab-key**                 |     	    Customer-managed encryption key                |
