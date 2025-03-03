# AWS Services Overview for Email Notification System
![Screenshot from 2025-03-01 21-17-51](https://github.com/user-attachments/assets/d68a284c-277c-416f-bd92-fba1ee284c58)

## 1. **Amazon Simple Email Service (SES)**
- **Purpose**: Send transactional emails (e.g., reminders) securely and at scale.  
- **Utility**:  
  - **Sandbox Mode**: Ensures emails are sent only to verified addresses (prevents spam).  
  - **High Deliverability**: Guarantees emails reach inboxes instead of spam folders.  
  - **Verified Identities**: Validates sender ("Sending Address") and recipient ("Customer Address") emails.  

---

## 2. **AWS Lambda**  
- **Purpose**: Execute serverless code to handle email workflows.  
- **Utility**:  
  - **`email_reminder_lambda`**: Generates and sends emails via SES when triggered by AWS Step Functions.  
  - **`api_lambda`**: Processes API requests and triggers the state machine workflow.  
  - **Cost-Effective**: Pay only for compute time used during execution.  

---

## 3. **AWS Step Functions**  
- **Purpose**: Orchestrate serverless workflows.  
- **Utility**:  
  - **State Machine**: Coordinates timed delays (e.g., reminders) and triggers the email Lambda function.  
  - **Error Handling**: Manages retries and failures in the workflow.  

---

## 4. **Amazon API Gateway**  
- **Purpose**: Expose APIs to interact with the backend.  
- **Utility**:  
  - **REST API**: Routes frontend requests to the `api_lambda` function.  
  - **CORS Support**: Enables secure communication between the S3-hosted frontend and backend.  

---

## 5. **Amazon S3**  
- **Purpose**: Host the static frontend application.  
- **Utility**:  
  - **Static Website Hosting**: Stores HTML, CSS, and JavaScript files.  
  - **Public Access**: Configured to serve the website globally.  

---

## 6. **IAM Roles**  
- **Purpose**: Grant secure permissions to AWS resources.  
- **Utility**:  
  - **Lambda Execution Role**: Allows Lambda to access SES, SNS, and CloudWatch Logs.  
  - **State Machine Role**: Permits Step Functions to invoke Lambda and interact with SNS.  

---

## 7. **AWS CloudFormation**  
- **Purpose**: Automate infrastructure setup.  
- **Utility**:  
  - **Templates**: Deploys IAM roles and policies quickly, ensuring consistency and reducing manual errors.  

---

## 🔑 **Key Workflow for Email Delivery**  
```plaintext
Frontend (S3) → User submits a request via the web app  
               ↓  
API Gateway → Routes the request to `api_lambda`  
               ↓  
Step Functions → Triggers a timer and executes `email_reminder_lambda`  
               ↓  
Lambda + SES → Generates and sends the email to the verified recipient  
