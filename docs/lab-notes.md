# 📄 Lab Notes — AWS Security Controls Lab

- **Author**: Andre Patterson 
- **Date**: July 2026 
- **Environment**: AWS Free Tier | eu-west-2 (London)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Lab 1 - IAM (Identity and Access Management) {#lab -1}

**Objective**: Create a user, group, and apply least privilege permissions using AWS managed policies.




**Step 1- IAM Dashboard (Root Account)**
![image alt](https://github.com/aap-soc/AWS-Security-Controls-Lab/blob/eaaf2b184a5723c8bf841e7ff3a7c800b9e2be25/screenshots/Lab%201-iam/lab1-MFA%2001.png)

Opened the IAM dashboard as root user. The console immediately displayed security recommendations  - a warning that the root account has no MFA enabled and that IAM best practices are not yet applied.

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
