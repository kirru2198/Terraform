To install **Terraform** on an Ubuntu machine, here's a simple and reliable method using HashiCorp's official APT repository:

---

### ✅ Step-by-Step: Install Terraform on Ubuntu

1. **Update your system**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install required dependencies**:
   ```bash
   sudo apt install -y gnupg software-properties-common curl
   ```

3. **Add HashiCorp GPG key**:
   ```bash
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
   ```

4. **Add the HashiCorp Linux repository**:
   ```bash
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
   sudo tee /etc/apt/sources.list.d/hashicorp.list
   ```

5. **Update and install Terraform**:
   ```bash
   sudo apt update
   sudo apt install terraform
   ```

6. **Verify the installation**:
   ```bash
   terraform -version
   ```

   Output should look like:
   ```
   Terraform v1.xx.x
   ```
---
