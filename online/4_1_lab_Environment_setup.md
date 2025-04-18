# 🧪 **Lab Notes: AWS EC2 + IAM + AWS CLI + Terraform Setup**

---

## ✅ **Objective**

Launch a Linux EC2 instance, create an IAM user with administrative privileges, configure AWS CLI inside the EC2 instance, and install Terraform for infrastructure automation.

---

## 🚀 **Step 1: Launch an EC2 Instance (Amazon Linux)**

1. Go to the AWS Console → EC2 Dashboard.
2. Click on **Launch Instance**.
3. Provide a name like: `Terraform-Lab-Instance`.
4. Choose Amazon Machine Image (AMI):
   - Select **Amazon Linux 2 AMI (HVM), SSD Volume Type**
5. Choose Instance Type:
   - e.g., **t2.micro** (Free Tier eligible)
6. Configure other settings as default or as required.
7. Select or create a new key pair to SSH into the instance.
8. Launch the instance and SSH into it.

---

## 👤 **Step 2: Create an IAM User with Admin Access**

1. Navigate to **IAM → Users** in the AWS Console.
2. Click **Add User**.
3. Set the username: `terraform-user` (or any preferred name).
4. Select **Access type**:  
   ✅ Programmatic access (to use with CLI)

5. Click **Next: Permissions**  
   - Attach existing policies directly.
   - Select **`AdministratorAccess`** policy.

6. Click **Next** through the rest and then **Create User**.

---

## 🔐 **Step 3: Create and Configure AWS Access Keys**

1. Go to **IAM → Users → terraform-user**.
2. Click on **Security credentials** tab.
3. Scroll to **Access keys** section.
4. Click **Create access key**.
   - Choose: **Command Line Interface (CLI)**.
   - Confirm and click **Next**.
   - Click **Create access key**.
5. **Copy both**:
   - `Access Key ID`
   - `Secret Access Key`

---

## ⚙️ **Step 4: Configure AWS CLI on EC2**

1. Ensure AWS CLI is installed:
   ```bash
   aws --version
   ```

2. If CLI is installed, configure it:
   ```bash
   aws configure
   ```
   Enter the values when prompted:
   ```
   AWS Access Key ID:      <Paste your Access Key>
   AWS Secret Access Key:  <Paste your Secret Key>
   Default region name:    ap-south-1
   Default output format:  text
   ```

✅ CLI is now configured and ready to use.

---

## 🔧 **Step 5: Install Terraform on EC2 (Amazon Linux)**

> Always follow the [official Terraform documentation](https://developer.hashicorp.com/terraform/downloads) for latest installation instructions.

1. Run the following commands in your EC2 terminal:

```bash
# Install required tools
sudo yum install -y yum-utils

# Add the HashiCorp Linux repository
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

# Install Terraform
sudo yum -y install terraform
```

2. Verify installation:
```bash
terraform -version
```

You should see output showing the installed Terraform version.
