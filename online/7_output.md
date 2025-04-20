# 🌐 Terraform Project: VPC Setup (11th December)

## 📁 Project Directory Structure

```bash
11th-December/
├── provider.tf
├── vpc.tf
├── outputs.tf
└── (terraform.tfstate - auto-generated)
```

> **Note:** You can visualize the structure using the `tree` command (if installed):  
> `tree .`

---

## 📄 File: `provider.tf`

This file declares the Terraform **provider** to be used.

```hcl
provider "aws" {
  profile = "default"
  region  = "ap-south-1"
}
```

**Explanation:**

- `provider "aws"`: Defines AWS as the cloud provider.
- `profile`: Uses the AWS CLI default profile (ensure it's configured).
- `region`: Specifies the AWS region (Mumbai - `ap-south-1`).

---

## 📄 File: `vpc.tf`

This file contains the actual resource definitions for creating a VPC setup.

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "main-vpc"
  }
}

# Additional resources like subnets, IGWs, route tables etc. go here
```

**Explanation:**

- `aws_vpc.main`: Creates a VPC with the specified CIDR block.
- Add more resources as needed (subnets, route tables, gateways etc.).

---

## 📄 File: `outputs.tf`

This file collects and displays important output values after resources are created.

```hcl
output "vpc_id" {
  value       = aws_vpc.main.id
  description = "The ID of the main VPC"
}

output "public_subnet_id" {
  value       = aws_subnet.public.id
  description = "The ID of the public subnet"
}

output "private_subnet_id" {
  value       = aws_subnet.private.id
  description = "The ID of the private subnet"
}
```

**Explanation:**

- `output`: A Terraform keyword to print resource values post creation.
- This helps easily retrieve values like VPC ID, subnet IDs etc.
- **Best practice** when multiple resources are created.

---

## ✅ Commands Execution Flow

### 🔹 1. Initialize the Terraform Project

```bash
terraform init
```

**Purpose:**  
Downloads the necessary provider plugins.  
> Terraform checks `provider.tf`, downloads `hashicorp/aws`.

---

### 🔹 2. Validate (Optional)

```bash
terraform validate
```

Validates the syntax of Terraform files.

---

### 🔹 3. Dry Run

```bash
terraform plan
```

**Purpose:**  
Shows the resources that Terraform will create (preview).

---

### 🔹 4. Apply the Configuration

```bash
terraform apply -auto-approve
```

**Purpose:**  
Creates the infrastructure defined in the `.tf` files.  
`-auto-approve` skips manual "yes" confirmation.

---

## 📤 Outputs After Apply

Sample output from `terraform apply` (based on `outputs.tf`):

```bash
Outputs:

private_subnet_id = "subnet-0abc1234def5678gh"
public_subnet_id  = "subnet-0def4567ghi8901jk"
vpc_id            = "vpc-0ff55aabbccddeeff"
```

This gives a quick reference to the important AWS resource IDs.

---

## 📂 Terraform State File

File: `terraform.tfstate`

> **Auto-generated** after running `terraform apply`.

**Purpose:**  
- Keeps track of all created resources.
- Records every attribute of each resource in **JSON** format.
- Acts as Terraform's **memory**.

To inspect:

```bash
cat terraform.tfstate
```

**Sample contents:**

```json
{
  "resources": [
    {
      "type": "aws_vpc",
      "name": "main",
      "instances": [
        {
          "attributes": {
            "id": "vpc-0ff55aabbccddeeff",
            "cidr_block": "10.0.0.0/16",
            ...
          }
        }
      ]
    }
  ]
}
```

---

## 📚 Key Concepts Recap

| Concept          | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **provider**     | Specifies cloud provider (AWS, Azure, GCP, etc.)                           |
| **resource**     | Block to define infrastructure components (VPC, subnet, etc.)              |
| **output**       | Retrieves and prints values after resource creation                        |
| **terraform init** | Downloads necessary plugins based on provider                            |
| **terraform plan** | Dry-run that shows what Terraform *would* do                             |
| **terraform apply**| Executes the plan and creates the resources                              |
| **terraform.tfstate**| Records real-time state of infrastructure                              |
