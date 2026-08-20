# royal-orchid-aws-disaster-recovery
Multi-region AWS disaster recovery solution for Royal Orchid Hotel Group.
# Royal Orchid Hotel Group – AWS Disaster Recovery Solution

## 📌 Project Overview

This project implements a multi-region disaster recovery solution for Royal Orchid Hotel Group using Amazon Web Services (AWS).

The solution is designed to maintain website availability during server failures by deploying a primary web server in the Mumbai AWS Region and a disaster recovery web server in the Singapore AWS Region.

Amazon Route 53 health checks and DNS failover are used to automatically redirect users to the disaster recovery server when the primary server becomes unavailable.

---

## 🎯 Project Objectives

- Provide high availability for the hotel website
- Implement disaster recovery across multiple AWS Regions
- Detect primary server failure using health checks
- Automatically redirect traffic to the DR server
- Maintain service availability during server failures
- Implement backup and recovery mechanisms
- Test and validate the disaster recovery process

---

## ☁️ AWS Services Used

| AWS Service | Purpose |
|---|---|
| Amazon EC2 | Hosts the primary and disaster recovery web servers |
| Amazon Route 53 | DNS management and failover routing |
| Route 53 Health Checks | Monitors server availability |
| Amazon S3 | Stores backup data |
| AWS Backup | Automated backup and recovery |
| Amazon VPC | Provides network infrastructure |
| Security Groups | Controls network traffic |
| Ubuntu Linux | Operating system for EC2 servers |
| Apache | Web server |

---

## 🏗️ Architecture

The project uses two AWS Regions:

### Primary Environment

**AWS Mumbai Region (ap-south-1)**

- Ubuntu EC2 instance
- Apache web server
- Primary website

### Disaster Recovery Environment

**AWS Singapore Region (ap-southeast-1)**

- Ubuntu EC2 instance
- Apache web server
- Disaster recovery website

### DNS and Failover

Amazon Route 53 manages the domain and uses health checks to monitor the primary server.

If the Mumbai server becomes unavailable, Route 53 redirects traffic to the Singapore disaster recovery server.

---

## 🔄 Disaster Recovery Workflow
## Architecture

The solution uses a multi-region disaster recovery architecture with Mumbai as the primary environment and Singapore as the disaster recovery (DR) environment.

```text
                         USERS
                           |
                           v
                  royalorchid.co.in
                           |
                           v
                    AMAZON ROUTE 53
                    DNS + Health Check
                           |
                +----------+----------+
                |                     |
             HEALTHY               FAILURE
                |                     |
                v                     v
        MUMBAI PRIMARY         SINGAPORE DR
           EC2 + Apache          EC2 + Apache
                |                     |
                v                     v
        PRIMARY WEBSITE          DR WEBSITE


       DATA BACKUP / RECOVERY
                  |
        +---------+---------+
        |                   |
        v                   v
   Amazon S3            AWS Backup
```
## 🧩 Architecture Components

### Amazon Route 53

Provides DNS management and failover routing for the hotel domain.

### Route 53 Health Check

Monitors the availability of the primary Mumbai web server.

### Mumbai EC2 – Primary

The primary production web server running Ubuntu Linux and Apache.

### Singapore EC2 – Disaster Recovery

The backup web server running Ubuntu Linux and Apache. It serves the website when the Mumbai server becomes unavailable.

### Amazon S3

Stores critical hotel data such as reservation and billing information.

### AWS Backup

Provides automated backup and recovery of protected AWS resources.

---

## ✅ Normal Operation

Under normal conditions, Route 53 directs users to the primary Mumbai EC2 instance.

```text
User
  |
  v
Route 53
  |
  | Health Check: HEALTHY
  v
Mumbai EC2
  |
  v
Apache
  |
  v
Hotel Website
```

This allows the hotel website to remain accessible even when the primary server is unavailable.

---

## 💾 Backup and Recovery

Amazon S3 is used to store critical hotel data in the production environment.

The S3 bucket used in this project is:

`roh-dr-backup-bucket`

AWS Backup was configured to automatically back up the production S3 bucket.

### Backup Configuration

