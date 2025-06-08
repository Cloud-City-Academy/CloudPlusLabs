
# 🎓 Cloud+ Lab: Custom VPC with EC2 Deployment (Free Tier)

## ✅ **Objective**
By the end of this lab, you will:
- Create a custom VPC with a public subnet
- Add an Internet Gateway and Route Table
- Launch a Free Tier EC2 instance inside your subnet
- Configure Security Groups and SSH key pair
- Test connectivity via SSH and HTTP

---

## 📝 **Pre-Lab Requirements**
- AWS IAM user with permissions to manage EC2 and VPC resources
- AWS Console access
- Terminal with SSH (macOS/Linux) or PuTTY (Windows)

---

## 📘 Part 1: Create a Custom VPC

1. Go to **VPC Dashboard** > **Your VPCs** > **Create VPC**
2. Choose **VPC only**
3. Set:
   - **Name**: `CloudPlus-<your-name>-VPC`
   - **IPv4 CIDR block**: `10.0.0.0/16`
   - Leave other settings default
4. Click **Create VPC**

---

## 🛠️ Part 2: Create a Public Subnet

1. Go to **Subnets** > **Create Subnet**
2. Set:
   - **Name**: `Public-Subnet`
   - **VPC**: `CloudPlus-<your-name>-VPC`
   - **Availability Zone**: (Pick any)
   - **CIDR Block**: `10.0.1.0/24`
3. Click **Create Subnet**

---

## 🔌 Part 3: Attach Internet Gateway

1. Go to **Internet Gateways** > **Create internet gateway**
   - Name: `CloudPlus-<your-name>-IGW`
2. Click **Attach to VPC** > Select `CloudPlus-VPC`

---

## 🌐 Part 4: Create Route Table and Add Route

1. Go to **Route Tables** > **Create route table**
   - Name: `Public-RT`
   - VPC: `CloudPlus-<your-name>-VPC`
2. Select your new Route Table
   - **Actions** > **Edit routes** > **Add route**:
     - **Destination**: `0.0.0.0/0`
     - **Target**: `Internet Gateway` > `CloudPlus-IGW`
3. Go to **Subnet associations** > **Edit subnet associations**
   - Check `Public-Subnet`
   - Save

---

## 🔐 Part 5: Create a Security Group

1. Go to **Security Groups** > **Create Security Group**
   - Name: `WebAccess-SG`
   - VPC: `CloudPlus-<your-name>-VPC`
2. Inbound Rules:
   - **SSH** (22) > My IP
   - **HTTP** (80) > Anywhere (0.0.0.0/0)
   - **HTTPS** (443) > Anywhere (0.0.0.0/0)
3. Save security group

---

## 🗝️ Part 6: Create a Key Pair

1. Go to **EC2 Dashboard** > **Key Pairs** > **Create Key Pair**
   - Name: `cloudplus-<your-name>-key`
   - File format: `.pem`
   - Download and **save the file securely**

---

## 🚀 Part 7: Launch EC2 Instance

1. Go to **EC2 Dashboard** > **Launch Instance**
2. Configure:
   - Name: `CloudPlus-<your-name>-WebServer`
   - AMI: Amazon Linux 2023 (Free Tier)
   - Instance Type: `t2.micro`
   - Key Pair: `cloudplus-key`
   - Network: `CloudPlus-VPC`
   - Subnet: `Public-Subnet`
   - Auto-assign Public IP: Enabled
   - Security Group: `WebAccess-SG`
3. Click **Launch Instance**

---

## 🧩 Part 8: SSH into Your Instance

1. Get the **public IP** from the EC2 instance details
2. In your terminal:
   ```bash
   chmod 400 cloudplus-key.pem
   ssh -i cloudplus-key.pem ec2-user@<your-public-ip>
   ```
   (Use `ec2-user` for Amazon Linux)

---

## ✨ Part 9: Test Web Server (Optional)

1. SSH into the instance
2. Install Apache:
   ```bash
   sudo yum install httpd -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   echo "<h1>Hello from CloudPlus!</h1>" | sudo tee /var/www/html/index.html
   ```
3. Visit `http://<your-public-ip>` in your browser — you should see your message.

---

## 🧹 Part 10: Clean Up (before 11 PM EST)
- Terminate EC2 instance
- Delete VPC and associated resources if needed (otherwise, they will be wiped automatically)

---

## ℹ️ Notes:
- Default VPC should **not be used** for this lab.
- Students have Free Tier permissions; larger instances or NAT Gateways will fail.
- Resources are deleted **nightly at 11 PM EST**, so save screenshots if needed.
