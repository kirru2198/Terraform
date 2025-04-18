## 🔧 **Terraform Environment Setup with AWS IAM & CLI**

---

### ✅ **Step 1: IAM User Setup in AWS Console**

1. **Go to IAM** in AWS Console.
2. Navigate to **Users**.
3. Select the user named **`AWS-DevOps`**.
4. **Policy Attached**:  
   - `AdministratorAccess`  
   - Grants full permissions to manage all AWS services and resources.

---

### ✅ **Step 2: Generate AWS Credentials for the User**

1. Go to **Security Credentials** tab.
2. Under **Access Keys**, do the following:
   - Deactivate and delete any existing access key.
   - Click **Create access key**.
   - Copy the **Access Key ID** and **Secret Access Key**.
   - **Important**: Save them securely (you won’t be able to retrieve the secret key again).

---

### ✅ **Step 3: Configure AWS CLI on EC2 Instance**

1. **Ensure AWS CLI is installed**  
   ```bash
   aws --version
   ```

2. **Configure AWS CLI**  
   ```bash
   aws configure
   ```
   - **Access Key ID**: (Paste from step above)
   - **Secret Access Key**: (Paste from step above)
   - **Default Region Name**: e.g. `ap-south-1`
   - **Output Format**: `text` (or can use `json`, `table`)

---

### ✅ **Step 4: Install Terraform on EC2 (Amazon Linux)**

1. **Follow official Terraform installation guide:**  
   [https://developer.hashicorp.com/terraform/downloads](https://developer.hashicorp.com/terraform/downloads)

2. **For Amazon Linux, run these commands:**

   ```bash
   sudo yum install -y yum-utils
   sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
   sudo yum -y install terraform
   ```

3. **Verify installation**  
   ```bash
   terraform -version
   ```

---

### ✅ **Step 5: Test Terraform Installation**

- Run:
   ```bash
   terraform
   ```
   - Shows list of Terraform commands.

---

### ✅ **Most Common Terraform Commands**

| Command               | Purpose                                                        |
|-----------------------|----------------------------------------------------------------|
| `terraform init`      | Initializes the working directory with config files            |
| `terraform validate`  | Validates the Terraform configuration                          |
| `terraform plan`      | Shows the execution plan (dry run)                             |
| `terraform apply`     | Applies the changes to reach the desired state                 |
| `terraform destroy`   | Destroys the infrastructure created by Terraform               |

---

### ✅ **Summary of Setup Steps**

1. **Created IAM user (`AWS-DevOps`)** with `AdministratorAccess`.
2. **Generated and configured AWS credentials** using `aws configure`.
3. **Installed Terraform** on Amazon Linux EC2 instance.
4. **Validated Terraform setup** and reviewed common commands.

---
