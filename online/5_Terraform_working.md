https://registry.terraform.io/
---

## ✅ Terraform Basics – High-Level Understanding

---

### 1. **Terraform Overview**
- **Terraform** is an open-source Infrastructure as Code (IaC) tool used to provision and manage cloud resources using code.
- Terraform files use the `.tf` extension and are written in **HCL (HashiCorp Configuration Language)**.
- HCL is **human-readable** and close to JSON. Terraform can also accept JSON format (`.tf.json`), though HCL is preferred.

---

### 2. **Terraform File Structure**
- The typical Terraform project contains multiple `.tf` files:
  - `providers.tf`: Contains provider configuration.
  - `main.tf`: Contains resource definitions.
  - `variables.tf`: Declares input variables (optional).
  - `outputs.tf`: Declares output values (optional).

---

### 3. **Terraform Workflow (Commands)**

| Step | Command | Description |
|------|---------|-------------|
| 1️⃣ | `terraform init` | Initializes the working directory. Downloads required provider plugins from the [Terraform Registry](https://registry.terraform.io). |
| 2️⃣ | `terraform validate` | (Optional) Checks the syntax and validity of the configuration. |
| 3️⃣ | `terraform plan` | Shows what actions Terraform **will perform** without making any changes (dry run). |
| 4️⃣ | `terraform apply` | Applies the configuration and provisions the resources in the specified cloud provider. |

---

### 4. **Providers in Terraform**

- **Definition**: A provider is a plugin that Terraform uses to **interact with APIs** of different platforms (e.g., AWS, Azure, Google Cloud, Kubernetes).
- Examples:
  - `hashicorp/aws` → For AWS
  - `hashicorp/azurerm` → For Azure
  - `hashicorp/google` → For GCP
  - `hashicorp/kubernetes` → For Kubernetes

- **Types of Providers**:
  - **Official**: Built and maintained by HashiCorp.
  - **Partner**: Built by cloud providers in partnership with HashiCorp.
  - **Community**: Open-source and created by individual contributors.

---

### 5. **How Providers Work**

- Providers translate Terraform code into API requests that the target cloud platform understands.
- Example:
  - Terraform AWS Provider will **generate AWS-compatible API calls**.
  - These API requests include:
    - **What** to create (resource type like EC2, S3, VPC, etc.)
    - **Where** to create (region)
    - **Who** is making the request (authenticated via credentials)

---

### 6. **Authentication with Providers**
- Terraform uses **AWS CLI credentials** for authentication.
- Credentials are stored in `~/.aws/credentials` (e.g., `[default]` profile).
- You define the profile in your Terraform code:

> in order to find see the profile use the following commands:
> ```bash
> - pwd
> - cd .aws
> - ls
> - config  credentials  # This is the output of ls command
> cat config
> [default] # what AWS credentials === adminstrator access
> ```

```hcl
provider "aws" {
  region  = "us-east-1"
  profile = "default"
}
```

- This profile must have appropriate IAM permissions (e.g., AdminAccess).

---

### 7. **Typical File Example (`main.tf`)**

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

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "MyVPC"
  }
}
```

---

### 8. **Resource Creation Example (VPC)**
- Just like using AWS Console where you provide VPC name and CIDR block, in Terraform, you do the same thing using HCL.
- After writing this code:
  - Run `terraform init` → Downloads plugins.
  - Run `terraform plan` → Shows what will be created.
  - Run `terraform apply` → Actually creates the VPC in AWS.

---

### 9. **Summary of Flow**
```
1. Install Terraform & AWS CLI
2. Configure AWS CLI with user credentials
3. Write Terraform code (.tf files)
4. Run:
   - terraform init
   - terraform validate
   - terraform plan
   - terraform apply
5. Terraform sends API requests using downloaded plugins
6. Resources get provisioned in the cloud
```

<img width="689" alt="image" src="https://github.com/user-attachments/assets/4e169721-c3e1-41fa-a12a-ce01bdfd2bad" />

<img width="465" alt="image" src="https://github.com/user-attachments/assets/fc59fc03-3e5f-424b-a4de-24f54a77ee64" />


