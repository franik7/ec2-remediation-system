# EC2 Monitoring and Remediation System – ServiceNow Implementation

## System Overview
This project implements the **ServiceNow portion** of an EC2 Monitoring and Remediation System.  
The purpose is to provide Netflix’s DevOps team with a **centralized response system** that:  
- Detects when an EC2 instance fails (status = OFF)  
- Automatically triggers workflows in ServiceNow  
- Provides AI-powered remediation guidance from the Knowledge Base  
- Sends Slack notifications to alert engineers in real time  
- Allows engineers to remediate the instance with a **one-click UI Action**  
- Logs every remediation attempt in a dedicated table for auditing  

📸  ServiceNow application scope showing **EC2 Monitoring and Remediation** with tables included.  (assets/Diagram.png) 

---

## Implementation Steps
The ServiceNow portion included these key components:

1. **Custom Tables**  
   - `EC2 Instance` – stores instance name, ID, and status (`ON` or `OFF`).  
   - `Remediation Log` – tracks remediation attempts, payloads, responses, status codes, and success flags.  

   📸 *Screenshot*: EC2 Instance table with populated records (show ON/OFF status).  
   📸 *Screenshot*: Remediation Log table with sample log entry.  

2. **UI Action** – *Trigger EC2 Remediation*  
   - Added as a form button on EC2 Instance records.  
   - Executes client-side script using GlideAjax.  

   📸 *Screenshot*: EC2 Instance record form with the **Trigger EC2 Remediation** button.  

3. **Script Include** – *EC2RemediationHelper*  
   - Handles outbound REST requests via the Connection & Credential Store (CCS).  
   - Reads instance ID, calls the AWS Integration Server, and inserts log entries.  

   📸 *Screenshot*: Script Include definition in ServiceNow Studio.  

4. **Flow Designer Workflow**  
   - Trigger: When EC2 Instance status updates to OFF.  
   - Actions:  
     - Create log record  
     - Run AI Search against Knowledge Base  
     - Post Slack notification via webhook  

   📸 *Screenshot*: Flow Designer showing trigger + actions.  

5. **Knowledge Base Integration**  
   - Created KB article “EC2 Remediation when OFF” with keywords for AI Search discoverability.  

   📸 *Screenshot*: Published KB article content.  

6. **Connection & Credential Store**  
   - Alias: *AWS Integration Server C C Alias*  
   - Connection: Host + base path `/api/v1/queue/start`  
   - Credential: Basic Auth (admin user)  

   📸 *Screenshot*: Connection & Credential Alias configuration.  

---

## Architecture Diagram
The workflow is illustrated below:

📸 *Insert your diagram here (`Diagram.png`)*  

Flow of events:  
1. AWS Integration Server updates ServiceNow EC2 Instance table.  
2. Flow triggers when status = OFF.  
3. AI Search retrieves KB article.  
4. Slack webhook sends alert with KB link.  
5. Engineer clicks remediation UI Action.  
6. Script Include executes REST call.  
7. Remediation Log updated.  
8. Instance status returns to ON.  

---

## Optimization
During implementation, the following improvements were applied for reliability and efficiency:  
- **Scoped Application**: Ensured naming conventions matched AWS Integration Server requirements (`x_snc_ec2_monito_0`).  
- **Error Handling**: Script Include captures and logs all error responses (`400`, `401`, `404`) with payload.    
- **Slack Messaging**: Used `<URL|label>` formatting to shorten links and improve readability.  

---

## DevOps Usage
For Netflix DevOps engineers, the workflow is:  

1. **Detection**:  
   EC2 instance status is auto-updated in ServiceNow. If OFF, a Slack alert is sent.  

2. **Guidance**:  
   Slack message contains a direct link to the relevant KB article with remediation instructions.  

3. **Remediation**:  
   Engineer opens the EC2 Instance record and clicks the **Trigger EC2 Remediation** button.  

4. **Execution**:  
   Script Include executes outbound REST → AWS Integration Server.  

5. **Audit**:  
   Remediation Log captures the attempt, payload, response, and success flag.  

📸 *Screenshot*: Slack message showing KB + instance record link.  
📸 *Screenshot*: Updated EC2 record status after remediation (OFF → ON).  

---


