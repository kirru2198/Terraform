Here’s a markdown guide on how to create a Virtual Private Cloud (VPC) in AWS using Terraform, along with subnets, an Internet Gateway, and routing tables.

---

# **AWS VPC Setup with Terraform**

This guide outlines how to create a Virtual Private Cloud (VPC) in AWS using Terraform. It includes the following steps:

Create a VPC.

Create a public and private subnet.

Attach an Internet Gateway to the VPC.

Set up routing tables for public and private subnets.


This guide demonstrates how to use Terraform to create a **Virtual Private Cloud (VPC)** in AWS with public and private subnets, an Internet Gateway, and route tables.

## **1. Terraform Configuration Files**

### **Provider Configuration (provider.tf)**

This file defines the AWS provider and credentials. It tells Terraform which cloud provider to use and which credentials to apply.

```hcl
# provider.tf

# AWS provider block
provider "aws" {
  region  = "ap-south-1"  # Mumbai region
  profile = "default"      # Use the default AWS CLI profile for credentials
}
```

### **VPC and Subnet Configuration (vpc.tf)**

In this file, we define the VPC, subnets (public and private), an Internet Gateway, and route tables for both subnets.

```hcl
# vpc.tf

# Define the VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"  # VPC CIDR block
  tags = {
    Name = "main-vpc"          # Name tag for the VPC
  }
}

# Define the public subnet
resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"      # Subnet CIDR block
  map_public_ip_on_launch = true                 # Enable public IP on launch
  availability_zone       = "ap-south-1a"       # Availability zone
  tags = {
    Name = "public-subnet"  # Name tag for the public subnet
  }
}

# Define the private subnet
resource "aws_subnet" "private" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.2.0/24"  # Subnet CIDR block
  availability_zone = "ap-south-1a"   # Availability zone
  tags = {
    Name = "private-subnet"  # Name tag for the private subnet
  }
}

# Define the Internet Gateway
resource "aws_internet_gateway" "gw" {
  vpc_id = aws_vpc.main.id
  tags = {
    Name = "main-gateway"  # Name tag for the gateway
  }
}

# Define the public route table
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"  # Route for internet access
    gateway_id = aws_internet_gateway.gw.id
  }

  tags = {
    Name = "public-route-table"  # Name tag for the public route table
  }
}

# Associate the public subnet with the public route table
resource "aws_route_table_association" "public" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}

# Define the private route table (no internet access)
resource "aws_route_table" "private" {
  vpc_id = aws_vpc.main.id
  tags = {
    Name = "private-route-table"  # Name tag for the private route table
  }
}

# Associate the private subnet with the private route table
resource "aws_route_table_association" "private" {
  subnet_id      = aws_subnet.private.id
  route_table_id = aws_route_table.private.id
}
```

### **Outputs Configuration (outputs.tf)**

This file outputs useful information about the created resources, such as the VPC ID and subnet IDs.

```hcl
# outputs.tf

# Output the VPC ID
output "vpc_id" {
  description = "The ID of the VPC"
  value       = aws_vpc.main.id
}

# Output the public subnet ID
output "public_subnet_id" {
  description = "The ID of the public subnet"
  value       = aws_subnet.public.id
}

# Output the private subnet ID
output "private_subnet_id" {
  description = "The ID of the private subnet"
  value       = aws_subnet.private.id
}
```

## **2. Terraform Project Structure**

After creating the necessary Terraform configuration files, your project directory will look like this:

```
terraform_project/
├── provider.tf         # AWS provider configuration
├── vpc.tf              # VPC, subnets, gateway, and route tables configuration
└── outputs.tf          # Outputs for resource IDs
```

## **3. Initialize the Terraform Project**

Before applying any Terraform configurations, initialize your project by running the following command. This installs necessary provider plugins and sets up the environment.

```bash
terraform init
```

You will see an output like this:

```bash
Initializing the backend...
Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v5.80.0...
- Installed hashicorp/aws v5.80.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider selections it made above. Include this file in your version control repository so that Terraform can guarantee to make the same selections by default when you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see any changes that are required for your infrastructure. All Terraform commands should now work.
```

