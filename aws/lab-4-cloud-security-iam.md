# 🧪 Lab 4: Cloud Security & IAM (AWS)

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Understand identity and access management (IAM), least privilege, and create secure access control policies in AWS.

---

## 🎯 Goals

- Create IAM Users and Groups  
- Attach Policies (Least Privilege)  
- Use MFA and Secure Login Practices  
- Test Role-based Access via Console and CLI  

---

## 🔐 Part 1: Create IAM Group and Policy

1. Go to **IAM** → **User Groups** → Create Group  
   - Group name: `CloudPlusAdmins`

2. Attach policy:  
   - Click **Add permissions**  
   - Select **AdministratorAccess** (for demo) or create custom policy with limited access

✅ **Checkpoint**: Group created with proper permissions.

---

## 👤 Part 2: Create IAM User

1. Go to **Users** → **Add users**  
   - Username: `student1`  
   - Access Type: Password + Programmatic Access  
   - Assign user to group: `CloudPlusAdmins`

2. Set custom password and **require password reset on first login**

✅ **Checkpoint**: User created with password and CLI access.

---

## 🧪 Part 3: Enable MFA for IAM User

1. Log in as `student1`  
2. Go to **My Security Credentials**  
3. Click **Activate MFA** → Choose virtual MFA device (e.g., Google Authenticator)  
4. Scan QR code and verify tokens  

✅ **Checkpoint**: MFA is now enabled for user login.

---

## 🔧 Part 4: Test Permissions via CLI

1. Configure CLI:
```bash
aws configure
# Provide Access Key ID, Secret, region, format
```

2. Test permissions:
```bash
aws s3 ls
```

3. Try to create a restricted resource (if custom policy was used) and verify denied actions

✅ **Checkpoint**: IAM policies and MFA enforced correctly.

---

## ✅ Lab Complete
