# 🧪 Lab x1: Deploy & Destroy EC2 Instance Using AWS CLI

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Learn how to use the AWS CLI to deploy and terminate an EC2 instance from the command line.

---

## 🎯 Goals

- Configure AWS CLI with proper credentials  
- Deploy a `t2.micro` instance with Amazon Linux 2023  
- SSH into the instance  
- Terminate the instance after verification  

---

## ⚙️ Configure AWS CLI

1. Open your terminal. Run:
   ```bash
   aws configure
   ```

2. Enter your AWS credentials:
   - **AWS Access Key ID**
   - **AWS Secret Access Key**
   - **Default region** (e.g., `us-east-1`)
   - **Default output format**: `json`

✅ **Checkpoint:** Run the following to verify connectivity:
```bash
aws ec2 describe-instances
```

---

## 🚀 Launch EC2 Instance via CLI

1. Create a key pair:
   ```bash
   aws ec2 create-key-pair --key-name cli-key --query 'KeyMaterial' --output text > cli-key.pem
   chmod 400 cli-key.pem
   ```

2. Find the latest Amazon Linux 2023 AMI:
   ```bash
   aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-6.1-x86_64 --region us-east-1
   ```

   Use the value under `Value` from the output as the AMI ID.

3. Create a security group:
   ```bash
   aws ec2 create-security-group --group-name cli-sg --description "CLI SG"
   ```

4. Allow SSH access:
   ```bash
   aws ec2 authorize-security-group-ingress \
     --group-name cli-sg \
     --protocol tcp \
     --port 22 \
     --cidr 0.0.0.0/0
   ```

5. Launch the EC2 instance:
   ```bash
   aws ec2 run-instances \
     --image-id <AMI-ID> \
     --count 1 \
     --instance-type t2.micro \
     --key-name cli-key \
     --security-groups cli-sg
   ```

🔍 **Note:** Replace `<AMI-ID>` with the actual value from step 2.

✅ **Checkpoint:** Use the following to get the public IP:
```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].PublicIpAddress" --output text
```

---

## 🔌 Connect to EC2 Instance

```bash
ssh -i "cli-key.pem" ec2-user@<public-ip>
```

✅ **Checkpoint:** You should now be connected to the EC2 instance.

---

## 💣 Terminate the EC2 Instance

1. Get the Instance ID:
   ```bash
   aws ec2 describe-instances --query "Reservations[*].Instances[*].InstanceId" --output text
   ```

2. Terminate the instance:
   ```bash
   aws ec2 terminate-instances --instance-ids <INSTANCE-ID>
   ```

3. Confirm it's terminated:
   ```bash
   aws ec2 describe-instances --instance-ids <INSTANCE-ID> --query "Reservations[*].Instances[*].State.Name"
   ```

✅ **Checkpoint:** Output should say `terminated`.

---

## ✅ Lab Complete

You successfully deployed and terminated an EC2 instance using only the AWS CLI.
