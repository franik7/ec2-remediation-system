# EC2 Monitoring and Remediation System – ServiceNow Implementation

## Overview
This repository contains the **ServiceNow implementation** for an EC2 Monitoring and Remediation System.  
The goal is to demonstrate how ServiceNow can be used to detect failing resources, notify engineers, and provide one-click remediation with audit logging.

The ServiceNow portion integrates with a simulated AWS Integration Server that writes EC2 instance health data into a custom table.  
All workflows, tables, actions, and notifications are built in ServiceNow.

---

## ServiceNow Components

- **Scoped Application**:  
  *EC2 Monitoring and Remediation* (scope: `x_snc_ec2_monito_0`)

- **Custom Tables**:  
  - **EC2 Instance Table** – stores instance name, ID, and current status (`ON` or `OFF`).  
  - **Remediation Log Table** – captures remediation attempts, request/response payloads, HTTP status, timestamps, and success flag.  

- **Flow Designer Workflow**:  
  Triggered when `Instance status = OFF`.  
  - Creates remediation log entry  
  - Runs AI Search to retrieve knowledge articles  
  - Sends Slack notification with KB link and instance reference  

- **Knowledge Base Integration**:  
  At least one KB article created with keywords (`EC2, server, instance, restart, AWS`) to support AI Search.  

- **UI Action**:  
  *Trigger EC2 Remediation* – a form button on EC2 Instance records. Calls a Script Include via GlideAjax.  

- **Script Include**:  
  *EC2RemediationHelper* – server-side logic that:  
  - Reads the instance ID from the EC2 Instance record  
  - Uses **Connection & Credential Store (CCS)** alias to fetch connection details  
  - Sends an outbound REST request to the integration server  
  - Logs the response in the Remediation Log table  

---

## Features
- Real-time detection of instance failures via table updates  
- Slack notifications with links to the affected instance tracker and relevant knowledge base articles  
- Engineer can remediate with a single click in ServiceNow  
- All remediation attempts are logged for auditing  

---

## Deliverables
- Update Set (`ec2-remediation-system.xml`) containing:  
  - EC2 Instance Table  
  - Remediation Log Table  
  - UI Action: Trigger EC2 Remediation  
  - Script Include: EC2RemediationHelper  
  - Flow Designer Workflow  
  - ACLs for tables and Script Include  
- System architecture diagram (`Diagram.png`)  
- README.md (this file)

---

> ⚠️ **Note**: This repo only covers the ServiceNow side of the project. The AWS Integration Server was provided as a simulated external system.
