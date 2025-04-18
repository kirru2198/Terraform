### How Terraform Works? 

how AWS works? 

Have you ever thought about what happens behind the scenes in AWS? Internally, everything is driven by APIs. Whether it's creating a VM, a database, or a bucket—you're interacting with APIs. In fact, everything in AWS or any cloud provider is basically an API under the hood.

<img width="515" alt="image" src="https://github.com/user-attachments/assets/52759f69-0ac6-4515-91c6-4a3971ab0ebe" />

---

## 🔍 How AWS Works Internally — Explained Step by Step

### ✅ 1. **What Is an API in AWS?**
An **API (Application Programming Interface)** is the core of how AWS operates.  
Everything you do in AWS — whether through the console, CLI, or SDK — eventually becomes an **API call** to an AWS service.

---

### 🧑‍💻 2. **Ways to Create an EC2 Instance**
An IAM user can create an EC2 instance using:

- **AWS Console** (GUI)
- **AWS CLI** (Command Line Interface)
- **AWS SDKs** (Using Python, Java, etc.)

Regardless of the method, each action results in an **API request** being sent to AWS.

---

### 📤 3. **What Happens Behind the Scenes?**

When you initiate a request to create an EC2 instance:

1. You provide required info (AMI, instance type, VPC, key pair, etc.).
2. You hit the **"Launch Instance"** button.
3. Your request is converted into an **API call**.
4. This API call is sent to an **AWS API Gateway** (EC2-specific endpoint).
5. The request includes:
   - Your identity (IAM user)
   - Action you want to perform
   - Authentication credentials (via access keys, session token, etc.)

---

### 🔐 4. **Authentication & Authorization**

The API call goes through two checks:

- **Authentication**: Confirms **who you are** (valid IAM user).
- **Authorization**: Checks **what you are allowed to do** based on attached IAM policies.

#### Example:
- If the IAM user has an `EC2ReadOnly` policy:  
  ❌ Cannot create an instance (you’ll get an “Unauthorized” error).
- If the IAM user has `EC2FullAccess` or a custom policy with `ec2:RunInstances`:  
  ✅ The instance will be created successfully.

---

### ⚙️ 5. **Result**
If authenticated and authorized:
- The request is processed.
- An EC2 instance is launched.
- You see it running in the AWS Console (or CLI/SDK output).

---

## 📌 Summary

> AWS is API-driven. Every action—whether via Console, CLI, or SDK—is an API call. That API call must be **authenticated** and **authorized** before it's executed. Once approved, AWS provisions the resource, such as an EC2 instance, using backend services via API Gateways.

---

![image](https://github.com/user-attachments/assets/b458a8e1-a136-4607-a4c7-a2ae5bf2fa21)

