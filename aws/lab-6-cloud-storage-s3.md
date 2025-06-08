# 🧪 Lab 6: Cloud Storage Management (AWS S3)

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Learn to provision and manage cloud storage resources using AWS S3, configure access, and enable lifecycle policies.

---

## 🎯 Goals

- Create and configure an S3 bucket  
- Upload and manage files  
- Set up access policies  
- Enable versioning and lifecycle rules  

---

## 🪣 Part 1: Create an S3 Bucket

1. Navigate to **S3** in the AWS Console  
2. Click **Create bucket**
   - Bucket name: `cloudplus-storage-lab`
   - Region: Same as EC2
   - Uncheck **Block all public access** (if testing public access)
   - Leave other defaults → **Create bucket**

✅ **Checkpoint**: You now have an S3 bucket created.

---

## 📁 Part 2: Upload and Manage Files

1. Click your bucket → **Upload**  
2. Add files or folders from your local machine  
3. Click **Upload**  

To verify:
- Click on uploaded object
- Copy the **Object URL**
- Paste in browser (if public access allowed)

✅ **Checkpoint**: File successfully uploaded and viewable.

---

## 🔐 Part 3: Bucket Policies and Access Control

1. Go to **Permissions** tab in your bucket  
2. Add a bucket policy like this (update your bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::cloudplus-storage-lab/*"
    }
  ]
}
```

✅ **Checkpoint**: Your bucket is now publicly accessible for GET requests.

---

## 🔄 Part 4: Enable Versioning and Lifecycle

1. Go to **Properties** tab  
2. Scroll to **Bucket Versioning** → Enable  
3. Go to **Lifecycle Rules** → Create rule  
   - Name: `delete-old-versions`
   - Apply to all objects
   - Add transition/delete rule: Delete previous versions after 30 days

✅ **Checkpoint**: Your bucket has versioning and cleanup automation.

---

## ✅ Lab Complete
