This session covered **Terraform variables** in depth, and it looks like your instructor provided a very real-world, structured walkthrough of how and why to use them. Let’s break down and summarize the key takeaways from what was discussed, so it’s easier to recall and apply:

---

### 🧠 **What are Terraform Variables?**

Terraform variables let you **parameterize your configuration**. Instead of hardcoding values (like VPC CIDRs, instance types, AMIs, etc.), you define variables, give them default values, and reuse them throughout your Terraform code.

---

### ✅ **Why use variables?**

- **Reusability**: You define a value once and use it multiple times.
- **Flexibility**: Easily switch values without modifying all files.
- **Maintainability**: Centralized control of config inputs.
- **Separation of concerns**: Keeps logic (in `.tf` files) and data/inputs (in `variables.tf`) separate.

---

### 📁 **Project structure recommendation**

Your instructor demonstrated a modular file layout, which is excellent practice:

```
terraform_project/
├── providers.tf
├── variables.tf
├── vpc.tf
├── subnet.tf
├── security_group.tf
├── ec2.tf
├── outputs.tf
```

Each `.tf` file handles a specific resource type, making code **modular, readable, and easier to debug**.

---

### 📌 **Defining a Variable (`variables.tf`)**

```hcl
variable "vpc_cidr" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}
```

### 📌 **Using a Variable (`vpc.tf`)**

```hcl
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
}
```

🔁 **Rule of thumb**: `var.<variable_name>` is how you reference a variable.

---

### 💡 **Extra Concepts Covered**

- **Multiple AWS profiles**: You can create and reference custom AWS CLI profiles using the `~/.aws/credentials` file and specify them in the provider block.
  
- **User Data Script**: At EC2 instance launch, you can install software or configure settings using `user_data`.

    ```hcl
    user_data = <<-EOF
      #!/bin/bash
      yum install -y httpd
      systemctl start httpd
      echo "Welcome!" > /var/www/html/index.html
    EOF
    ```

- **Security Groups**: Ingress (inbound) and Egress (outbound) rules were shown clearly.

---

### 🧪 Common Error Handling Tip

Terraform will point to exact file and line number on errors. Modular files make this easier to resolve:

- If there's a bug in `ec2.tf` at line 3, you're only looking at a small file, not a monolith with 100s of lines.

---

Let me know if you want a cheat sheet of Terraform variable types (like `string`, `list`, `map`), or a sample hands-on mini-project to practice this setup on your own.

Awesome, glad you’re following along!

So just to recap and solidify the core concept we covered before the break — **Terraform variables**:

### 🔧 What Are Terraform Variables?

Variables in Terraform allow you to **parameterize your code**, so instead of hardcoding values like CIDR blocks, instance types, AMI IDs, etc., you define them in one place (like `variables.tf`) and **reuse them across your configuration**.

---

### 🎯 Why Use Variables?

Here’s **why** they’re useful:

- ✅ **Reusability**: Define once, use many times.
- ✅ **Maintainability**: Update a value in one place instead of everywhere in your code.
- ✅ **Flexibility**: Make your code adaptable for different environments (dev, staging, prod).
- ✅ **Cleaner Code**: Avoid magic numbers or hardcoded values.

---

### 📂 Best Practice Setup

Terraform project structure typically looks like this:

```
terraform-project/
│
├── providers.tf         # Define your provider & region
├── variables.tf         # All variable declarations
├── vpc.tf               # VPC resource code using variables
├── subnet.tf            # Subnets (public/private)
├── security-group.tf    # Ingress/egress rules
├── ec2.tf               # EC2 instance using variables & user_data
├── outputs.tf           # Output values (like instance IP)
```

---

