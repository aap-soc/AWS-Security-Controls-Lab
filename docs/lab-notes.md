## 📄 Lab Notes — AWS Security Controls Lab

- **Author**: Andre Patterson 
- **Date**: July 2026 
- **Environment**: AWS Free Tier | eu-west-2 (London)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Lab 1 - IAM (Identity and Access Management) {#lab -1}

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
