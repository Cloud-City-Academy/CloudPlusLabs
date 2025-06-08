# 🧪 Lab 1: Cloud Architecture Fundamentals (AWS)

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Understand how to create and configure basic cloud infrastructure using AWS Free Tier.

---

## 🎯 Goals

- Create a VPC and subnet
- Attach an Internet Gateway
- Launch a virtual machine (EC2)
- Connect to the instance via SSH

---

## 📝 Part 1: Register for AWS Free Tier

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

## 🌐 Part 2: Create a VPC with Public Subnet

1. Navigate to **VPC** service → "Your VPCs" → **Create VPC**
   - Name: `cloudplus-vpc`
   - IPv4 CIDR: `10.0.0.0/16`
   - Leave rest as default → Create

2. Go to **Subnets** → **Create Subnet**
   - Name: `public-subnet`
   - VPC: `cloudplus-vpc`
   - AZ: e.g., `us-east-1a`
   - CIDR: `10.0.1.0/24`

3. Go to **Internet Gateways** → Create:
   - Name: `cloudplus-igw`
   - Attach to: `cloudplus-vpc`

4. Edit Route Table:
   - Use default Route Table attached to VPC
   - Add route: `0.0.0.0/0 → cloudplus-igw`
   - Associate with `public-subnet`

✅ **Checkpoint**: You have a VPC with internet access via a public subnet.

---

## 🖥️ Part 3: Launch EC2 Instance

1. Go to **EC2 Dashboard** → Launch Instance
   - Name: `cloudplus-ec2`
   - AMI: Amazon Linux 2023
   - Type: `t2.micro` (Free Tier)
   - Key Pair: Create new → `cloudplus-key` → Download `.pem`
   - Network: `cloudplus-vpc`, Subnet: `public-subnet`
   - Auto-assign Public IP: Yes
   - Firewall: Allow SSH (port 22)

2. Launch Instance

✅ **Checkpoint**: Your instance is running with a public IP.

---

## 🔌 Part 4: Connect to EC2

1. Open terminal and run:
```bash
chmod 400 cloudplus-key.pem
ssh -i "cloudplus-key.pem" ec2-user@<your-public-ip>
```

---

## 🧹 Part 5 (Optional): Clean Up Resources

To avoid unnecessary charges from AWS, you should terminate any running resources after the lab.

### ✅ Terminate EC2 Instance

1. Go to the **EC2 Dashboard**
2. Select your instance (`cloudplus-ec2`)
3. Click **Instance state → Terminate instance**
4. Confirm the termination

✅ **Checkpoint**: Your instance should now show a status of "shutting down" and will soon be removed.

---

## ✅ Lab Complete

You’ve successfully:

- Set up your first cloud infrastructure  
- Created and configured a VPC, subnet, and internet gateway  
- Launched and connected to a cloud-hosted virtual machine (EC2)  
- Practiced fundamental cloud networking and security setup  
- (Optionally) Cleaned up cloud resources to avoid unnecessary charges  

---

## 🧠 Knowledge Check (Optional)

Before moving on, ask yourself:

- Can I explain the purpose of a VPC and subnet?  
- Do I understand what an internet gateway does?  
- Can I SSH into an EC2 instance securely?  
- Do I know how to terminate resources I no longer need?  

---

## 📎 Additional Resources

- [AWS Free Tier Documentation](https://aws.amazon.com/free/)  
- [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/index.html)  
- [VPC Networking Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)  

---

## 📌 Recommended Next Steps

- Proceed to **Lab 2: Cloud Deployment Strategies (Blue/Green)**  
- Explore more about IAM roles and security groups  
- Try launching a RHEL instance instead of Amazon Linux  

Happy Cloud Learning! ☁️🚀
