To install the AWS CLI (version 2) on an Ubuntu machine, follow these steps:

---

### ✅ Step-by-Step: Install AWS CLI v2 on Ubuntu

1. **Update your system**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install dependencies**:
   ```bash
   sudo apt install unzip curl -y
   ```

3. **Download the AWS CLI v2 installer**:
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   ```

4. **Unzip the installer**:
   ```bash
   unzip awscliv2.zip
   ```

5. **Run the install script**:
   ```bash
   sudo ./aws/install
   ```

6. **Verify the installation**:
   ```bash
   aws --version
   ```

   You should see output like:
   ```
   aws-cli/2.x.x Python/3.x.x Linux/x86_64
   ```

---

### 🧹 Optional: Clean up
```bash
rm -rf awscliv2.zip aws/
```