## **4. Apply the Configuration**

To apply the configuration and create the resources, run the following command:

```bash
terraform apply
```

You will see output like this:

```bash
aws_vpc.main: Creating...
aws_vpc.main: Creation complete after 1s [id=vpc-01d5b13b728a9ff55]
aws_internet_gateway.gw: Creating...
aws_subnet.public: Creating...
aws_subnet.private: Creating...
aws_route_table.private: Creating...
aws_route_table.private: Creation complete after 0s [id=rtb-0e60344f4f15d6697]
aws_internet_gateway.gw: Creation complete after 0s [id=igw-02de4a6b1fb071cd3]
aws_route_table.public: Creating...
aws_subnet.private: Creation complete after 0s [id=subnet-0aaffacb19482a027]
aws_route_table_association.private: Creating...
aws_route_table_association.private: Creation complete after 1s [id=rtbassoc-0d6c4bb94ec02ca87]
aws_route_table.public: Creation complete after 1s [id=rtb-0b3b0fb5c028ae9af]
aws_subnet.public: Still creating... [10s elapsed]
aws_subnet.public: Creation complete after 11s [id=subnet-0ef9dcbf64d9d5dfe]
aws_route_table_association.public: Creating...
aws_route_table_association.public: Creation complete after 0s [id=rtbassoc-0c404537098fbbe40]

Apply complete! Resources: 8 added, 0 changed, 0 destroyed.

Outputs:

private_subnet_id = "subnet-0aaffacb19482a027"
public_subnet_id = "subnet-0ef9dcbf64d9d5dfe"
vpc_id = "vpc-01d5b13b728a9ff55"
```

Terraform has successfully created the VPC, subnets, and associated resources.

## **5. Verify the Resources**

You can now verify the resources by running the following:

```bash
terraform output
```

This will display the output values, such as the VPC ID and the subnet IDs:

```bash
private_subnet_id = "subnet-0aaffacb19482a027"
public_subnet_id = "subnet-0ef9dcbf64d9d5dfe"
vpc_id = "vpc-01d5b13b728a9ff55"
```

## **6. Terraform Project Directory Listing**

Use the `ls` command to list the files and directories in your Terraform project directory:

```bash
ls -lrt
```

Output:

```bash
-rw-r--r-- 1 user user   97 Apr  7 10:00 outputs.tf
-rw-r--r-- 1 user user  240 Apr  7 10:00 provider.tf
-rw-r--r-- 1 user user  820 Apr  7 10:00 vpc.tf
drwxr-xr-x 3 user user 4096 Apr  7 10:01 .terraform
-rw-r--r-- 1 user user  150 Apr  7 10:01 .terraform.lock.hcl
-rw-r--r-- 1 user user   40 Apr  7 10:01 terraform.tfstate
```
Explanation of ls -lart
ls – List directory contents.

-l – Long listing format (shows permissions, owner, group, file size, and time of last modification).

-a – Shows hidden files (those starting with a dot).

-r – Reverses the order of listing (from newest to oldest).

-t – Sorts by modification time, with the newest files first.

The output would include your Terraform files (provider.tf, vpc.tf, outputs.tf), along with metadata like .terraform.lock.hcl (locking the provider versions) and the state file (terraform.tfstate), which stores the current state of your infrastructure.

---

## **7. Summary**

1. **Provider Configuration**: You defined the AWS provider in `provider.tf` to set up the region and credentials.
2. **VPC and Resources**: In `vpc.tf`, you created the VPC, subnets, Internet Gateway, and route tables.
3. **Outputs**: The `outputs.tf` file helps retrieve and display useful information about the created resources, such as VPC and subnet IDs.
4. **Initialization and Apply**: You initialized the Terraform project with `terraform init` and applied the configuration with `terraform apply`.
5. **Verification**: The outputs displayed the VPC and subnet IDs, confirming that the resources were created successfully.

By following this guide, you now have a functional VPC with public and private subnets in AWS, fully configured through Terraform.
