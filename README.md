# 🚀 AI-Driven Serverless Image Processing Pipeline

**Engineer:** Shahd Galal Anwar
**Region:** `eu-central-1` (Frankfurt)  

---

## 🏗️ 1. System Architecture
This solution implements a **fully decoupled, event-driven pattern** to automate image ingestion and optimization. By leveraging AWS serverless primitives, the pipeline eliminates infrastructure management while ensuring high availability and cost-efficiency.
![System-Architecture](System_Architecture.png)

### **The Workflow Lifecycle**
1. **Secure Handshake:** The Client (Browser) requests a secure upload handshake via **Amazon API Gateway**.
2. **Delegated Authorization:** A specialized **Lambda function** (`GetPresignedUrl`) issues an **S3 Presigned URL**, enabling secure, credential-free uploads directly to the cloud.
3. **Direct Ingestion:** The Client transmits the raw image to the **Source S3 Bucket**, bypassing server-side bottlenecks.
4. **Event Trigger:** S3 Event Notifications automatically invoke the **ImageProcessor Lambda** upon successful object creation.
5. **Optimization & Persistence:**
    * The processor dynamically initializes the **Pillow** library within the ephemeral `/tmp` directory.
    * Images are resized and optimized on-the-fly.
    * Final metadata (S3 paths and timestamps) is indexed in **Amazon DynamoDB**.
    * The processed asset is committed to the **Destination S3 Bucket**.

---

## 🛠️ 2. Technical Specification

| Layer | Technology | Role |
| :--- | :--- | :--- |
| **Frontend** | HTML5, CSS3, Vanilla JS | Responsive UI & Direct S3 Integration |
| **API Management** | Amazon API Gateway | RESTful Endpoint Orchestration |
| **Compute** | AWS Lambda (Python 3.12) | On-demand Image Manipulation |
| **Storage** | Amazon S3 | Scalable Object Storage (Source & Dest) |
| **Database** | Amazon DynamoDB | Low-latency NoSQL Metadata Store |

---

## ⚙️ 3. Engineering Highlights
To maintain high performance and security in the **Frankfurt** region, the following technical guardrails were implemented:

* **Dynamic Dependency Injection:** To keep the deployment package lightweight, the **Pillow** library is installed at runtime within the `/tmp` directory. This "Bootstrap" method ensures the function remains lean and reduces cold-start latency.
* **Security Hardening:** Boto3 clients are strictly enforced to use **S3 Signature Version 4**, ensuring compatibility with Frankfurt’s enhanced security requirements.
* **Performance Tuning:** A **60-second timeout** is configured to account for the dynamic library bootstrap and processing of high-resolution assets.
* **Least Privilege Access:** The `ImageProcessorRole-Frankfurt` is scoped specifically to S3, DynamoDB, and CloudWatch Logs to minimize the security blast radius.

---

## 💡 4. Challenges Overcome
* **CORS & Signature Mismatch:** Resolved `403 Forbidden` errors by synchronizing the S3 CORS policy with the frontend Fetch API and standardizing signature versions.
* **Library Management:** Overcame Lambda deployment limits by implementing a custom runtime installation script for image processing dependencies.
* **Regional Latency:** Synchronized all resources within `eu-central-1` to minimize cross-region data transfer costs and maximize throughput.

---

## 📽️ 5. Visual Documentation
* **Architecture Diagram:** `[architecture-diagram.png]`
* **Live Demonstration:** `[Link to Video/GIF]`
