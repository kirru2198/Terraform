
# Terraform - Infrastructure as Code (IAC)
---

Terraform is a tool that automates infrastructure provisioning and management, making it a key part of the Infrastructure as Code (IAC) approach. IAC helps automate the process of setting up and managing infrastructure, allowing developers and operations teams to deploy and maintain their systems using code instead of manual configuration.

---

## Command Example:
```bash
sudo su -
PS1="Terraform $"
```

This sets the shell prompt (`PS1`) to show `Terraform $` for clarity, indicating that the user is working with Terraform.

---

## What is Terraform?

- **Terraform** is a tool developed by HashiCorp that enables users to provision, manage, and scale infrastructure in a consistent and repeatable manner using a declarative configuration language.
- It is platform-agnostic and can be used with **any cloud provider** (AWS, Google Cloud, Azure, etc.).
- Terraform helps in automating the process of creating, updating, and maintaining infrastructure, making it a fundamental part of the Infrastructure as Code approach.
  
### Terraform vs CloudFormation:
- **CloudFormation** is another infrastructure-as-code tool, but it is specific to AWS. It allows AWS users to model and provision AWS resources using YAML or JSON templates.
- **Terraform**, however, is more flexible as it supports **multiple cloud providers** and not just AWS.

---

## How Terraform Works

When you run Terraform to create or manage resources, it interacts with APIs provided by cloud providers like AWS, Azure, etc.

1. **API Gateway Interaction:**
   - Terraform communicates with the cloud provider's **API Gateway** (often miscalled the "API endpoint").
   - The API Gateway acts as a gatekeeper to ensure that the requests Terraform sends are authorized and authenticated.

2. **Authentication and Authorization:**
   - The API server on the cloud provider’s side handles authentication (confirming who you are) and authorization (ensuring you have the right permissions to perform the action).
   - In the case of AWS, Terraform will use the credentials configured in the **AWS CLI** (Access Key ID and Secret Access Key) to authenticate the requests.

---

## Requirements and Steps

Before you start working with Terraform, ensure you have the following setup.

### Step 1: Install AWS CLI

If you're using an Amazon Linux machine, the **AWS CLI** is likely already installed. You can verify the installation with the following command:

```bash
aws --version
```

If it's not installed, you can install it by following the [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

### Step 2: Configure AWS CLI

After installing the AWS CLI, you need to configure it with your AWS credentials. Run the following command to set up the AWS CLI:

```bash
aws configure
```

You will be prompted to provide the following information:
- **AWS Access Key ID**: Your AWS access key.
- **AWS Secret Access Key**: Your AWS secret access key.
- **Default region name**: The AWS region where you want to operate.
- **Default output format**: Choose the output format, typically `json`.

### Step 3: Install Terraform

To install Terraform, follow the instructions provided in the [official Terraform installation guide](https://learn.hashicorp.com/tutorials/terraform/install-cli).

Alternatively, you can install Terraform using the package manager for your operating system (e.g., `apt`, `brew`, etc.).

### Step 4: Run Terraform

1. **Write Terraform Configuration:**
   - Create a `.tf` file (e.g., `main.tf`) with the configuration for the infrastructure you want to create.
   - Example of a simple Terraform configuration for AWS:

   ```hcl
   provider "aws" {
     region = "us-west-2"
   }

   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

2. **Initialize Terraform:**
   - Run `terraform init` in the directory containing your `.tf` files. This will initialize the working directory and download any required provider plugins.

   ```bash
   terraform init
   ```

3. **Plan and Apply Changes:**
   - Before applying the changes, it's always a good idea to run `terraform plan`, which shows you what changes Terraform will make.
   
   ```bash
   terraform plan
   ```

   - Once you're ready to apply the changes and create the resources, use the following command:

   ```bash
   terraform apply
   ```

   - Confirm the action when prompted.

4. **Manage Infrastructure:**
   - After applying the configuration, Terraform will manage the infrastructure. You can make updates to your `.tf` files and reapply the changes to modify the infrastructure.
   - To destroy the infrastructure when you're done, run:

   ```bash
   terraform destroy
   ```

---

## Conclusion

By using Terraform, you can automate the creation and management of infrastructure in a consistent, repeatable, and version-controlled manner, which is the essence of Infrastructure as Code. It's a powerful tool that works across multiple cloud providers, allowing you to manage your infrastructure resources in a unified way.

