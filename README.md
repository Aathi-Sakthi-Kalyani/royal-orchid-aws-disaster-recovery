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

```text
                         User
                           |
                           v
                    royalorchid.co.in
                           |
                           v
                     Amazon Route 53
                           |
                    Health Check
                           |
                +----------+----------+
                |                     |
             Healthy                Failed
                |                     |
                v                     v
        Mumbai EC2              Singapore EC2
          PRIMARY                    DR
                |                     |
                v                     v
             Apache                Apache
                |                     |
                +----------+----------+
                           |
                           v
                     Hotel Website
