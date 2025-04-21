# Terraform with Variables for Reusability and Flexibility

Variables in Terraform allow you to define values once and reuse them multiple times throughout your configuration files. This improves reusability, manageability, and flexibility, as you can easily update the values in one place and have them propagate across all the relevant resources.

In this guide, we’ll explore how to use variables in Terraform to manage resources like VPCs, subnets, EC2 instances, and security groups.

---

## **1. Overview of Terraform Variables**

Variables are defined in `variables.tf` and can be referenced anywhere in your configuration. Here’s a breakdown of the files and resources we’ll use:

- **`variables.tf`**: Contains the definitions of variables such as CIDR blocks, instance type, and other configurations.
- **`vpc.tf`**: Contains the resource definitions for the VPC.
- **`subnet.tf`**: Contains the subnet definitions, including both public and private subnets.
- **`ec2-instance.tf`**: Contains the EC2 instance definition.
- **`security-group.tf`**: Contains the security group definition that allows HTTP and SSH traffic.

<img width="474" alt="image" src="https://github.com/user-attachments/assets/f15b79f8-0b4f-420e-8df0-8198c3df30a6" />

<img width="145" alt="image" src="https://github.com/user-attachments/assets/ba394fe7-a1a1-4dfb-a34c-328f92518423" />

---

## **2. Define Variables**

### **`variables.tf`**

In this file, we define the variables for the VPC, subnets, instance type, and other necessary configurations.

```hcl
# variables.tf

variable "vpc_cidr" {
  description = "The CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "public_subnet_cidr" {
  description = "The CIDR block for the public subnet"
  type        = string
  default     = "10.0.1.0/24"
}

variable "private_subnet_cidr" {
  description = "The CIDR block for the private subnet"
  type        = string
  default     = "10.0.2.0/24"
}

variable "instance_type" {
  description = "The type of instance to use"
  type        = string
  default     = "t2.micro"
}

variable "ami_id" {
  description = "The AMI ID to use for the instance"
  type        = string
  default     = "ami-06f621d90fa29f6d0"  # Update this as per your region
}

variable "key_name" {
  description = "The key name to use for the instance"
  type        = string
  default     = "my-key-pair"  # Ensure this key pair exists in your AWS account
}
```

Here, we’ve defined several variables such as `vpc_cidr`, `public_subnet_cidr`, `private_subnet_cidr`, `instance_type`, `ami_id`, and `key_name`.

- **`vpc_cidr`**: The CIDR block for the VPC.
- **`public_subnet_cidr`**: The CIDR block for the public subnet.
- **`private_subnet_cidr`**: The CIDR block for the private subnet.
- **`instance_type`**: The type of EC2 instance to be used (e.g., `t2.micro`).
- **`ami_id`**: The Amazon Machine Image (AMI) ID to be used.
- **`key_name`**: The key name for SSH access to the EC2 instance.

---

## **3. Use Variables in Resource Definitions**

### **`vpc.tf`**

In this file, we define the VPC resource using the `vpc_cidr` variable.

```hcl
# vpc.tf

resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = "main-vpc"
  }
}
```

Here, the `cidr_block` is dynamically set from the `vpc_cidr` variable.

### **`subnet.tf`**

In this file, we define the public and private subnets using `public_subnet_cidr` and `private_subnet_cidr` variables.

```hcl
# subnet.tf

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.public_subnet_cidr
  map_public_ip_on_launch = true
  availability_zone       = "ap-south-1a"
  tags = {
    Name = "public-subnet"
  }
}

resource "aws_subnet" "private" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.private_subnet_cidr
  availability_zone = "ap-south-1a"
  tags = {
    Name = "private-subnet"
  }
}
```

- The `public` subnet uses the `public_subnet_cidr` variable.
- The `private` subnet uses the `private_subnet_cidr` variable.

### **`ec2-instance.tf`**

Here we use the `ami_id`, `instance_type`, and `key_name` variables to launch an EC2 instance.

```hcl
# ec2-instance.tf

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.public.id
  key_name      = var.key_name
  vpc_security_group_ids = [aws_security_group.web_sg.id]

  user_data = <<-EOF
              #!/bin/bash
              yum update -y
              yum install -y httpd
              systemctl start httpd
              systemctl enable httpd
              echo "<html><h1>Hello from Terraform</h1></html>" > /var/www/html/index.html
              EOF

  tags = {
    Name = "web-server"
  }
}
```

This defines an EC2 instance in the public subnet, with the specified AMI, instance type, and key name.

### **`security-group.tf`**

Here, we define the security group for the EC2 instance to allow SSH and HTTP traffic.

```hcl
# security-group.tf

resource "aws_security_group" "web_sg" {
  vpc_id = aws_vpc.main.id
  description = "Allow HTTP and SSH traffic"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "web-sg"
  }
}
```

This security group allows inbound traffic on port 22 (SSH) and port 80 (HTTP), and all outbound traffic.

---

## **4. Error: Key Pair Not Found**

If you see an error like:

```
│ Error: creating EC2 Instance: operation error EC2: RunInstances, https response error StatusCode: 400, RequestID: a414dc2e-cefa-4541-b50c-cee920e22889, api error InvalidKeyPair.NotFound: The key pair 'my-key-pair' does not exist
```

This means the key pair specified (`my-key-pair`) does not exist in your AWS account. To resolve this issue:

1. Make sure the key pair exists in your AWS account.
2. Update the `key_name` variable in `variables.tf` with the correct key pair name.

---

## **5. Project Structure**

Your Terraform project should look like this:

```
terraform_project_1/
├── provider.tf         # AWS provider configuration
├── variables.tf        # Variable definitions
├── vpc.tf              # VPC resource definitions
├── subnet.tf           # Subnet resource definitions
├── security-group.tf   # Security group resource definitions
├── ec2-instance.tf     # EC2 instance resource definitions
├── outputs.tf          # Outputs (optional)
```

---

## **6. Conclusion**

By using variables in Terraform, you can make your infrastructure more flexible, reusable, and easier to manage. Instead of hardcoding values in each resource, you define them once in `variables.tf` and reference them throughout your configuration files.

This approach simplifies updates and makes the code more modular, which is beneficial when working in a collaborative environment or managing complex infrastructures.
