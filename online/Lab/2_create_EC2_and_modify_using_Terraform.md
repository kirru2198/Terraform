# **Terraform EC2 Instance Project Guide**

This guide demonstrates how to create, modify, and manage an EC2 instance using Terraform. It will cover initializing a Terraform project, applying configurations, and modifying existing resources.

## **1. Set Up Your Terraform Project Directory**

### **Create a new directory for the project:**
First, create a directory for your Terraform project and navigate into it:

```bash
mkdir my_tf_project
cd my_tf_project
```

## **2. Create Terraform Configuration Files**

### **Create the `provider.tf` File**

In this file, we define the AWS provider and specify the region and the profile to use for AWS credentials.

```hcl
# provider.tf

# Provider Block
provider "aws" {
  region  = "ap-south-1"  # Choose the AWS region (Mumbai in this case)
  profile = "default"      # Use the default AWS CLI profile
}
```

### **Create the `ec2-instance.tf` File**

This file defines the EC2 instance that Terraform will create.

```hcl
# ec2-instance.tf

# Resource Block
resource "aws_instance" "ec2demo" {
  ami           = "ami-06f621d90fa29f6d0"  # Amazon Linux 2 AMI (adjust for your region)
  instance_type = "t2.micro"               # Instance type (small, cost-effective)
}
```

### **Directory Structure**

Your Terraform project structure should now look like this:

```
my_tf_project/
├── terraform-provider.tf   # AWS provider configuration
└── ec2-instance.tf         # EC2 instance definition
```

## **3. Initialize the Terraform Project**

Run the following command to initialize the Terraform project. This will install the required provider (in this case, AWS) and set up the necessary configuration.

```bash
terraform init
```

The output will look like this:

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

## **4. Apply the Terraform Configuration**

Now that the project is initialized, apply the Terraform configuration to create the EC2 instance. Run the following command:

```bash
terraform apply
```

Terraform will display a plan and ask for your confirmation. After confirming (type `yes`), Terraform will create the EC2 instance.

The output will look like this:

```bash
aws_instance.ec2demo: Creating...
aws_instance.ec2demo: Still creating... [10s elapsed]
aws_instance.ec2demo: Still creating... [20s elapsed]
aws_instance.ec2demo: Still creating... [30s elapsed]
aws_instance.ec2demo: Creation complete after 32s [id=i-05c5d55f4819f6069]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

## **5. Verify the Files in Your Project Directory**

After running `terraform apply`, you will see the following structure:

```bash
ls -la
```

Output:

```
.terraform              # Plugin downloads
.terraform.lock.hcl     # Lock file for provider versions
ec2-instance.tf         # Your EC2 instance configuration code
terraform-provider.tf   # AWS provider configuration code
terraform.tfstate       # Terraform state file (holds resource info)
```

### **State File: `terraform.tfstate`**

The `terraform.tfstate` file is a critical file in Terraform as it holds the state of your infrastructure. It is Terraform's "single source of truth," tracking all created resources.

You can inspect the state file using the following commands:

```bash
cat terraform.tfstate
```

To specifically search for details about the instance:

```bash
cat terraform.tfstate | grep instance
cat terraform.tfstate | grep instance_type
```

---

## **6. Modify the EC2 Instance Configuration**

### **Change the Instance Type**

To modify the instance type, follow these steps:

1. **Stop the Instance**: First, stop the instance to avoid any potential issues while changing the configuration.

2. **Modify the Instance Type**: In the `ec2-instance.tf` file, change the instance type. For example:

```hcl
resource "aws_instance" "ec2demo" {
  ami           = "ami-06f621d90fa29f6d0"  # Amazon Linux 2 AMI
  instance_type = "t2.medium"              # Change from t2.micro to t2.medium
}
```

3. **Log in to the AWS Console (Optional)**:
   - Go to the AWS Console → EC2 → Instances.
   - Select the instance.
   - Under **Actions > Instance Settings**, choose **Change Instance Type** and select the new instance type.

4. **Start the Instance**: Start the instance again after modifying the instance type.

---

## **7. Apply the Changes with Terraform**

After updating the `ec2-instance.tf` file, run the following commands to apply the changes.

### **Plan the Changes**

Before applying the changes, run the `terraform plan` command to see the changes that will be made to your infrastructure:

```bash
terraform plan
```

### **Apply the Changes**

To apply the changes automatically without a prompt, run:

```bash
terraform apply --auto-approve
```

This will apply the changes (e.g., modifying the instance type) without requiring further confirmation.

---

## **8. Summary**

1. **Initialization**: We initialized the Terraform project using `terraform init`, which downloaded the necessary AWS provider.
2. **Creating Resources**: We created an EC2 instance using the `terraform apply` command.
3. **Modifying Resources**: We updated the `ec2-instance.tf` file, stopped the EC2 instance, modified its instance type, and then applied the changes using `terraform plan` and `terraform apply`.
4. **State Management**: The state of the infrastructure was maintained in `terraform.tfstate`, which helps Terraform track and manage resources.

By following these steps, you can manage your AWS infrastructure efficiently using Terraform.
