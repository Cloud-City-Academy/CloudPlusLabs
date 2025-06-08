# 🧪 Lab 2: Cloud Deployment Strategies (AWS, RHEL) (OPTIONAL LAB)

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Understand deployment models and implement a simple blue/green deployment strategy using AWS and RHEL.

---

## 🎯 Goals

- Review cloud deployment models (Public, Private, Hybrid)  
- Perform a basic blue/green deployment using EC2 and Route 53  
- Understand rollback strategies and cutover techniques  

---

## 📚 Part 1: Review Cloud Deployment Models

| Model   | Description                                 | Example                        |
|---------|---------------------------------------------|--------------------------------|
| Public  | Hosted by third-party cloud provider        | AWS, Azure, GCP                |
| Private | Exclusive cloud environment for one org     | On-prem OpenStack              |
| Hybrid  | Combines public and private clouds          | AWS Direct Connect + VMware    |

✅ **Checkpoint**: Know when to use each model depending on compliance, control, or scale.

---

## 🚀 Part 2: Launch Blue & Green EC2 Environments with RHEL

### 1. Launch Blue Instance:

- **AMI**: Red Hat Enterprise Linux 9 (Free Tier eligible)  
- **Instance Type**: `t2.micro`  
- **Name Tag**: `blue-server`  
- **User Data**:

```bash
#!/bin/bash
sudo dnf update -y
sudo dnf install -y httpd
echo "<h1>Blue Version - RHEL</h1>" | sudo tee /var/www/html/index.html
sudo systemctl enable httpd
sudo systemctl start httpd
```

---

## 🔁 Part 3: Create Green Environment and Test Cutover

### 1. Launch Green Instance:

- **AMI**: Red Hat Enterprise Linux 9  
- **Instance Type**: `t2.micro`  
- **Name Tag**: `green-server`  
- **User Data**:

```bash
#!/bin/bash
sudo dnf update -y
sudo dnf install -y httpd
echo "<h1>Green Version - RHEL</h1>" | sudo tee /var/www/html/index.html
sudo systemctl enable httpd
sudo systemctl start httpd
```

---

## 🌐 Part 4: Use Route 53 to Cut Over Traffic

### 1. Create a Hosted Zone

- Go to AWS Management Console → Route 53 → **Hosted Zones**
- Click **Create Hosted Zone**
- Click **Create Hosted Zone**
  - **Domain name**: `cca-yourname-cloudlab.duckdns.org` (or use a registered domain)
        - **Example**: `cca-yourname-cloudlab.duckdns.org`
  - **Type**: Public hosted zone

📌 *Note: If you do not own a domain, you can test using public IPs or consider using a free domain provider like duckdns for lab purposes.*

### 2. Create an A Record for the Blue Environment

- In your new hosted zone, click **Create record**
  - **Record name**: `cca-yourname-cloudlab.example.com`
  - **Record type**: A – IPv4 address
  - **Value**: Public IP of the Blue EC2 instance
  - **TTL**: 60 seconds (low TTL helps with testing cutovers)
  - Click **Create records**

### 3. Test the DNS Routing

- Use `dig` or `nslookup` to verify resolution:
```bash
#!/bin/bash
dig cca-yourname-cloudlab.example.com
```

## ✅ Lab Complete