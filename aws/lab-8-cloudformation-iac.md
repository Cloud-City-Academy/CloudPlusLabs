# 🧪 Lab 8: Infrastructure as Code (IaC) with AWS CloudFormation

**Course:** CompTIA Cloud+ CV0-004  
**Objective:** Learn to provision infrastructure using IaC tools—in this lab, AWS CloudFormation.

---

## 🎯 Goals

- Understand Infrastructure as Code (IaC) principles  
- Create a CloudFormation stack to deploy a simple EC2 infrastructure  
- Review and delete the stack

---

## 📘 Part 1: Introduction to CloudFormation

**CloudFormation** allows you to model and provision AWS resources using JSON or YAML templates. These templates are declarative and reusable, helping enforce consistency and compliance.

✅ **Checkpoint**: Know what a CloudFormation template is and when to use one.

---

## ⚙️ Part 2: Create a Simple CloudFormation Template

Create a file named `simple-ec2.yaml`:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple EC2 instance with security group

Resources:
  EC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c02fb55956c7d316  # Amazon Linux 2 in us-east-1
      KeyName: cloudplus-key
      SecurityGroups:
        - !Ref EC2SecurityGroup

  EC2SecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Properties:
      GroupDescription: Enable SSH
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
```

✅ **Checkpoint**: Validate your YAML file using a linter or CloudFormation designer.

---

## 🚀 Part 3: Deploy the Template via Console

1. Go to **CloudFormation** in AWS Console  
2. Choose **Create Stack → With new resources (standard)**  
3. Upload `simple-ec2.yaml`  
4. Name the stack `CloudPlusLab8`  
5. Follow prompts and **Create Stack**

✅ **Checkpoint**: A new EC2 instance and security group are created.

---

## 🧹 Part 4: Clean Up

1. In CloudFormation → select the `CloudPlusLab8` stack  
2. Choose **Delete**  
3. Confirm to tear down the resources

✅ **Checkpoint**: Stack and all associated resources are removed.

---

## ✅ Lab Complete