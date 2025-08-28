# AWS Overview and Security

---

## AWS: Data Center, Region, and Availability Zone (AZ)

### 1. Data Center
- A **data center** is a physical building that contains servers, storage, networking, cooling, and power systems.  
- Think of it as the factory where the actual computing happens.  
- AWS has many data centers around the world.

### 2. Region
- A **Region** is a large geographical area (for example: `us-east-1` = North Virginia, `eu-west-1` = Ireland).  
- Each region contains **multiple Availability Zones**.  
- You usually choose a region close to your users (for low latency) or one that complies with data regulations (e.g., keeping data in the EU).

### 3. Availability Zone (AZ)
- An **Availability Zone (AZ)** is a group of one or more data centers within the same region.  
- Each AZ is isolated from the others (different power, cooling, and physical separation) but connected with high-speed, low-latency links.  
- Most regions have at least 2–3 AZs.

---

### How They Work Together?
- **Data Center** → A single physical site with servers.  
- **AZ** → One or more data centers grouped together.  
- **Region** → A set of multiple AZs in the same geographic area.  

This structure allows AWS to provide:
- **High Availability**: Deploy across multiple AZs so that if one fails, others keep running.  
- **Global Reach**: Deploy in multiple regions to serve users worldwide.

---

## AWS Region Considerations

When selecting an AWS Region, you should consider:

### 1. Compliance
- Some data must stay in specific countries due to **legal or regulatory requirements**.  
- Example: GDPR in the EU → choose an EU region.

### 2. Latency
- Choose a region **close to your users** for faster response times.  
- Shorter physical distance = lower latency.

### 3. Pricing
- **Costs vary by region** (compute, storage, data transfer).  
- Example: `us-east-1` is often cheaper than some Asia-Pacific regions.

### 4. Service Availability
- Not all AWS services are offered in every region.  
- Some new services launch in limited regions first.

**Summary:**  
Pick your AWS Region based on **legal compliance, proximity (latency), cost, and available services.**

---

## Scope of AWS Services
Depending on the AWS Service you use, your resources are either deployed at the **AZ**, **Region**, or **Global** level.  

- **Region-scoped services** → You only select the Region, and AWS automatically increases durability and availability.  
- **AZ-scoped services** → You specify the AZ, and you are responsible for handling durability and high availability (e.g., replicating across multiple AZs).  

---

## Maintain Resiliency
To keep your application available, you need to maintain high availability and resiliency.  

- Best practice: Use **Region-scoped, managed services**, since resiliency is built-in.  
- If not possible: Replicate workloads across **multiple AZs** (at least 2).  
- This ensures that if one AZ fails, another AZ can take over the traffic.

---

## Ways to Interact with the AWS API
Every action you make in AWS is an API call

- **AWS Management Console**  
- **AWS Command Line Interface (CLI)**  
- **AWS SDKs (Software Development Kits)**

---
## Authentication vs Authorization

- **Authentication**: Verifies *who you are*.  
  - Example: logging in with email & password, tokens, or biometrics.  
  - Answers: *“Are you who you say you are?”*

- **Authorization**: Defines *what you can do*.  
  - Example: permissions to read, edit, delete, or create resources in AWS.  
  - Answers: *“What actions can you perform?”*

## AWS Root User

- The **root user** is the first identity created with an AWS account.  
- It uses the account’s email and password to sign in.  
- Has **full access** to all AWS services and resources.
---
# IAM (Identity and Access Management)

- **IAM** is a web service for managing access to your AWS account and resources.  
- Provides a centralized view of **who/what can sign in (authentication)** and **what they can do (authorization)**.  
- Allows sharing access without exposing root credentials.  
- Supports **granular permissions** (e.g., giving a user read-only access to specific resources).  

An IAM user represents a person or service that interacts with AWS


## IAM Groups

- An **IAM group** is a collection of IAM users.  
- All users in a group **inherit the group’s permissions**.  
- Best practice for organizing users by role (e.g., *developers*, *security*, *admins*).  
- Makes it easy to add/remove/change user permissions at scale.  
- Key points:  
  - Groups can have many users.  
  - Users can belong to many groups.  
  - Groups cannot contain other groups.  
- New IAM identities start with **no permissions by default**. Access is granted using **IAM policies**.  


## IAM Policies

- **IAM policies** define permissions for users, groups, and roles.  
- When a request is made, AWS checks all attached policies to decide if it is **allowed or denied**.  
- Example: a developer in the *developers group* inherits group policies + their own user policies.  

---
# IAM BEST PRACTICES

## Lock Down the AWS Root User
- Do **not** share root credentials.  
- Delete root user access keys.  
- Enable **MFA** on the root account.  

## Principle of Least Privilege
- Grant only the **minimum permissions** needed for a task.  
- Add more permissions only when necessary.  

## Use IAM Appropriately
- IAM secures access to **AWS resources** within one account.  
- Not for website user sign-in/sign-up or OS/network security.  

## Use IAM Roles When Possible
- Roles provide **temporary credentials** (15 min – 36 hrs).  
- Easier to manage than long-term user credentials.  

## Consider Using an Identity Provider (IdP)
- Centralizes identity management for organizations.  
- Supports federation via AWS IAM Identity Center or third-party IdPs.  
- Example: manage a single employee identity across multiple AWS accounts.  

## Consider AWS IAM Identity Center
- Provides a **single sign-on (SSO) portal** for multiple AWS accounts and apps.  
- Syncs users/groups from third-party IdPs.  
- Separates identity management from AWS for stronger security.  