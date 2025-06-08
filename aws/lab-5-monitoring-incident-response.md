# 🧪 Lab 5: Cloud Monitoring & Incident Response (AWS)

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Set up cloud monitoring, create alarms, and simulate a basic incident response process using AWS CloudWatch.

---

## 🎯 Goals

- Enable detailed monitoring on EC2  
- Create CloudWatch alarms  
- Simulate high CPU usage  
- Automate notification with SNS  

---

## 📊 Part 1: Enable Monitoring on EC2

1. Go to **EC2 Dashboard**  
2. Select your instance (`cloudplus-ec2`)  
3. Under **Monitoring** tab, click **Enable Detailed Monitoring**

✅ **Checkpoint**: Instance now sends 1-minute metrics to CloudWatch.

---

## 🔔 Part 2: Create CloudWatch Alarm

1. Go to **CloudWatch** → **Alarms** → **Create alarm**  
2. Choose EC2 → `cloudplus-ec2` → Metric: `CPUUtilization`  
3. Conditions:
   - Threshold: `CPUUtilization > 70% for 2 datapoints within 5 minutes`

4. Notification:
   - Create new SNS topic → `cloud-alerts`  
   - Email subscription: your email address  
   - Confirm the subscription from your inbox

✅ **Checkpoint**: Alarm and notification system are ready.

---

## 🔥 Part 3: Simulate High CPU Usage

1. SSH into your EC2 instance:
```bash
ssh -i "cloudplus-key.pem" ec2-user@<your-public-ip>
```

2. Install stress tool:
```bash
sudo amazon-linux-extras install epel -y
sudo yum install -y stress
```

3. Run stress test:
```bash
stress --cpu 2 --timeout 300
```

✅ **Checkpoint**: Alarm should trigger within 5 minutes and send an email.

---

## 🛠️ Part 4: Review Alarm and Take Action

1. Go back to **CloudWatch** → **Alarms**  
2. View status change and notifications  
3. Take manual action (e.g., reboot instance or notify team)

✅ **Checkpoint**: You’ve simulated an incident and verified your alert workflow.

---

## ✅ Lab Complete
