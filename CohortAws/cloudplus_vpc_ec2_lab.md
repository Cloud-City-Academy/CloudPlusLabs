
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

## If you dont have an AWS account (Register for AWS Free Tier)

Can use this as reference on how to signup: [https://www.geeksforgeeks.org/amazon-web-services-aws-free-tier-account-set-up/](https://www.geeksforgeeks.org/amazon-web-services-aws-free-tier-account-set-up/)

1. Go to [https://aws.amazon.com/free](https://aws.amazon.com/free)
2. Click **Create a Free Account**
3. Follow the signup instructions:
   - Email and password
   - Billing info (credit/debit card)
   - Identity verification
   - **Select the Free Basic Support Plan**
4. Log in to [https://console.aws.amazon.com](https://console.aws.amazon.com)

✅ **Checkpoint**: You should now be on the AWS Management Console.

---

## 📘 Part 1: Create a Custom VPC

1. Go to **VPC Dashboard** > **Your VPCs** > **Create VPC**
2. Choose **VPC only**
3. Set:
   - **Name**: `CloudPlus-VPC`
   - **IPv4 CIDR block**: `10.0.0.0/16`
   - Leave other settings default
4. Click **Create VPC**

---

## 🛠️ Part 2: Create a Public Subnet

1. Go to **Subnets** > **Create Subnet**
2. Set:
   - **Name**: `Public-Subnet`
   - **VPC**: `CloudPlus-VPC`
   - **Availability Zone**: (Pick any)
   - **CIDR Block**: `10.0.1.0/24`
3. Click **Create Subnet**

---

## 🔌 Part 3: Attach Internet Gateway

1. Go to **Internet Gateways** > **Create internet gateway**
   - Name: `CloudPlus-IGW`
2. Click **Attach to VPC** > Select `CloudPlus-VPC`

---

## 🌐 Part 4: Create Route Table and Add Route

1. Go to **Route Tables** > **Create route table**
   - Name: `Public-RT`
   - VPC: `CloudPlus-VPC`
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
   - VPC: `CloudPlus-VPC`
2. Inbound Rules:
   - **SSH** (22) > My IP
   - **HTTP** (80) > Anywhere (0.0.0.0/0)
   - **HTTPS** (443) > Anywhere (0.0.0.0/0)
3. Save security group

---

## 🗝️ Part 6: Create a Key Pair

1. Go to **EC2 Dashboard** > **Key Pairs** > **Create Key Pair**
   - Name: `cloudplus-key`
   - File format: `.pem`
   - Download and **save the file securely**

---

## 🚀 Part 7: Launch EC2 Instance

1. Go to **EC2 Dashboard** > **Launch Instance**
2. Configure:
   - Name: `CloudPlus-WebServer`
   - AMI: Ubuntu 22.04 or latest (Free Tier)
   - Instance Type: `t2.micro`
   - Key Pair: `cloudplus-key`
   - Network: `CloudPlus-VPC`
   - Subnet: `Public-Subnet`
   - Auto-assign Public IP: Enabled
   - Security Group: `WebAccess-SG`
   - DONT FORGET TO ENABLE Ip Auto addressing and don't use the default VPC (Use the one you made by clicking edit)
3. Click **Launch Instance**

---
## 🧩 Part 8: Use EC2 Instance Connect (AWS Console)

1. Go to EC2 Dashboard > Instances

2. Select your instance

3. Click Connect

4. Choose the EC2 Instance Connect tab

5. Click Connect again to launch a browser-based SSH session

✅ No need to upload your .pem key — Instance Connect uses temporary public keys.

## 💻 Option 2: SSH into Your Instance
(Install https://git-scm.com/downloads/win for windows, set it up and utilize git bash)
1. Get the **public IP** from the EC2 instance details
2. In your terminal:
   ```bash
   chmod 400 cloudplus-key.pem
   ssh -i cloudplus-key.pem ubuntu@<your-public-ip>
   ```
   (Use `ec2-user` for Amazon Linux)

---

## ✨ Part 9: Test Web Server (Optional)

1. SSH into the instance
2. Install Apache:
   ```bash
   sudo apt update
   sudo apt install apache2 -y
   sudo systemctl start apache2
   sudo systemctl enable apache2
   ```
3. Visit `http://<your-public-ip>` in your browser — you should see your message.

---

## 🧹 Part 10: Clean Up 
- Terminate EC2 instance
- Delete VPC and associated resources that were created throughotu the lab.

---

## ℹ️ Notes:
- Default VPC should **not be used** for this lab.
- Default Subnet should **not be used** for this lab.
