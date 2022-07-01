# AWS Certified Cloud Practitioner

### **IAM - Identity and Access Management**

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

_If you have permission issues (like I have on my company laptop), you can use AWS CloudShell. This is a browser simulation of a command prompt which has aws cli, node and other things pre-installed. You can practice your aws cli skills on it._

Policy documents must have a version and a statement in the body.
The statement must consist of

- Effects (allow/deny)
- Actions (which action to allow/deny such a \* for all actions)
- Resources (affected resources such as \* for all resources)

IAM Security tools

- IAM Credentials Report (account-level)
- IAM Access Advisor (user-level)

**IAM Best Practices**

- Follow the least privilege principle, don't give more permissions than a user needs.
- Set up and manage password policy and password rotation policy for IAM users
- Recommended that root account is not used for login, and should be secured with Multi-factor Authentication (MFA)
- One IAM User per person ONLY
- One IAM Role per Application
- Assign users to groups and assign permissions to groups
- Use and enforce MFA
- Audit permissions of your account with IAM Credentials Report

---

### **EC2 - Elastic Compute Cloud**

> Secure and resizable compute capacity for virtually any workload. Infrastructure as a Service.

Infrastructure setup on AWS

- Renting virtual machines (EC2)
- Storing data on virtual drives (EBS - Elastic Block Store)
- Distributing load across machines (ELB - Elastic Load Balancer)
- Scaling the services (ASG - Auto-scaling Group)

By default, an EC2 machine comes with:

- A private IP for internal AWS network
- A public IP for WWW

If your machine is stopped and then restarted, the public IP will change.

**EC2 User Data**

- It is possible to bootstrap our instances using an EC2 User data script
- Bootstrapping means launching commands when a machine starts
- That script is only run once at the instance first start
- Purpose: Ec2 data is used to automated boot tasks such as:
  - Installing updates
  - Installing software
  - Downloading common files from the internet

**Launch an EC2 Instance**

- Choose an Amazon Machine Image(AMI)
- Choose an Instance type
- Configure instance details (user data, iam roles)
- Add Storage
- Add tags
- Configure Security Group (ssh, http etc rules to your instance)

**EC2 Meta Data**

- Convenient way to obtain information about our EC2 instance from the running instance
- Metadata = Info about EC2 instance
- Metadata of an instance contains a lot of information about that instance
  - ami-id
  - instance id
  - hostname
  - mac id
  - public/private ips
  - security groups
  - etc

**EC2 Instance Types**

- General Purpose  
  Example: t2.micro  
  Great for a diversity of workloads such as web servers or code repositories. Balance between compute, memory and networking.
- Compute Optimized  
  Example: c5.xlarge  
  Great for compute-intensive tasks that require high performance processors: batch processing workloads, media transcoding, high performance web servers,
  high performance computing, scientific modeling, machine learning, dedicated gaming servers
- Memory Optimized  
  Example: r5a.4xlarge  
  Fast performance for workloads that process large data sets in memory.
  Use Cases: High performance relational/non-relational databases, distributed web scale cache stores, in-memory databases optimized for BI(business intelligence)
  applications performing real-time processing of big unstructured data
- Storage Optimized
  Example: d3.8xlarge
  Great for storage-intensive tasks that require high, sequenial read and write access to large data sets on local storage.
  Use cases: High freguency online transaction processing (OLTP) systems, Relational and NoSQL databases, cache for in-memory databases(redis), data warehousing
  applications, distributed file systems.

**EC2 Security Groups**
Security groups control how the traffic is allowed into or out of your EC2 instances.
Security groups are acting as a firewall on EC2 instances.
They regulate access to ports, authorized IP ranges IPv4 and IPv6, control the inbound and outbound network.

Classic Ports to know

- Port 22 = SSH (Secure Shell) log into a linux instance
- Port 21 = FTP (File Transfer Protocol) upload files into a file share
- Port 22 = SFTP (Secure File Transfer Protocol) upload files using SSH
- Port 80 = HTTP access unsecured websites
- Port 443 = HTTPS access secured websites
- Port 3389 = RDP (Remote Desktop Protocol) log into a Windows instance

The easiest way to connect to your linux instance is EC2 Instance Connect using the web browser.
Instance connect uses the SSH protocol to connect to your EC2 instance and opens a terminal in your browser (very cool!).

_**DO NOT RUN aws configure command in your ec2 instance and enter your IAM credentials, instead attach an IAM Role to the EC2 instance.**_

**EC2 Instance Purchasing Options**

- On Demand Instances: short workload, predictable pricing
- Reserved Instances: long workloads (>= 1 year)
- Convertible Reserved Instances: long workloads with flexible instances
- Scheduled Reserved Instances: launch within time window you reserve
- Spot Instances: short workloads, for cheap, can lose instances (less reliable)
- Dedicated Instances: no other customers will share your hardware
- Dedicated Hosts: book an entire physical server, control instance placement

On-Demand Instance:

- Pay for what you use
- Has the highest cost but no upfront payment
- No long term commitment
- Recommended for short-term and un-interrupted workloads, where you can’t predict how the application will behave

Reserved Instances:

- Up to 75% cheaper compared to On-demand
- Pay upfront
- Reservation period can be 1 or 3 years
- Reserve a specific instance type

Convertible Reserved Instances:

- These are reserved instances but now with additional flexibility, one can change the EC2 instance type.
- Up to 54% discount compared to on-demand.

Scheduled Reserved Instances:

- Launch within time window you reserve. Example: Every Tuesday between 1PM - 3PM

Spot Instances:

- Can get a discount of up to 90% compared to On-demand
- You bid a price and get the instance as long as its under the price
- Price varies based on offer and demand
- Spot instances are reclaimed within a 2 minute notification warning when the spot price goes above your bid
- Not great for critical jobs or databases

Dedicated Instances:

- Instances running on hardware that’s dedicated to you
- No control over instance placement

Dedicated Hosts:

- Physical dedicated EC2 server for your use
- Full control of EC2 Instance placement
- Visibility into the underlying sockets / physical cores of the hardware
- Allocated for your account for a 3 year period reservation
- More expensive
- Useful for software that have a complicated licensing model (Bring your own License)
- Or for a companies that have strong regulatory or compliance needs

**EC2 Shared Responsibility Model**  
AWS is responsible for infrastructure, replacing faulty hardware, compliance validation
User is responsible for security groups rules, operating-system patches and updates, software and utilities installed on the EC2 instance, IAM roles assigned to the ec2 instance, data security on your instance.

---

### **EC2 - Storage**
