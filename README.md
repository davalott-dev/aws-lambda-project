# AWS S3 → Lambda → SNS File Upload Notifier  

### **Project Overview**
This project demonstrates how to automate notifications when a file is uploaded to an Amazon S3 bucket using a **serverless AWS architecture**.  
It uses **S3**, **Lambda (Python)**, and **SNS** to send alerts automatically whenever a new file is added — a great example of **event-driven automation** in the cloud.

---

### **🎯 Purpose**
To showcase my ability to design and implement **AWS serverless workflows** that react to real-time events, handle permissions, and integrate multiple AWS services seamlessly.

---

### **🧩 Architecture**
1. **Amazon S3** – Stores uploaded files (e.g., images, documents).  
2. **AWS Lambda** – Automatically triggered when a new file is uploaded.  
3. **Amazon SNS** – Sends an email notification with file details to subscribers.  

```text
User Uploads File → S3 Event → Lambda Function → SNS Email Notification
```

**Architecture Diagram:**
```
        +-----------+
        |   User    |
        +-----------+
              |
              v
        +-----------+
        |    S3     |
        |  Bucket   |
        +-----------+
              |
       (Event Trigger)
              v
        +-----------+
        |  Lambda   |
        |  Function |
        +-----------+
              |
         Publishes Message
              v
        +-----------+
        |    SNS     |
        |   Topic    |
        +-----------+
              |
         Sends Notification
              v
        +-----------+
        |   Email   |
        +-----------+
```

---

### **⚙️ Technologies Used**
- **AWS S3**  
- **AWS Lambda (Python 3.9)**  
- **AWS SNS**  
- **IAM Roles and Permissions**  
- **CloudWatch Logs**

---

### **📁 File Structure**
```
s3-lambda-sns-notifier/
│
├── lambda_function.py        # Lambda code
├── trigger-event.json        # Example event test payload
├── architecture-diagram.png  # (Optional visual)
└── README.md
```

---

### **Lambda Function (Python)**
```python
import json
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    sns = boto3.client('sns')

    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    message = f"File uploaded: {key} in bucket {bucket}"

    sns.publish(
        TopicArn='arn:aws:sns:YOUR_REGION:YOUR_ACCOUNT_ID:YOUR_TOPIC_NAME',
        Message=message,
        Subject='New File Uploaded to S3'
    )

    print("Event received:", json.dumps(event))
    print("Notification sent successfully!")
    return {'statusCode': 200, 'body': json.dumps('Notification sent!')}
```

---

### **How It Works**
1. **Create an S3 bucket** (e.g., `lambda-file-handler`).  
2. **Create a Lambda function** using the above Python code.  
3. **Create an SNS topic** and subscribe your email.  
4. **Attach the correct IAM role** so Lambda can access S3 and SNS.  
5. **Add an S3 trigger** for `beach.jpg` events to invoke the Lambda function.  
6. **Upload a file (e.g., beach.jpg)** to your bucket — and check your email for a notification!  

---

### **What I Learned**
- How AWS **event-driven architectures** work.  
- The importance of **IAM roles and permissions** in automation.  
- How to integrate multiple AWS services without using servers.  
- How to monitor and debug Lambda executions with **CloudWatch Logs**.  

---

### **Future Improvements**
- Add **CloudWatch alarms** for failed uploads.  
- Use **SES (Simple Email Service)** for richer email templates.  
- Extend to process or analyze files (e.g., image size, metadata).  