| Configuration | Details |
|---|---|
| Backup Resource | `roh-dr-backup-bucket` |
| Backup Type | Automated |
| Storage Tier | Warm |
| Backup Status | Completed |
| AWS Backup Plan | `roh-backup-plan` |

### Recovery Test

A recovery point was successfully created using AWS Backup and subsequently restored.

The restored S3 data included:

- `billing.txt`
- `reservations.txt`

The restore job completed successfully, confirming that the backup data could be recovered.

---

## 🧪 Disaster Recovery Testing

The disaster recovery mechanism was tested by intentionally stopping the primary Mumbai EC2 instance.

### Test Scenario

1. The Mumbai EC2 instance was running normally.
2. Route 53 health check reported the primary server as healthy.
3. The Mumbai EC2 instance was stopped to simulate a server failure.
4. Route 53 detected that the primary server was unavailable.
5. DNS failover redirected traffic to the Singapore EC2 instance.
6. The Singapore Apache server successfully served the disaster recovery website.
7. The domain was tested again and the website successfully loaded through the Singapore server.
8. The Mumbai EC2 instance was subsequently restored.

### Test Result

**Disaster recovery failover was successfully tested and validated.**

---

## 📊 Validation Results

| Test | Result |
|---|---|
| Mumbai EC2 primary server | ✅ Successful |
| Route 53 health check | ✅ Healthy |
| Primary server failure simulation | ✅ Successful |
| Route 53 DNS failover | ✅ Successful |
| Singapore DR server | ✅ Successful |
| DR website accessibility | ✅ Successful |
| S3 backup | ✅ Successful |
| AWS Backup recovery point | ✅ Successful |
| S3 restore operation | ✅ Completed |
| `billing.txt` restoration | ✅ Available |
| `reservations.txt` restoration | ✅ Available |

---

## 🔐 Security

The project uses AWS security mechanisms including:

- Amazon VPC for network infrastructure
- EC2 Security Groups for controlling network traffic
- IAM roles for AWS Backup
- S3 access controls
- AWS-managed encryption for backup data
- Route 53 health checks for endpoint monitoring

Sensitive information such as passwords, AWS access keys, secret credentials, and private configuration data are not included in this repository.

---

## 🛠️ Technologies Used

- Amazon Web Services (AWS)
- Amazon EC2
- Amazon Route 53
- Route 53 Health Checks
- Amazon S3
- AWS Backup
- Amazon VPC
- Security Groups
- Ubuntu Linux
- Apache Web Server
- GitHub

---

## 📚 Project Learning

This project provided practical experience with:

- Multi-region AWS architecture
- EC2 deployment and configuration
- Ubuntu Linux server administration
- Apache web server configuration
- DNS management
- Route 53 failover routing
- Route 53 health checks
- Amazon S3 data management
- AWS Backup
- Backup restoration
- Disaster recovery testing
- Cloud infrastructure troubleshooting
- GitHub project documentation

---

## 🚀 Future Improvements

The current disaster recovery architecture can be extended with:

- Amazon CloudFront
- AWS WAF
- Application Load Balancer
- Infrastructure as Code using Terraform or AWS CloudFormation
- Automated EC2 recovery
- Cross-region S3 replication
- Centralized CloudWatch monitoring
- Automated disaster recovery testing
- Database replication

---

## 👩‍💻 Project Author

**Aathi Sakthi Kalyani**

B.E. Computer Science and Engineering

GitHub:  
https://github.com/Aathi-Sakthi-Kalyani

---

## ⭐ Project Summary

This project demonstrates a multi-region AWS disaster recovery architecture for Royal Orchid Hotel Group.

The primary website is hosted on an EC2 instance in the Mumbai AWS Region, while a second EC2 instance in the Singapore AWS Region acts as the disaster recovery server.

Route 53 health checks and DNS failover were used to detect primary server failure and redirect users to the Singapore DR environment.

The disaster recovery process was successfully validated by stopping the Mumbai EC2 instance and confirming that the website remained accessible through the Singapore server.

AWS Backup was also tested successfully by creating and restoring an S3 recovery point containing the project's hotel reservation and billing data.

ssfully tested, with billing.txt and reservations.txt successfully restored using AWS Backup.
