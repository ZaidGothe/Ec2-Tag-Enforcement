# Ec2-Tag-Enforcement
This project demonstrates how to enforce mandatory tagging on AWS EC2 instances using IAM Deny Policies.  It ensures that no EC2 instance can be launched without required tags, helping organizations maintain proper cloud governance and cost tracking.


# EC2 Instance Launch with Enforced Tagging Policy

This project demonstrates how to enforce mandatory tagging on AWS EC2 instances using IAM Deny Policies, ensuring every virtual machine is properly identified and governed before launch.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Required Tags](#required-tags)
- [IAM Policy](#iam-policy)
- [Setup Guide](#setup-guide)
- [Test Results](#test-results)
- [Project Structure](#project-structure)

---

## Overview

Without proper resource tagging, it becomes impossible to track resource ownership, allocate costs accurately, and enforce governance policies in AWS.

This project:
- Configures a tag enforcement policy using **AWS IAM**
- Validates behavior by launching EC2 instances **with** and **without** required tags
- Proves the deny policy blocks untagged launches and allows tagged ones

---

## Required Tags

Every EC2 instance must have the following four tags at launch:

| Tag Key    | Description              |
|------------|--------------------------|
| `Name`     | Instance owner name      |
| `emailID`  | Owner email address      |
| `phoneNo`  | Owner phone number       |
| `Place`    | Owner location/city      |

---

## IAM Policy

The deny policy is at [`iam-policies/ec2-tag-enforcement-policy.json`](iam-policies/ec2-tag-enforcement-policy.json).

It blocks `ec2:RunInstances` whenever **any** of the four mandatory tags are missing, using the `aws:RequestTag` condition key with a `Null` check.

---

## Setup Guide

### Step 1 — Log in to AWS Console
- Go to https://console.aws.amazon.com
- Select region: `ap-south-1` (Mumbai)

### Step 2 — Create the IAM Deny Policy
1. Navigate to **IAM → Policies → Create Policy**
2. Click the **JSON** tab and paste the contents of `ec2-tag-enforcement-policy.json`
3. Name it: `EC2-Tag-Enforcement-Policy`
4. Click **Create Policy**

### Step 3 — Attach Policy to IAM User
1. Go to **IAM → Users → Select your test user**
2. Click **Add Permissions → Attach Policies Directly**
3. Attach both `AmazonEC2FullAccess` and `EC2-Tag-Enforcement-Policy`

### Step 4 — Test Launch WITHOUT Tags (Failure)
1. Go to **EC2 → Instances → Launch Instances**
2. Fill in all settings but **skip all tags**
3. Click **Launch Instance** — authorization error expected

### Step 5 — Launch WITH All Required Tags (Success)
1. Go to **EC2 → Instances → Launch Instances**
2. Under **Name and Tags**, add all 4 required tags
3. Set **Resource Type** to `Instances` for each tag
4. Use: `Amazon Linux 2023 AMI`, `t3.micro`, default security group
5. Click **Launch Instance** — should succeed

### Step 6 — Verify Tags & Terminate
1. Select the instance → click **Tags** tab to confirm all 4 tags
2. Right-click → **Terminate Instance** to avoid billing charges

---

## Test Results

| Test Scenario                     | Expected | Actual      |
|----------------------------------|----------|-------------|
| Launch WITHOUT required tags     | DENIED   | ✅ DENIED   |
| Launch WITH all 4 required tags  | SUCCESS  | ✅ SUCCESS  |

---

## Project Structure

```
ec2-tagging-policy/
├── README.md
├── .gitignore
├── iam-policies/
│   └── ec2-tag-enforcement-policy.json
├── docs/
│   └── project-report.md
└── screenshots/
    └── (place AWS console screenshots here)
```

---

## Technologies Used

- **AWS EC2** — Virtual machine service
- **AWS IAM** — Identity and Access Management
- **Tag-based Governance** — Resource tagging enforcement

---

## Author

**Zaid Gothe** — MCA Intern, FortuneCloud Technologies  
📧 gothe.zaid@gmail.com | 📍 Pune
