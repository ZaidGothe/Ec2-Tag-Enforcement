# Project Report: EC2 Tagging Enforcement via IAM

## 1. Introduction

Cloud cost management is a critical challenge for enterprise organizations. Without proper resource tagging, it becomes impossible to track ownership, allocate costs accurately, and enforce governance policies.

This project implements tag enforcement using AWS IAM Policies and validates the behavior by attempting EC2 instance launches both with and without the required tags.

## 2. Objectives

- Apply AWS tagging best practices
- Configure a tag enforcement policy using AWS IAM
- Demonstrate successful instance launch when all mandatory tags are present
- Demonstrate that launching without required tags is denied

## 3. Required Tags

| Tag Key    | Purpose                     |
|------------|-----------------------------|
| `Name`     | Instance owner name         |
| `emailID`  | Owner contact email         |
| `phoneNo`  | Owner contact phone         |
| `Place`    | Owner location/city         |

## 4. Policy Design

An IAM Deny Policy blocks `ec2:RunInstances` whenever any of the four mandatory tags are missing. It uses the `aws:RequestTag` condition key with a `Null` operator.

The `Null` operator returns `true` when a key is absent from the request context. Since multiple condition keys apply logical AND by default, the policy denies launch if **any single tag** is missing.

## 5. Test Results

| Test Scenario                     | Expected | Actual      |
|----------------------------------|----------|-------------|
| Launch WITHOUT required tags     | DENIED   | ✅ DENIED   |
| Launch WITH all 4 required tags  | SUCCESS  | ✅ SUCCESS  |

**Failure error:** `User is not authorized to perform ec2:RunInstances`

## 6. Key Learnings

1. IAM **Deny** policies always take precedence over Allow policies.
2. The `Null` condition operator effectively enforces the presence of request tags.
3. Tags must target the `Instances` resource type to be evaluated at launch time.
4. Always terminate test instances immediately to avoid unexpected billing charges.

## 7. Conclusion

The project successfully demonstrates a lightweight, IAM-native approach to enforce cloud governance through mandatory EC2 tagging. This pattern can be extended to other AWS resource types and scaled across an AWS Organization using Service Control Policies (SCPs).

