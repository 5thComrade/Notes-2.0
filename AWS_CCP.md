[title]: #F24C4C
[note]: #F5F0BB
[best]: #FF9F29

# AWS Certified Cloud Practitioner

### **<span style="color:#F24C4C">IAM - Identity and Access Management</span>**

> AWS Identity and Access Management (IAM) provides fine-grained access control across all of AWS. With IAM, you can specify who can access which services and resources, and under which conditions. With IAM policies, you manage permissions to your workforce and systems to ensure least-privilege permissions.

Access can be applied to:

- Users - End users(people)
- Groups - Collection of users under one set of permissions
- Roles - Assigned to AWS resources, specifying what the resource (EC2) is allowed to access on another resource(S3)
- Policies (JSON documents): Defines what each of the above can and cannot do. Note: IAM has predefined managed policies.

IAM supports Multi-factor authentication for users
MFA device options

- Virtual MFA device(Google Authenticator, Authy)
- Universal 2nd Factor(U2F) Security device (YubiKey by Yubico) - this is a physical key
- Hardware Key Fob MFA device
- Hardware Key Fob MFA device for AWS GovCloud(US)

How to access AWS?

- AWS Management Console (protected by password + MFA)
- AWS Command Line Interface: CLI (protected by access keys)
- AWS Software Development Kit: SDK - for code (protected by access keys)

_<span style="color:#F5F0BB">If you have permission issues (like I have on my company laptop), you can use AWS CloudShell. 
  This is a browser simulation of a command prompt which has aws cli, node and other things pre-installed. 
  You can practice your aws cli skills on it.</span>_

Policy documents must have a version and a statement in the body.
The statement must consist of

- Effects (allow/deny)
- Actions (which action to allow/deny such a \* for all actions)
- Resources (affected resources such as \* for all resources)

IAM Security tools

- IAM Credentials Report (account-level)
- IAM Access Advisor (user-level)

**<span style="color:#FF9F29">IAM Best Practices</span>**

- Follow the least privilege principle, don't give more permissions than a user needs.
- Set up and manage password policy and password rotation policy for IAM users
- Recommended that root account is not used for login, and should be secured with Multi-factor Authentication (MFA)
- One IAM User per person ONLY
- One IAM Role per Application
- Assign users to groups and assign permissions to groups
- Use and enforce MFA
- Audit permissions of your account with IAM Credentials Report

---
