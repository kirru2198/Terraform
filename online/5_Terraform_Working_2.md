# 🧱 **Terraform High-Level Workflow (with AWS)**

---

## 📌 What is Terraform?

- **Terraform** is an **Infrastructure as Code (IaC)** tool developed by **HashiCorp**.
- It allows you to **codify infrastructure**: VPCs, EC2 instances, IAM users, etc.
- The configuration files are written in **HCL (HashiCorp Configuration Language)**, which is close to JSON and very easy to read.

---

## 📁 File Structure & Code Format

- Terraform code files always have the **`.tf`** extension.
- You define:
  - **Provider**: Cloud platform (e.g., AWS)
  - **Resources**: What to create (e.g., EC2, VPC)
  - **Authentication profile**: How Terraform is authorized to deploy.

---

## ⚙️ Example Directory Structure

```bash
mkdir my-tf-project
cd my-tf-project
touch providers.tf
touch ec2.tf
```

---

## 🧾 Sample `providers.tf` File

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region  = "us-east-1"
  profile = "default"
}
```

> 🔐 `profile = "default"` refers to the AWS CLI profile you configured with access/secret keys.

---

## 📦 What is a **Provider**?

- A **provider** is a **bridge between Terraform and a cloud platform**.
- It helps **translate Terraform code into actual API calls** to services like AWS, Azure, or GCP.
- You must declare the provider in your code.

### 🔸 Types of Providers:

- **Official** – Maintained by HashiCorp (e.g., `aws`, `azurerm`)
- **Partner** – Maintained by third-party vendors
- **Community** – Open-source contributors

> 📌 Example: To create resources in AWS, you need the **AWS provider**.

---

## 🔍 Example Resource File `ec2.tf`

```hcl
resource "aws_instance" "my_ec2" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
  tags = {
    Name = "MyEC2Instance"
  }
}
```

---

## 🔄 Terraform Command Workflow

### 1. **`terraform init`**
- Initializes the Terraform working directory.
- Downloads all required **provider plugins** (e.g., AWS plugin).
- Creates a `.terraform` directory.

### 2. **`terraform validate`**
- Checks the syntax of your Terraform code.
- Not mandatory, but **good practice**.

### 3. **`terraform plan`**
- Shows a **preview** of what will happen.
- Does not make any real changes.
- Helps you verify the changes before applying.

### 4. **`terraform apply`**
- Actually applies the code to provision the resources.
- Requires confirmation (`yes`) or can be auto-approved using `-auto-approve`.

> 🔁 **Code ➝ Terraform ➝ Provider Plugin ➝ Cloud Provider API ➝ Resources Created**

---

## 🔐 Role of AWS CLI in Terraform

- AWS CLI must be installed and configured with a valid **IAM user's access keys**.
- These credentials authorize Terraform to **authenticate and interact** with AWS.

### AWS CLI Setup Recap:

```bash
aws configure
```

Input:
- AWS Access Key ID
- AWS Secret Access Key
- Default Region (`ap-south-1`)
- Default Output Format (`text`, `json`, etc.)

This sets up the **default profile** in:

- `~/.aws/config`
- `~/.aws/credentials`

---

## 🔗 How Terraform Uses AWS CLI Profile

In your Terraform `provider` block:

```hcl
provider "aws" {
  region  = "ap-south-1"
  profile = "default"
}
```

Terraform will **read the credentials** stored in the default profile and **use them to authenticate requests** to AWS.

---

## 🔄 Summary Flow

```plaintext
1. Write .tf files with provider + resources
2. Run `terraform init` → Downloads provider plugin
3. Run `terraform validate` → Checks syntax
4. Run `terraform plan` → Dry run
5. Run `terraform apply` → Executes and creates resources
```
