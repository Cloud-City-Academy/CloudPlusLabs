# 🧪 Lab 3: Cloud Operations and Scaling (AWS, RHEL)

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Monitor cloud instances and configure auto-scaling using AWS Free Tier and RHEL.

---

## 🎯 Goals

- Configure logging and monitoring on a RHEL EC2 instance  
- Enable and test auto-scaling behavior  
- Understand metrics and alarms with CloudWatch  

---

## Setup RHEL EC2 Instance with Monitoring

1. Launch a new **EC2 Instance**:
   - AMI: Red Hat Enterprise Linux 9
   - Type: `t2.micro` (Free Tier)
   - Name Tag: `cloudplus-monitor`
   - Enable Public IP
   - In the **User Data** section, paste the following script:

```bash
#!/bin/bash
sudo dnf update -y
sudo dnf install -y httpd
echo "<h1>Cloud Monitoring - RHEL</h1>" | sudo tee /var/www/html/index.html
sudo systemctl enable httpd
sudo systemctl start httpd
```

2. Create and attach a **Security Group** allowing:
   - Port 22 (SSH)
   - Port 80 (HTTP)

3. Connect to your instance via SSH:
```bash
chmod 400 cloudplus-key.pem
ssh -i "cloudplus-key.pem" ec2-user@<your-public-ip>
```

---

## Enable CloudWatch Monitoring

4. Go to **CloudWatch** → **Log groups**:
   - Create a new log group: `cloudplus-monitoring`

5. Install the CloudWatch Agent:
```bash
sudo dnf install -y amazon-cloudwatch-agent
```

6. Create a configuration file:
```bash
sudo tee /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json > /dev/null <<EOF
{
  "agent": {
    "metrics_collection_interval": 60,
    "logfile": "/var/log/messages"
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/messages",
            "log_group_name": "cloudplus-monitoring",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
EOF
```

7. Start the CloudWatch Agent:
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl   -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json -s
```

✅ **Checkpoint**: Your instance logs should now appear in the CloudWatch console.

---

## Set Up Auto Scaling

8. Go to **EC2 > Launch Templates** → Create template:
   - Name: `cloudplus-template`
   - Use the same RHEL AMI, instance type, key pair, and security group as above

9. Go to **Auto Scaling > Auto Scaling Groups** → Create:
   - Use `cloudplus-template`
   - VPC and subnet: Same as previous labs
   - Desired capacity: 1
   - Min: 1, Max: 2

10. Add a **Scaling Policy**:
   - Target tracking: Average CPU ≥ 60%
   - Cooldown period: 300s

11. Generate CPU load to test scaling:
```bash
yes > /dev/null &
```

✅ **Checkpoint**: Watch the Auto Scaling group create a new instance when CPU usage rises.

---

## ✅ Lab Complete

You have now configured monitoring, logging, and an auto-scaling policy for RHEL on AWS Free Tier.