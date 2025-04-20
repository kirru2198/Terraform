# 🌐 Terraform AWS VPC Setup – Step-by-Step with Descriptions

In this guide, we'll walk through how to **create a VPC in AWS using Terraform**, along with subnets (public and private), an Internet Gateway (IGW), and route tables. We'll also understand how Terraform uses its resource blocks and how to associate resources.

---

## 📌 What is a VPC?

A **Virtual Private Cloud (VPC)** is a logically isolated network within AWS. It allows you to launch AWS resources in a virtual network that you define.

---

## 🧱 Steps to Create a VPC

1. Create a **VPC**
2. Create **Subnets** (Public & Private)
3. Create an **Internet Gateway (IGW)**
4. Attach IGW to the VPC
5. Create **Route Tables**
6. Associate Route Tables with Subnets
7. Configure route entries

---

## 🏗️ Terraform Resource Syntax

Every Terraform resource follows this structure:

```hcl
resource "<PROVIDER>_<RESOURCE_TYPE>" "<NAME>" {
  ...configuration...
}
```

Example for a VPC:
```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "main-vpc"
  }
}
```

---

## 🛠️ VPC Creation

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true
  tags = {
    Name = "main-vpc"
  }
}
```

---

## 🌐 Subnet Creation (Public & Private)

```hcl
# Public Subnet
resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "us-east-1a"
  map_public_ip_on_launch = true

  tags = {
    Name = "public-subnet"
  }
}

# Private Subnet
resource "aws_subnet" "private" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.2.0/24"
  availability_zone = "us-east-1a"

  tags = {
    Name = "private-subnet"
  }
}
```

**Note:**  
The `map_public_ip_on_launch = true` makes sure that the EC2 instances launched in the **public subnet** get a public IP.

---

## 🚪 Internet Gateway (IGW)

```hcl
resource "aws_internet_gateway" "gw" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "main-gateway"
  }
}
```

The IGW must be **attached to the VPC** for internet connectivity.

---

## 🚦 Route Table and Routes

### Public Route Table

```hcl
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.gw.id
  }

  tags = {
    Name = "public-route"
  }
}
```

### Associate Route Table with Public Subnet

```hcl
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}
```

---

## 🔒 Private Route Table (Optional Setup Example)

If using NAT Gateway for private subnets:

```hcl
resource "aws_route_table" "private" {
  vpc_id = aws_vpc.main.id

  # route block for NAT Gateway will go here if needed
  tags = {
    Name = "private-route"
  }
}

resource "aws_route_table_association" "private_assoc" {
  subnet_id      = aws_subnet.private.id
  route_table_id = aws_route_table.private.id
}
```

---

## 🏷️ Why Use Tags?

- Helps you **easily identify resources**
- Tagging with `Name` is helpful for visualization in AWS Console
- Sometimes required for correct resource naming in dashboards

**Note:**  
There are discussions on forums (e.g., StackOverflow) suggesting that omitting the `tags` block may result in resources being unnamed in the AWS Console even if you provide `name` in other ways.

---

## 📖 Understanding Terraform Documentation

Whenever you check a Terraform resource (like `aws_vpc`, `aws_subnet`, etc.), always refer to:

- **Argument Reference:** Lists all attributes you can configure
- **Required vs Optional Parameters:** Required must be provided, optional has default values

---

## 🧠 Pro Tip

> Once you understand the **logic of one Terraform module**, creating others becomes easier—it's mostly about **understanding the relationships** and **syntax**.