### 🧠 Example Variable Declaration (in `variables.tf`)

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "vpc_cidr" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}
```

### 🔁 Using Variables in Other Files

```hcl
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
}

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
}
```

---

### 💬 Let me know if you want a hands-on mini challenge or want to dive deeper into `terraform.tfvars`, `locals`, or `output` blocks next.

Perfect! Let's keep the momentum going. Since you said “yes,” I’ll assume you're up for a **hands-on mini challenge**—and then we can dive into `terraform.tfvars`, `locals`, or `output` blocks after that.

---

### 🧩 Mini Terraform Variables Challenge

Let’s say you're building a basic AWS infrastructure with a VPC and one EC2 instance. Here's your challenge:

#### 🛠️ Task:
1. **Declare** the following variables in `variables.tf`:
   - `vpc_cidr` (default: `"10.10.0.0/16"`)
   - `instance_type` (default: `"t3.micro"`)
   - `ami_id` (no default, required input)

2. **Use** these variables in `main.tf` (or `vpc.tf` and `ec2.tf`) to:
   - Create a VPC using `vpc_cidr`
   - Launch an EC2 instance using `instance_type` and `ami_id`

---

### 🧾 Bonus:
- Create a `terraform.tfvars` file and supply a custom `ami_id` value (e.g., `"ami-0c55b159cbfafe1f0"`).
- Add an `outputs.tf` file to print the EC2 instance's public IP.

---

Do you want me to give you a full working solution after you try, or would you like me to walk you through building each file step by step?

Awesome! Since you're ready, let’s **build it step by step** — that way you'll really get how variables and other pieces work together in Terraform.

---

### ✅ Step 1: Define Variables (`variables.tf`)

```hcl
variable "vpc_cidr" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.10.0.0/16"
}

variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t3.micro"
}

variable "ami_id" {
  description = "AMI ID for the EC2 instance"
  type        = string
}
```

---

### ✅ Step 2: Use the Variables in Main Infra (`main.tf`)

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
}

resource "aws_subnet" "main" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.10.1.0/24"
  availability_zone = "us-east-1a"
}

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.main.id

  tags = {
    Name = "MyWebServer"
  }
}
```

---

### ✅ Step 3: Create `terraform.tfvars`

```hcl
ami_id = "ami-0c55b159cbfafe1f0"
```

You can customize the `ami_id` here (or pull from AWS Console if needed).

---

### ✅ Step 4: Output the Public IP (`outputs.tf`)

```hcl
output "ec2_public_ip" {
  value = aws_instance.web.public_ip
}
```

---

### 🧪 Test it Out:

```bash
terraform init
terraform plan
terraform apply
```

You'll see the EC2 instance created with the IP shown after the apply. You can destroy it later with:

```bash
terraform destroy
```

---

Want to add **`locals`**, **`map variables`**, or **more automation** next?

Nice! Let’s level up a bit. We'll introduce **`locals`**, a **`map variable`**, and make your configuration cleaner and more dynamic.

---

## 🔁 Step 5: Add `locals` for Reusability

Create a section in `main.tf` (or a new `locals.tf` file):

```hcl
locals {
  project_name = "devops-training"
  environment  = "dev"
  instance_name = "${local.project_name}-web"
}
```

Use it in your EC2 tag:

```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.main.id

  tags = {
    Name = local.instance_name
    Env  = local.environment
  }
}
```

---

## 🗺️ Step 6: Use a `map` Variable for Instance Types per Environment

Update `variables.tf`:

```hcl
variable "instance_types" {
  description = "Map of instance types per environment"
  type        = map(string)
  default = {
    dev  = "t3.micro"
    prod = "t3.medium"
  }
}
```

Then change `instance_type` in `aws_instance`:

```hcl
instance_type = var.instance_types[local.environment]
```

Now your instance type depends on the environment set in `locals`.

---

## 🔧 Optional: Parameterize `environment` via Variable Instead of Hardcoding

You can make this dynamic by converting the local `environment` into a variable:

### In `variables.tf`:

```hcl
variable "environment" {
  description = "Deployment environment"
  type        = string
  default     = "dev"
}
```

### Then in `locals.tf`:

```hcl
locals {
  project_name  = "devops-training"
  instance_name = "${local.project_name}-web"
  environment   = var.environment
}
```

---

### ✅ Now you're using:
- `variables` (with types and defaults)
- `terraform.tfvars` for values
- `locals` for reusability
- `map variables` for conditional logic

---

Want to go next into:
- 🔐 Creating and attaching **security groups**?
- 🌐 Attaching **Elastic IPs** or **Internet Gateway** for public access?
- 📦 Using **modules** to split up reusable VPC/EC2 logic?

Pick your direction or tell me your goal — I’ll help you build it!
