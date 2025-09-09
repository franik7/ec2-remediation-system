# EC2 Monitoring and Remediation System – ServiceNow Implementation

## Overview
This project implements a **semi-automated incident response system** on ServiceNow for monitoring and remediating AWS EC2 instances.  
It was designed as part of a Netflix case study where reliable streaming infrastructure is critical for over 260 million subscribers.

The system integrates with a simulated **AWS Integration Server** that continuously monitors EC2 health and updates ServiceNow tables.  
When an EC2 instance goes **OFF**, the workflow automatically triggers incident handling, knowledge retrieval, and Slack notifications.  
DevOps engineers can then perform **one-click remediation** through a UI Action in ServiceNow, with all remediation attempts logged for audit.

---

## System Architecture
**Flow of events:**
1. AWS Integration Server monitors EC2 instances and updates the **EC2 Instance Table** in ServiceNow.  
2. When an instance goes **OFF**, a **Flow Designer Workflow** is triggered.  
3. The flow:
   - Creates a remediation log entry  
   - Runs **AI Search** to find knowledge base articles with remediation instructions  
   - Sends a Slack notification with alert + KB link  
4. An engineer reviews the KB instructions and uses the **Trigger EC2 Remediation** button.  
5. The UI Action calls the **EC2RemediationHelper Script Include**, which sends an outbound REST request to the AWS Integration Server.  
6. The EC2 instance is remediated (turned back ON), and details are captured in the **Remediation Log Table**.  

---

## Key Components
- **Custom Tables**:  
  - EC2 Instance Table (monitored instances)  
  - Remediation Log Table (audit trail of remediation attempts)  
- **UI Action**: Trigger EC2 Remediation  
- **Script Include**: EC2RemediationHelper (handles outbound REST to AWS Integration Server)  
- **Flow Designer Workflow**: Handles incident trigger, AI Search, and Slack notification  
- **Slack Integration**: Posts alerts and KB links via webhook  
- **Knowledge Base**: Stores remediation instructions retrievable via AI Search  

---

## Features
- Automatic detection of EC2 instance failures  
- AI-powered knowledge retrieval with relevant remediation steps  
- Slack notifications for real-time incident awareness  
- Manual one-click remediation via ServiceNow UI Action  
- Comprehensive logging of all remediation attempts  

---

## Deliverables
- System architecture diagram (Diagram.png)  
- Update set export (`ec2-remediation-system.xml`)  
- UI Action + Script Include (JavaScript files)  
- README documentation  

---

> ⚠️ **Note**: This repo is for educational purposes and simulates integration with AWS via a staged integration server.
