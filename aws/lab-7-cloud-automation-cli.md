# 🧪 Lab 7: Cloud Automation with AWS CLI

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Automate cloud resource management using the AWS CLI.

---

## 🎯 Goals

- Install and configure AWS CLI  
- Use CLI to provision an EC2 instance  
- Automate VPC and subnet creation  
- Understand scripting basics for automation  

---

## 🛠️ Part 1: Install and Configure AWS CLI

### 1. Install AWS CLI
Follow the instructions for your OS:  
- [https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

### 2. Configure CLI
```bash
aws configure
```
Provide:
- AWS Access Key ID  
- AWS Secret Access Key  
- Region (e.g., `us-east-1`)  
- Output format: `json`

✅ **Checkpoint**: Run `aws ec2 describe-instances` to test.

---

## 🧱 Part 2: Create a VPC, Subnet, and Internet Gateway

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block 10.0.1.0/24
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet-gateway-id <igw-id> --vpc-id <vpc-id>
```

> Replace `<vpc-id>` and `<igw-id>` with your actual values.

---

## 🚀 Part 3: Launch EC2 Instance with CLI

```bash
aws ec2 run-instances   --image-id ami-0abcdef1234567890   --count 1   --instance-type t2.micro   --key-name cloudplus-key   --security-group-ids <sg-id>   --subnet-id <subnet-id>   --associate-public-ip-address
```

✅ **Checkpoint**: Confirm instance in AWS Console or use CLI:
```bash
aws ec2 describe-instances
```

---

## 🧩 Part 4: Bash Script Example (Optional)

Save as `launch.sh`:
```bash
#!/bin/bash
# Create VPC
vpc_id=$(aws ec2 create-vpc --cidr-block 10.0.0.0/16 --query 'Vpc.VpcId' --output text)

# Other AWS CLI commands go here
echo "VPC created: $vpc_id"
```

Run with:
```bash
chmod +x launch.sh
./launch.sh
```

✅ **Lab Complete**
