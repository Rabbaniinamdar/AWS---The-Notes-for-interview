# AWS(Amazon Web Service) Notes

# **🔐 AWS IAM Identity Center (SSO) – Comprehensive Notes**

## ✅ **Overview**

**AWS IAM Identity Center** (formerly AWS SSO) provides centralized identity and access management across multiple AWS accounts and applications. It enables users to access their assigned AWS accounts with single sign-on (SSO) while allowing administrators to define roles and permissions at scale.

---

## 🧠 **Core Concepts**

| Term          | Description                                                                 |
| ------------- | --------------------------------------------------------------------------- |
| **Root User** | Full access to the AWS account. Use only for critical administrative tasks. |
| **IAM User**  | A specific identity for individuals or systems with login/API access.       |
| **IAM Group** | Logical collection of IAM users. Policies attached here apply to all users. |
| **IAM Role**  | An identity assumed by trusted entities to gain temporary permissions.      |
| **Policy**    | JSON document defining permissions (allow/deny) on AWS resources.           |

---

## 🛠️ **Step-by-Step Setup**

### 🔹 1. **Create an AWS Account**

* Sign up at [aws.amazon.com](https://aws.amazon.com).
* Provide email, account name, and billing details.
* Use the **Free Tier** responsibly to avoid unexpected charges.

---

### 🔹 2. **Secure the Root Account**

* Use only for:

  * MFA setup
  * Billing
  * Initial Identity Center configuration
* Enable **MFA** and store credentials securely.

---

### 🔹 3. **Access IAM Identity Center**

* From AWS Console, search for **IAM Identity Center**.
* Use this to:

  * Create/manage users
  * Define **Permission Sets**
  * Assign access to AWS accounts
  * Enable SSO login

---

### 🔹 4. **Create IAM Users (if needed outside SSO)**

* Go to **IAM > Users > Add user**

  * Choose access type:

    * Console access
    * Programmatic access (CLI/API)
  * Set initial password (reset recommended)
* Avoid this in favor of Identity Center unless necessary.

---

### 🔹 5. **Assign Permissions via Groups**

* Go to **IAM > Groups > Create Group**

  * Name logically (e.g., `DevOps`, `Analysts`)
  * Attach policies (AWS Managed or custom)
* Add users to groups to apply bulk permissions.

---

### 🔹 6. **Create IAM Roles**

* Roles are ideal for:

  * **Cross-account access**
  * **Federated identities**
  * **AWS service access**
* Go to **IAM > Roles > Create role**

  * Select trusted entity (another AWS account, Identity Center, or service)
  * Attach policies
  * Distribute **Role ARN** if needed

---

### 🔹 7. **Create and Attach Policies**

* Types of policies:

  * **AWS Managed**: Predefined by AWS (e.g., `AmazonS3ReadOnlyAccess`)
  * **Customer Managed**: Custom policies written in JSON
* Use **Visual Editor** or JSON to define:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "*"
    }
  ]
}
```

---

### 🔹 8. **Set Up Permission Sets in IAM Identity Center**

* Go to **IAM Identity Center > Permission Sets**
* Create sets defining what users can do (e.g., `ReadOnlyAccess`, `AdminAccess`)
* Assign sets to users/groups per AWS account
* Behind the scenes, Identity Center creates IAM roles and applies the permissions automatically.

---

## 🛡️ **Security Best Practices**

| Practice                   | Recommendation                               |
| -------------------------- | -------------------------------------------- |
| MFA                        | Enable for all users and root account        |
| Least Privilege Principle  | Grant only necessary permissions             |
| IAM Access Key Rotation    | Rotate regularly; never hardcode credentials |
| Monitor Logs               | Enable AWS CloudTrail and review regularly   |
| Disable Unused Users/Roles | Clean up periodically                        |

---

## 🗂️ **Summary Table**

| Step            | Action                                                      |
| --------------- | ----------------------------------------------------------- |
| Account Setup   | Create AWS account and secure root access                   |
| IAM Users       | Create users if not using SSO                               |
| Groups          | Organize users logically and assign common permissions      |
| Roles           | Delegate access across accounts/services/federated entities |
| Policies        | Define and assign fine-grained permissions                  |
| Identity Center | Manage centralized user access with SSO and permission sets |

---

## 🚀 **Benefits of IAM Identity Center**

* Centralized access control across AWS Organizations
* Improved **security posture** with centralized identity management
* No need to manage individual IAM users in each AWS account
* Simplifies SSO login for developers, admins, and business users

## 🧠 Serverless Architecture vs Monolithic (Traditional) Architecture

## ✅ 1. What is Serverless?

 **Serverless doesn't mean no servers — it means you don’t manage them, Serverless is a cloud-native architecture where you write just your code, and the provider handles provisioning, scaling, and billing — making it perfect for event-driven, scalable, and cost-effective workloads like APIs, automation, and data processing.**

* In **serverless architecture**, developers write and deploy **code (functions)** without worrying about:

  * Server provisioning
  * Operating system management
  * Auto-scaling
  * Fault tolerance

* AWS handles infrastructure — you focus purely on **business logic**.

### 🔧 Common Serverless Services:

* **AWS Lambda** – Runs your function code on-demand
* **API Gateway** – Creates RESTful APIs to trigger Lambdas
* **DynamoDB** – Serverless NoSQL database
* **S3** – Serverless object storage
* **Step Functions** – For orchestrating multiple Lambda calls

---

## 🚫 2. Traditional / Monolithic Architecture (e.g., EC2-based)

* You manually provision a **VM or EC2** instance.
* Responsible for:

  * Setting up OS (e.g., Ubuntu)
  * Installing runtime (e.g., Node.js, Java)
  * Monitoring traffic and scaling
  * Handling uptime and patching
* **Billing:** Pay per **running hour**, regardless of usage.

---

## ⚖️ 3. Side-by-Side Comparison

| Feature            | Monolithic (EC2)              | Serverless (Lambda)                       |
| ------------------ | ----------------------------- | ----------------------------------------- |
| **Setup**          | Manual: VM, OS, app runtime   | Automatic: Just upload function code      |
| **Scaling**        | Manual or auto-scaling config | Auto-scales per request                   |
| **Billing**        | Per hour                      | Per request (GB-second duration-based)    |
| **Deployment**     | Full app/server               | Individual functions                      |
| **Responsibility** | DevOps-heavy                  | Infra abstracted (DevOps-light)           |
| **Use case fit**   | Long-running, stateful apps   | Stateless, event-driven, bursty workloads |

---

## 💰 4. AWS Lambda Cost Model

* **1 million free requests/month**
* After that: **\$0.20 per million requests**
* You are billed for:

  * **Request count**
  * **Execution duration** (GB-seconds)
* Example:

  * A 128 MB Lambda running for 1 second = 128 MB-sec = 0.125 GB-sec
  * Extremely cost-efficient for low-traffic or unpredictable workloads.

---

## 🚀 5. Scalability & Resilience

* Serverless:

  * Instantly scales to thousands of parallel executions.
  * No idle servers = no cost when idle.
  * Great for **microservices**, **event-driven**, or **real-time processing** (e.g., image uploads, log processing, API backends).
* Monolithic:

  * Requires load balancers, auto-scaling groups.
  * Idle resources still cost money.

---

## 🧪 6. Real-World Example: Prime Video

* **Amazon Prime Video** migrated **some workloads** *back* to monolith (from serverless).
* Reason: **High-volume, predictable, compute-intensive** workloads (like video processing) were **more cost-efficient** in monolith form.
* Takeaway: **Architecture is not one-size-fits-all** — evaluate based on workload.

---

## ⚠️ 7. Limitations of Serverless

| Limitation                | Detail                                      |
| ------------------------- | ------------------------------------------- |
| **Timeout**               | Max duration: 15 minutes per Lambda         |
| **Cold Starts**           | First call after idle period is slower      |
| **State Management**      | Functions are stateless; use external state |
| **Debugging**             | Harder than traditional apps                |
| **Execution Environment** | Limited packages/languages/runtime versions |
| **Vendor Lock-in**        | Deep integration with AWS                   |

---

## ✅ 8. When to Use Serverless

Use it when:

* You have **event-driven** or **API-based** applications
* You want **fast time-to-market**
* You're a **small team/startup** with limited DevOps support
* You expect **irregular or burst traffic**
* You need to **minimize costs**

---

## ❌ When Not to Use Serverless

Avoid it when:

* You require **persistent connections** (e.g., WebSockets)
* You have **high CPU or memory-bound** long tasks (use EC2/Fargate)
* You need **fine-grained OS control**


### 🧩 Serverless Architecture Flow

```
Client (Frontend)
   ⬇️ HTTP Request
API Gateway (Handles endpoint routing)
   ⬇️
AWS Lambda (Function logic: CRUD, processing)
   ⬇️
DynamoDB / RDS / S3 (Database or Storage)
```

* No need to manage backend servers.
* Functions scale automatically based on traffic.
* Pay only when the function is invoked.

---

## 🛠️**Practical Implementation — Build a Serverless REST API**

We'll create a simple **“User Registration API”** using:

* AWS Lambda
* API Gateway
* Node.js runtime
* DynamoDB (optional DB for persistence)

---

### ✅ Step-by-Step: Deploy Serverless API with AWS Lambda + API Gateway

---

### 📌 1. **Create Lambda Function**

1. Go to **AWS Console** > **Lambda** > **Create Function**
2. Choose:

   * Author from scratch
   * Function name: `createUser`
   * Runtime: `Node.js 18.x`
3. Click **Create Function**

---

### 📦 2. **Write Node.js Code in Lambda**

In the function editor:

```js
exports.handler = async (event) => {
    const body = JSON.parse(event.body);
    
    const name = body.name;
    const email = body.email;

    // Logic (in real-world: store to DB like DynamoDB)
    const response = {
        statusCode: 200,
        body: JSON.stringify({
            message: "User created successfully",
            user: { name, email }
        }),
    };
    return response;
};
```

✅ This function receives POST data and returns a response with user info.

---

### 🌐 3. **Create API Gateway (HTTP API)**

1. Go to **API Gateway** > **Create API**
2. Choose: **HTTP API**
3. Integration: Choose your Lambda (`createUser`)
4. Add route:

   * Method: `POST`
   * Path: `/register`
5. Deploy the API

🔗 You’ll get a public URL like:

```
https://abc123.execute-api.ap-south-1.amazonaws.com/register
```

---

### 🧪 4. **Test API Using Postman or Curl**

#### Postman Request:

* Method: `POST`
* URL: (your API endpoint)
* Body: `raw JSON`

```json
{
  "name": "Rabbani",
  "email": "rabbani@example.com"
}
```

✅ Expected response:

```json
{
  "message": "User created successfully",
  "user": {
    "name": "Rabbani",
    "email": "rabbani@example.com"
  }
}
```

---

### 💾 5. (Optional) Add DynamoDB for Persistent Storage

#### DynamoDB Table Setup:

* Table name: `Users`
* Primary key: `email`

#### Modify Lambda Code:

```js
const AWS = require('aws-sdk');
const dynamo = new AWS.DynamoDB.DocumentClient();

exports.handler = async (event) => {
    const body = JSON.parse(event.body);
    const { name, email } = body;

    const params = {
        TableName: "Users",
        Item: { email, name },
    };

    await dynamo.put(params).promise();

    return {
        statusCode: 200,
        body: JSON.stringify({ message: "User stored!", user: { name, email } }),
    };
};
```

🔐 Don’t forget to assign **DynamoDB full access policy** to the Lambda IAM role.

---

## 🔄 Summary of Architecture:

```
Frontend (React/Angular App)
      |
      |  [POST] /register
      V
API Gateway (HTTP API)
      |
      V
AWS Lambda (Business Logic)
      |
      V
DynamoDB (Optional Data Store)
```

---

## 🧠 Interview Tips:

| Concept       | What to Focus On                                      |
| ------------- | ----------------------------------------------------- |
| Lambda        | Cold starts, scaling, timeout limits                  |
| API Gateway   | Routes, integration types (HTTP/REST), auth           |
| DynamoDB      | NoSQL structure, primary keys, pricing                |
| Billing       | Lambda: per-invocation; EC2: per uptime               |
| Real Use Case | Prime Video’s move from Lambda to ECS for high volume |

---

## 📘 Bonus Tips

* **Add CORS headers** if you're using a frontend:

```js
headers: {
  "Access-Control-Allow-Origin": "*",
  "Content-Type": "application/json"
}
```

### **✅Interview questions** 

### **1. What is serverless architecture?**

**Answer:**
Serverless architecture allows developers to build and run applications without managing infrastructure. The cloud provider (e.g., AWS) automatically handles server provisioning, scaling, and maintenance. Developers focus only on writing and deploying functions (like AWS Lambda).

**Example:**
A function triggered when an image is uploaded to S3 that resizes the image.

**Keywords:** infrastructure abstraction, event-driven, function-as-a-service (FaaS), auto-scaling

---

### **2. How is AWS Lambda different from EC2?**

| Feature         | AWS Lambda                      | EC2 (Elastic Compute Cloud)         |
| --------------- | ------------------------------- | ----------------------------------- |
| **Server mgmt** | Fully managed by AWS            | User-managed                        |
| **Billing**     | Per request (GB-second)         | Per uptime (hour/second)            |
| **Scaling**     | Automatic                       | Manual or via auto-scaling          |
| **Use Case**    | Short-lived, event-driven tasks | Long-running, stateful applications |

**Keywords:** per-invocation billing, ephemeral compute, managed vs unmanaged

---

### **3. What are cold starts? How do you mitigate them?**

**Answer:**
A cold start happens when a Lambda function is invoked after being idle — AWS needs to initialize the runtime and dependencies, causing a delay (\~100ms to a few seconds).

**Mitigation:**

* Use **provisioned concurrency** to keep functions warm
* Optimize dependencies and package size
* Use **lighter runtimes** (e.g., Node.js, Go)

**Keywords:** warm start, idle timeout, provisioned concurrency

### **4. Explain a scenario where serverless is not the right choice.**

**Answer:**
If you need **persistent connections** (e.g., WebSockets for real-time chat) or **long-running background processes** (e.g., video encoding > 15 min), serverless is not suitable due to timeout and statelessness limitations.

**Better Alternatives:**

* Use EC2, ECS, or AWS Fargate for such workloads.

**Keywords:** persistent compute, execution timeout, stateful processing

---

### **5. How is billing handled in AWS Lambda?**

**Answer:**
Billing is based on:

* **Number of requests** (1M free/month, then \$0.20/million)
* **Duration** of each execution (rounded to nearest ms)
* **Allocated memory** (128 MB to 10 GB)

**Formula:**
`Cost = Request Count x Duration x Memory`

**Example:**
512MB function running for 2 seconds = 1.024 GB-seconds

**Keywords:** GB-seconds, pay-per-use, free tier

---

### **6. Compare API Gateway + Lambda vs Express.js on EC2**

| Feature                 | API Gateway + Lambda | Express.js on EC2                  |
| ----------------------- | -------------------- | ---------------------------------- |
| **Scalability**         | Auto-scaled          | Manual or with auto-scaling groups |
| **Cost at Low Traffic** | Near-zero            | Fixed (server always running)      |
| **Maintenance**         | Minimal              | Requires OS/runtime patching       |
| **Cold Starts**         | Possible             | No cold starts                     |
| **Latency**             | Slightly higher      | Lower and consistent               |

**Keywords:** serverless REST API, Express backend, cost-efficiency, DevOps effort

### **7. How does AWS handle concurrency in Lambda?**

**Answer:**
Lambda **spins up a new instance** of the function per request, up to a region-specific **concurrency limit** (default 1,000). You can increase this with a request.

**Types:**

* **Unreserved concurrency**: Shared pool
* **Reserved concurrency**: Guarantees a set number of instances
* **Provisioned concurrency**: Pre-warmed instances, avoids cold starts

**Keywords:** parallel invocations, concurrency throttling, reserved concurrency

---

### **8. How would you design a serverless web application backend?**

**Answer:**

📦 **Architecture:**

* **Frontend**: React or Angular (hosted on S3 + CloudFront)
* **API Layer**: API Gateway routes HTTP requests
* **Compute**: AWS Lambda functions handle logic
* **Database**: DynamoDB for NoSQL or Aurora Serverless for relational
* **Authentication**: Cognito for user auth
* **Storage**: S3 for file uploads

**Bonus:**

* Use **CloudWatch** for logging/monitoring
* Use **Step Functions** for workflows

**Keywords:** decoupled architecture, microservices, scalability, minimal ops

---

### **9. What are the trade-offs between serverless and containers?**

| Aspect           | Serverless                    | Containers (ECS/Fargate)           |
| ---------------- | ----------------------------- | ---------------------------------- |
| **Startup Time** | Cold start delay              | Faster with always-on containers   |
| **State**        | Stateless                     | Can be stateful                    |
| **Use Case**     | Short, event-driven functions | Long-running apps, custom runtimes |
| **Control**      | Limited runtime/infra control | Full control (OS, dependencies)    |
| **Scaling**      | Fully automatic               | Configurable scaling               |
| **Cost**         | Pay-per-use                   | May incur idle cost                |


### 🚀 **Goal**

Run a Spring Boot app on AWS Lambda with HTTP access using API Gateway (no server to manage).

---

### ✅ **1. Project Setup**

* Use a special **Maven archetype** to generate a Spring Boot project ready for Lambda:

```bash
mvn archetype:generate \
  -DarchetypeGroupId=software.amazon.awssdk \
  -DarchetypeArtifactId=aws-serverless-springboot3-archetype \
  -DarchetypeVersion=1.9 \
  -DgroupId=com.example \
  -DartifactId=springboot-lambda \
  -Dversion=1.0-SNAPSHOT \
  -DinteractiveMode=false
```

It auto-generates:

* `StreamLambdaHandler.java` – Lambda entry point
* Preconfigured `pom.xml` with dependencies
* `template.yml` – optional deployment file

---

### 🧩 **2. Write Your App Code**

* **Controller** → Define REST endpoints (GET, POST, PUT, DELETE)
* **Service** → Add business logic (in-memory list for demo)
* **DTO** → Define model class (e.g., `Course.java`)

> Use standard Spring Boot annotations: `@RestController`, `@Service`, `@RequestMapping`, etc.

---

### 📦 **3. Package the App**

Run:

```bash
mvn clean package
```

This produces a `.zip` file (e.g., `springboot-lambda-1.0-SNAPSHOT-aws-lambda.zip`) inside `target/`.

> Lambda needs a `.zip`, not a `.jar`.

---

### ☁️ **4. Deploy to AWS Lambda**

* Go to AWS Lambda → **Create Function**

  * Runtime: Java 21
  * Name: `springboot-lambda-api`
* Upload your `.zip` file
* Set the **Handler** to:

  ```
  com.example.StreamLambdaHandler::handleRequest
  ```

---

### 🌐 **5. Create API Gateway**

* Go to API Gateway → **Create API**

  * Choose **HTTP API** or **REST API**
* Create a proxy resource (`/{proxy+}`) and method (`ANY`)
* Link it to your Lambda function
* Deploy it to a **stage** (e.g., "dev")
* Copy the **Invoke URL**

---

### 🔄 **6. Test with Postman / curl**

Use the Invoke URL:

```http
POST    https://your-api/dev/courses
GET     https://your-api/dev/courses
PUT     https://your-api/dev/courses/1
DELETE  https://your-api/dev/courses/1
```
---

## 📌 **What is AWS S3 (Simple Storage Service)?**

* **S3** stands for **Simple Storage Service** by AWS.
* It is **object-based** cloud storage used for storing any type of file (e.g., images, videos, logs, backups).
* Ideal for storing static files, media, documents, backups, etc.

---

## 🧱 **Core Concepts**

### 🪣 Buckets

* Logical containers for storing data (files = objects).
* Each bucket name must be **globally unique** (like domain names).
* You can have **up to 100 buckets** per AWS account.
* Each bucket has **no size limit**.

### 📦 Objects

* Files stored inside buckets.
* Includes metadata (e.g., size, file type).
* Objects can be of any file type.

### 📁 Folders

* Folders are a **UI concept** to group objects.
* Internally, it's a **prefix** in the object's name (like `images/photo.jpg`).

---

## 🌍 **Bucket Naming Rules**

* Must be **globally unique**.
* Lowercase only, no special characters.
* Suggested naming format: `yourdomain-application-name-region`

---

## 🌐 **Bucket Regions**

* You must choose an **AWS region** when creating a bucket (e.g., `ap-south-1` for Mumbai).
* Your data is stored **physically** in that region.

---

## 🔐 **Security & Access Control**

### IAM (Identity and Access Management)

* **Root user** should not be used daily.
* Create **IAM users/groups**, assign permissions via **policies**.
* Best practice: Use **least privilege** principle.

### Bucket Access

* By default, **buckets and objects are private**.
* You can allow public access using:

  * **Bucket policy**
  * **Object-level permissions**
  * **ACLs (Access Control Lists)**

---

## ⚙️ **Live Demo Overview**

### 1. Creating IAM User for S3

* Go to IAM → Create user.
* Attach the **AmazonS3FullAccess** policy.
* Login with that user for demo.

### 2. Creating a Bucket

* Go to S3 → Create bucket.
* Choose region.
* Set bucket name (must be unique).
* Leave default options unless specific configuration is needed.

### 3. Uploading Files

* Drag-and-drop or use the upload UI.
* You can upload individual files or folders.
* Once uploaded, files can be:

  * Viewed
  * Downloaded
  * Renamed
  * Deleted
  * Shared (if public)

### 4. Making Object Public

* Access denied if tried publicly by default.
* Must **edit bucket permissions**:

  * Unblock public access at **bucket level**.
  * Add a **bucket policy** to allow public GET access.
  * Or set object-level permission via **ACL**.

---

## ⚠️ Best Practices

* Always create **IAM users**, don’t use root account.
* Enable **MFA (Multi-Factor Authentication)** for secure access.
* Do **not make buckets public** unless absolutely required.
* Use **lifecycle policies** to automate file deletion or transitions to cheaper storage.
* Enable **versioning** for critical data to recover deleted/overwritten files.

---

## ✅ Summary: Key Takeaways

| Feature      | Details                                                       |
| ------------ | ------------------------------------------------------------- |
| Storage Type | Object-based                                                  |
| Main Unit    | Buckets → Objects                                             |
| Capacity     | Unlimited per bucket                                          |
| Permissions  | Private by default, use IAM/bucket policies to manage access  |
| Use Cases    | Static assets, backups, logs, media, documents                |
| Security     | Use IAM, never use root, avoid public access unless necessary |


## 🌐 **Hosting a Static Website with S3**

### 🚀 **Steps to Host a Static Website on S3**

1. **Prepare Your Site Files**

   * Ensure you have an `index.html` (and optionally `style.css`, `script.js`, images, etc.)
   * Organize all files into a single folder.

2. **Create an S3 Bucket**

   * Go to **S3 Console**
   * Click **Create bucket**
   * Enter a **globally unique name** (e.g., `rabbani-portfolio`)
   * Choose your **region** (e.g., `ap-south-1` for Mumbai)
   * **Uncheck "Block all public access"**
   * Acknowledge the warning and click **Create bucket**

3. **Upload Website Files**

   * Open your bucket
   * Click **Upload** → Drag & drop all website files (HTML, CSS, images)

4. **Enable Static Website Hosting**

   * Go to the **Properties** tab of your bucket
   * Scroll to **Static website hosting**
   * Click **Edit** → Enable hosting
   * Specify:

     * **Index document**: `index.html`
     * (Optional) **Error document**: `error.html`
   * Save changes

5. **Make Website Public**

   * Go to the **Permissions** tab
   * Scroll to **Bucket Policy**
   * Paste this JSON (replace `your-bucket-name`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

6. **Access Your Site**

   * After enabling static hosting, you'll see a **website endpoint URL**, e.g.:

     ```
     http://your-bucket-name.s3-website.ap-south-1.amazonaws.com
     ```
   * Open it in your browser to view your site live.

---

### 📌 Notes & Best Practices

| Practice                      | Why It's Important                                   |
| ----------------------------- | ---------------------------------------------------- |
| Avoid public buckets          | Unless you're hosting static websites                |
| Use clean bucket names        | Reflect project name or domain                       |
| Enable versioning             | So you can roll back to previous file versions       |
| Automate with lifecycle rules | Move old assets to cheaper storage (e.g., Glacier)   |
| Use CloudFront + HTTPS        | For better performance, security, and custom domains |
| Keep backups                  | Download periodically or sync with CLI               |

---

### ✅ **What is a Pre-signed URL?**

A **pre-signed URL** is a **secure, time-limited URL** generated by your application that gives **temporary access** to an object in an S3 bucket. It can be used for:

* **Uploading** a file (HTTP PUT)
* **Downloading** a file (HTTP GET)
* **Other operations like DELETE, etc.**

---

### 🔒 Pre-signed URL vs Traditional/Public URL

| Feature                   | Pre-signed URL                                            | Traditional/Public URL                      |
| ------------------------- | --------------------------------------------------------- | ------------------------------------------- |
| **Access Control**        | Secure, temporary access                                  | Public – no access control once shared      |
| **Expiration**            | Yes (e.g., valid for 15 min)                              | No (always accessible)                      |
| **Authentication Needed** | No, since it's pre-authenticated                          | Yes, unless the object is public            |
| **Usage Scenarios**       | Secure file sharing, uploads without exposing credentials | Public content like images on a public site |
| **Upload Support**        | ✅ Yes, via PUT request                                    | ❌ No, not possible directly                 |
| **Custom Logic**          | Can control permission, file type, etc. per request       | Not customizable per user                   |
| **S3 Bucket Privacy**     | Bucket can be private                                     | Must be public or have ACLs for access      |

---

### 🛡️ Why use Pre-signed URLs?

1. **Secure Upload/Download**

   * You don’t expose your S3 credentials or make the bucket public.
   * Only the person with the pre-signed URL can access the file.

2. **Temporary Access**

   * You can set expiration (e.g., 15 minutes, 1 hour).
   * After that, the link becomes invalid.

3. **Fine-Grained Control**

   * You can control who accesses what, and when.
   * Useful for user-based file sharing (e.g., file download after login).

4. **Avoiding Server Load**

   * Files go directly to/from S3 via browser or mobile app, not via your backend server.

5. **Compliance & Privacy**

   * Supports GDPR, HIPAA, and secure practices without public exposure.

---

### 📁 Real-World Example:

#### ✅ Good Use (Pre-signed URL):

* User uploads a profile picture securely from a mobile app.
* Your backend generates a pre-signed PUT URL valid for 5 minutes.
* The app uploads directly to S3 using that URL — secure and efficient.

#### ❌ Bad Use (Public URL):

* You upload files directly to a public S3 bucket.
* Anyone with the URL can access/download them forever.
* Risk of data leakage, and you can’t revoke access.

---

### 🔧 Summary

| When to use Pre-signed URL?                                                   |
| ----------------------------------------------------------------------------- |
| ✔ For secure, temporary file access                                           |
| ✔ For direct uploads from frontend/mobile                                     |
| ✔ When your S3 bucket is private                                              |
| ✔ When user-based access is needed                                            |
| ✔ When you want to avoid exposing credentials or storing files on your server |



### 🔁 **Full Flow: Upload → Event → Lambda → Audit → Lifecycle**

```
+---------------------+
|   S3 Bucket         |
|   (with Versioning) |
+---------+-----------+
          |
          | (1) User uploads object (or new version)
          ↓
+-------------------------+
|     Event Trigger       |
|   (S3 Notification)     |
|   Type: PUT/DELETE      |
+-----------+-------------+
            |
            ↓
+-------------------------+
|      Lambda Function    |
|  (onFileUploadHandler)  |
|  ✓ Send Email           |
|  ✓ Log to DB            |
|  ✓ Process File         |
+-------------------------+
            |
            ↓
+--------------------------+
|    CloudTrail Logging    |
|  (who uploaded/when etc)|
+--------------------------+

🧾 After Upload:
        ↓
+--------------------------+
| Lifecycle Management     |
| - After 10 days → IA     |
| - After 30 days → Glacier|
| - After 60 days → Delete |
+--------------------------+

(Optional)
        ↓
+---------------------------+
| Cross-Region Replication |
| Backup to another region |
+---------------------------+
```


## ✅ **AWS S3 (Simple Storage Service) – Part 4: In-depth Concepts & Real-world Practices**

### 📌 **Overview**

This session deep dives into advanced S3 features and best practices that cloud engineers and developers must know for production use. Topics covered include **bucket settings**, **object versioning**, **security**, **automation**, **cost optimization**, and **monitoring**.

---

## 🗂️ **1. Bucket and Object Fundamentals**

* **Bucket Properties**:

  * Contains metadata such as:

    * AWS Region (e.g., `ap-south-1`)
    * Bucket ARN (Amazon Resource Name)
    * Creation timestamp

* **Objects in S3**:

  * Each file, image, or folder uploaded is treated as an **object**.
  * An object = data + metadata (key-value pairs) + unique key

* **Common Object Actions**:

  * Upload, download, delete, move, rename, set permissions
  * Versioned actions (if versioning is enabled)

---

## 🔁 **2. Versioning**

* **Purpose**:

  * Maintain **history of changes**. Every time an object is modified or re-uploaded, a new version is created.
  * Useful for **accidental overwrite recovery** and **audit trails**

* **How to Enable**:

  * In the bucket settings → Enable “Versioning”

* **Storage & Cost Implication**:

  * Each version occupies separate storage = **more cost**
  * Deleted files become "delete markers" — still consume storage unless removed

* **Best Practice**:

  * Use lifecycle rules to clean up older versions after a retention period

---

## 🏷️ **3. Tags (Key-Value Metadata)**

* **Purpose**:

  * Used for categorization, billing separation, cost tracking, and automation

* **Example Tags**:

  * `Project: InvoiceApp`, `Owner: Rabbani`, `Environment: Production`

* **IAM Tag-based Policies**:

  * You can define access policies that grant/restrict based on tags

---

## 🕵️‍♂️ **4. Event Logging & Notifications**

### 🔍 **CloudTrail Integration**

* Records **API-level activity** on S3 (upload, delete, ACL/policy changes)

* Helps in:

  * Security auditing
  * Compliance
  * Troubleshooting: Who deleted my file and when?

* **How to Use**:

  * Enable CloudTrail for the bucket or the AWS account

---

### 🔔 **Event Notifications**

* **Purpose**: Automatically trigger actions when an event occurs

* **Supported Events**:

  * `s3:ObjectCreated:*` → file uploaded
  * `s3:ObjectRemoved:*` → file deleted

* **Targets**:

  * **AWS Lambda** → run custom logic (e.g., email/SMS)
  * **Amazon SNS** → push notifications
  * **Amazon SQS** → queue processing

* **Practical Example**:

  * Uploading a file triggers a Lambda that sends an email to the admin

---

## 🌐 **5. Static Website Hosting**

* **Enable from Bucket Properties**
* **Required Configuration**:

  * Index document (e.g., `index.html`)
  * Error document (e.g., `error.html`)
  * Public access must be enabled via bucket policy
* **Use Case**:

  * Hosting documentation, portfolio sites, landing pages

---

## 🔐 **6. Permissions & Security**

* **3 Layers of Access Control**:

  1. **Bucket Policies** – JSON-based global permissions (preferred method)
  2. **IAM Policies** – User/group/role-based access
  3. **ACLs** (Access Control Lists) – Fine-grained control (deprecated, avoid use)

* **Blocked Public Access**:

  * AWS provides settings to block all public access at bucket/account level (enabled by default)

* **Best Practice**:

  * Never keep confidential or sensitive data in publicly accessible buckets

---

## 🔄 **7. CORS (Cross-Origin Resource Sharing)**

* **Use Case**: Access S3 objects from front-end apps hosted on different domains (e.g., React/Angular apps)

* **CORS Configuration Example**:

  ```xml
  <CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>http://localhost:8080</AllowedOrigin>
      <AllowedMethod>GET</AllowedMethod>
      <AllowedMethod>PUT</AllowedMethod>
      <AllowedHeader>*</AllowedHeader>
    </CORSRule>
  </CORSConfiguration>
  ```

* **Important**:

  * Without CORS, browser will block frontend requests to S3

---

## 📊 **8. Monitoring and Analytics**

* **Metrics Available**:

  * Total storage size
  * Number of objects
  * Requests per second
  * Error rates

* **S3 Storage Class Analysis**:

  * Helps decide when to transition data to a lower-cost class (e.g., Infrequent Access)

* **Inventory Reports**:

  * Periodic reports in CSV or JSON
  * Lists objects, sizes, last modified dates, and metadata

---

## 🔁 **9. Lifecycle Management**

* **Lifecycle Rules** automate object transition & deletion:

  * Example:

    * Day 0: Upload to Standard
    * Day 10: Move to Standard-IA
    * Day 30: Move to Glacier
    * Day 90: Permanently delete

* **Benefits**:

  * Cost optimization
  * Automatic cleanup of unnecessary data

---

## 🌍 **10. Replication (CRR/SRR)**

* **Cross-Region Replication (CRR)**:

  * Automatically copies new objects to another bucket in a **different region**
  * Use case: Disaster Recovery, Global Read Access

* **Same-Region Replication (SRR)**:

  * For data segregation within the same region (e.g., logs, compliance)

* **Pre-requisites**:

  * Versioning must be enabled on both source and destination buckets

---

## 💸 **11. Cost Management & Optimization**

* **S3 Pricing Breakdown**:

  * **Storage**: Based on size + storage class
  * **Requests**: Each PUT, GET, LIST costs a small amount
  * **Data Transfer**:

    * Inbound: Free
    * Outbound (download): Charged per GB

* **Storage Classes**:

  * Standard (frequent access)
  * Standard-IA (infrequent)
  * Intelligent-Tiering (automatically optimizes cost)
  * Glacier / Glacier Deep Archive (cold storage)

* **Best Practice**:

  * Archive cold data
  * Enable object expiration rules
  * Use Storage Class Analysis for recommendations

---

## 📌 **Summary Table**

| **Feature**                | **Purpose**                                                         |
| -------------------------- | ------------------------------------------------------------------- |
| **Versioning**             | Preserve all versions of objects; useful for backup and recovery    |
| **Tags**                   | Metadata for cost tracking, automation, and policy control          |
| **CloudTrail**             | Audit trail of all S3-related activities                            |
| **Event Notifications**    | Automate workflows (e.g., Lambda trigger on upload)                 |
| **Static Website Hosting** | Serve static websites directly from S3 with public access           |
| **Permissions**            | Secure S3 with bucket policies and IAM; avoid public ACLs           |
| **CORS**                   | Allow browser-based apps to interact with S3 across domains         |
| **Monitoring**             | Analyze usage and storage; generate object inventory reports        |
| **Lifecycle Management**   | Automate transition to cheaper storage and object deletion          |
| **Replication**            | Backup data across or within regions                                |
| **Cost Optimization**      | Use right storage class; monitor usage; optimize lifecycle policies |


## 🖥️ What is EC2?

**Amazon EC2 (Elastic Compute Cloud)** is a web service that provides **resizable compute capacity in the cloud**. It allows you to **launch virtual servers** (called *instances*) on-demand and scale them based on your needs.

---

## 📌 Key Concepts

| Term                            | Description                                                                         |
| ------------------------------- | ----------------------------------------------------------------------------------- |
| **Instance**                    | A virtual server in the cloud.                                                      |
| **AMI (Amazon Machine Image)**  | A template with OS, software, and settings used to launch an instance.              |
| **Instance Type**               | Defines CPU, memory, storage, and network capacity (e.g., t2.micro, m5.large).      |
| **EBS (Elastic Block Store)**   | Persistent storage for your instances. Like a hard disk.                            |
| **Security Groups**             | Acts as a virtual firewall to control traffic to/from EC2.                          |
| **Key Pair**                    | Used to SSH (login) into instances securely.                                        |
| **Elastic IP**                  | A static public IP that can be attached to an instance.                             |
| **User Data**                   | Script you can run during instance boot to automate setup (e.g., install software). |
| **Elastic Load Balancer (ELB)** | Distributes traffic across multiple EC2 instances.                                  |
| **Auto Scaling**                | Automatically increases/decreases the number of instances based on load.            |

---

## 🧾 EC2 Instance Types

Amazon EC2 offers different instance types optimized for different use cases:

| Instance Family                            | Use Case                                                            |
| ------------------------------------------ | ------------------------------------------------------------------- |
| **General Purpose** (e.g., t4g, t3, m5)    | Balanced CPU, memory, and networking. Good for web apps, small DBs. |
| **Compute Optimized** (e.g., c7g, c6i)     | High performance CPUs. Good for compute-intensive tasks.            |
| **Memory Optimized** (e.g., r6g, x2idn)    | More memory. Used for large DBs, caching, in-memory processing.     |
| **Storage Optimized** (e.g., i4i, d3)      | High IOPS or throughput. Good for NoSQL, OLTP, etc.                 |
| **Accelerated Computing** (e.g., p4, inf2) | GPU-based. Ideal for ML, video processing, HPC.                     |

---

## 🧰 EC2 Use Cases

* Host websites and APIs
* Run backend services
* Deploy databases and caching systems
* Machine Learning model training and inference
* Video encoding/processing
* Game server hosting

---

## 📋 How to Launch an EC2 Instance (Manually)

1. **Log in to AWS Console**
2. Navigate to **EC2 Dashboard**
3. Click **Launch Instance**
4. Choose:

   * **AMI** (e.g., Amazon Linux, Ubuntu)
   * **Instance Type** (e.g., t2.micro)
   * **Key Pair** for SSH
   * **Security Group** (allow ports like 22 for SSH, 80 for HTTP)
   * **Storage** (default EBS volume)
5. Launch and connect via SSH

---

## 💰 Pricing Overview

### EC2 Pricing Models

| Model                  | Description                                                                       |
| ---------------------- | --------------------------------------------------------------------------------- |
| **On-Demand**          | Pay by the hour/second. Best for short-term, unpredictable workloads.             |
| **Reserved Instances** | Commit to 1- or 3-year usage for heavy discounts. Best for steady workloads.      |
| **Spot Instances**     | Use spare capacity for up to 90% off. Best for flexible or batch jobs.            |
| **Savings Plans**      | Flexible pricing model for compute usage (not just EC2). Commit for 1 or 3 years. |
| **Free Tier**          | 750 hours/month of t2.micro or t3.micro for 12 months (for new AWS accounts).     |

---

## 🔐 Security Best Practices

* Use **key pairs** and never share private keys.
* Keep **security groups** minimal (e.g., only open port 22 to your IP).
* Rotate **access credentials** regularly.
* Use **IAM roles** for instances instead of embedding credentials in apps.
* Enable **CloudWatch Logs** and **monitoring**.

---

## 📦 Storage Options for EC2

| Storage Type                  | Description                                                              |
| ----------------------------- | ------------------------------------------------------------------------ |
| **EBS**                       | Network-attached block storage. Persistent and resizable.                |
| **Instance Store**            | Temporary disk attached to physical host. Lost on reboot/stop.           |
| **EFS (Elastic File System)** | Shared file system across multiple instances.                            |
| **S3**                        | Not directly attached, but often used to store backups, logs, or assets. |

---

## 📈 Monitoring and Logs

* **Amazon CloudWatch** – Monitor CPU, memory, disk, and network.
* **AWS CloudTrail** – Logs API calls.
* **EC2 System Logs** – See boot errors and instance-level logs.

---

## 🚀 EC2 vs Other Compute Services

| Service       | When to Use                                                         |
| ------------- | ------------------------------------------------------------------- |
| **EC2**       | Full control over OS, storage, software. Long-running applications. |
| **Lambda**    | Serverless, event-driven short tasks. No server management.         |
| **ECS / EKS** | For Docker container orchestration.                                 |
| **Lightsail** | Simplified version of EC2 for small businesses or devs.             |

---

## 📍 Summary

* EC2 gives you full control over your compute environment.
* Offers flexibility in pricing, instance types, and OS/software.
* Ideal for scalable, enterprise-level workloads.
* Requires proper setup for security, cost optimization, and availability.

---


## 🚀 Spring Boot App Deployment to EC2 using S3 Pre-Signed URL (100% UI + minimal EC2 CLI)

### ✅ Overview

This guide helps you:

* Upload a Spring Boot `.jar` to S3
* Generate a **pre-signed URL**
* Launch an EC2 instance
* Use **EC2 Instance Connect** to access EC2 from browser
* Download and run the app using `wget` with the URL

---

## 📋 Step-by-Step Instructions

---

### 🔧 1. **Build Your Spring Boot JAR**

* In your IDE, run:

  ```
  ./mvnw clean package
  ```
* Locate the final `.jar` file in `target/`.

---

### 🪣 2. **Upload JAR to S3**

1. Go to AWS Console → **S3**
2. Click **Create bucket**

   * Bucket name: e.g., `springboot-app-artifacts`
   * Region: Same as your EC2 instance (e.g., `ap-south-1`)
   * Keep block public access **enabled**
   * Click **Create bucket**
3. Open the bucket → Click **Upload**
4. Select your Spring Boot `.jar` file and upload it.

---

### 🔑 3. **Generate Pre-signed URL for Download**

1. In S3, go to the uploaded file.
2. Click the file name → Actions → **Share with a pre-signed URL**
3. Choose expiration time (e.g., 1 hour)
4. Copy the generated **pre-signed URL**.

---

### 💻 4. **Launch EC2 Instance**

1. Go to AWS Console → **EC2** → **Instances**
2. Click **Launch Instance**
3. Set:

   * Name: `springboot-instance`
   * OS: **Amazon Linux 2 AMI**
   * Instance type: `t2.micro` (Free Tier)
   * Key pair: Create one (if needed)
   * **Network settings:**

     * Inbound rules: Add

       * Port 22 (SSH)
       * Port 8080 (for Spring Boot app)
   * Click **Launch**

---

### 🔗 5. **Connect to EC2 via Instance Connect**

1. Go to EC2 → Instances
2. Select your instance → Click **Connect**
3. Choose **EC2 Instance Connect (browser-based)** → Click **Connect**

---

### 🧲 6. **Download Spring Boot JAR using Pre-signed URL**

Once connected in the browser shell:

```bash
# Use the pre-signed URL from step 3
wget "https://springboot-app-artifacts.s3.ap-south-1.amazonaws.com/yourapp.jar?...signature=xyz"
```

---

### ☕ 7. **Install Java (Amazon Linux 2)**

Still in EC2 terminal:

```bash
sudo yum install java-1.8.0-openjdk -y
```

Verify:

```bash
java -version
```

---

### 🚦 8. **Run Spring Boot Application**

```bash
java -jar yourapp.jar
```

* If your app is on port `8080`, and it's open in the security group, you can access:

  ```
  http://<EC2_PUBLIC_IP>:8080
  ```

---

### 🔒 9. **Security Group Configuration Check**

* Go to EC2 → Your Instance → Security tab → Click the security group
* Inbound rules should allow:

  * SSH (port 22)
  * TCP (port 8080)

---

## ✅ Summary

| Step | Description                                |
| ---- | ------------------------------------------ |
| 1    | Build your `.jar` locally                  |
| 2    | Upload to S3 via UI                        |
| 3    | Generate pre-signed URL                    |
| 4    | Launch EC2 via UI                          |
| 5    | Connect using **EC2 Instance Connect**     |
| 6    | Download JAR using `wget <pre-signed-url>` |
| 7    | Install Java                               |
| 8    | Run the app using `java -jar`              |
| 9    | Access your app from public IP             |


# 🚀 Full Deployment Guide: Spring Boot App on AWS (EC2 + RDS + S3)

## 🧾 Overview

This guide explains how to deploy a Java Spring Boot **CRUD backend** application on **AWS Cloud**, using:

* **Amazon EC2** – For hosting the Spring Boot app
* **Amazon RDS (MySQL)** – As the relational database
* **Amazon S3** – To transfer your app's JAR file from local system to EC2

---

## 🛠️ Step-by-Step Deployment Guide

---

### ✅ 1. **Prepare Spring Boot App**

* Use a prebuilt Spring Boot CRUD app (e.g., Product management).
* Typical structure:

  * `Product` entity → `ProductRepository` (JPA) → `ProductService` → `ProductController`
* Test it locally using Postman and MySQL.

---

### ✅ 2. **Provision Amazon RDS (MySQL)**

* Go to AWS Console → **RDS → Create Database**

  * **Engine**: MySQL
  * **Edition**: Standard Create
  * **Tier**: Free Tier (if eligible)
  * **Instance ID**: `productdb`
  * **Master username/password**: Set and note securely
  * **Public Access**: Yes (for development only)
  * **Port**: 3306
  * **Initial DB Name**: `product_db`

#### 🔒 Edit RDS Security Group

* Add **Inbound Rule**:

  * Type: Custom TCP
  * Port: 3306
  * Source: **Your IP Address** (use `0.0.0.0/0` only for temporary testing)

---

### ✅ 3. **Verify RDS Connection**

* Use **MySQL Workbench**:

  * Hostname: RDS endpoint
  * Port: 3306
  * Username: `admin`
  * Password: (set above)
  * Test connection and check database visibility

---

### ✅ 4. **Update Application Configuration**

In `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://<RDS-ENDPOINT>:3306/product_db
spring.datasource.username=admin
spring.datasource.password=<your-password>
```

* Run locally to confirm integration with RDS

---

### ✅ 5. **Build the Spring Boot App**

```bash
mvn clean install
```

* Output: `.jar` file located in `target/` (e.g., `backend-app.jar`)

---

### ✅ 6. **Upload JAR to Amazon S3**

#### 🪣 Create S3 Bucket

* Go to **S3 → Create bucket**

  * Unique bucket name (e.g., `springboot-app-uploads`)
  * Region: Same as EC2
  * Block Public Access: Keep **enabled**
  * No need for versioning/static website

#### 📤 Upload JAR File

* Upload your `backend-app.jar` to the bucket

#### 🔗 Create a Pre-Signed URL

* From the S3 Console:

  * Select the JAR file → **Actions → Share with a pre-signed URL**
  * Set expiration time (e.g., 1 hour)
  * Copy the URL

---

### ✅ 7. **Provision EC2 Instance**

#### 🚀 Launch EC2

* AWS Console → EC2 → Launch Instance

  * Name: `springboot-backend`
  * AMI: **Amazon Linux 2 AMI**
  * Instance type: `t2.micro` (Free Tier)
  * Key pair: Create/download `.pem` file
  * Security group:

    * Allow **SSH (22)** from your IP
    * Allow **TCP (8080)** from `0.0.0.0/0` (or your IP)

#### 🌐 Get Public IP/DNS

* Note public IPv4 or public DNS of the EC2 instance

---

### ✅ 8. **Connect to EC2 Instance**

* Use **EC2 Instance Connect** (no CLI/SSH needed):

  * AWS Console → EC2 → Connect → EC2 Instance Connect

---

### ✅ 9. **Install Java on EC2**

```bash
sudo dnf install java-17-amazon-corretto -y
java -version
```

---

### ✅ 10. **Download JAR from S3**

Use `wget` with your **pre-signed URL** inside EC2:

```bash
wget "<pre-signed-url>"
```

> Example:

```bash
wget "https://springboot-app-uploads.s3.amazonaws.com/backend-app.jar?...signedParams..."
```

---

### ✅ 11. **Run the Spring Boot App**

```bash
java -jar backend-app.jar
```

* Ensure no errors, and server binds to **port 8080**

---

### ✅ 12. **Test the Deployment**

Use Postman or browser:

```http
http://<EC2-PUBLIC-IP>:8080/api/products
```

* Test CRUD operations

---

## 🔐 Key Security & Best Practices

| Resource | Setting                            | Recommendation                             |
| -------- | ---------------------------------- | ------------------------------------------ |
| **RDS**  | Public Access                      | Yes (for dev), **No for production**       |
|          | Security Group                     | Only allow port 3306 from trusted IPs      |
| **EC2**  | Public Access                      | OK for app testing; restrict in production |
|          | Security Group                     | Allow port 8080 only from your IP          |
| **S3**   | Pre-Signed URL for secure download | ✅ Used instead of public access            |
| **IAM**  | Create roles for production access | Avoid using root or admin long-term        |

---

## ✅ Summary Table

| Component        | Purpose                                 |
| ---------------- | --------------------------------------- |
| Spring Boot      | Backend app providing CRUD API          |
| Amazon RDS       | Cloud-based MySQL database              |
| Amazon EC2       | Virtual machine to host the application |
| Amazon S3        | Temporary file storage for deployment   |
| Instance Connect | UI-based SSH alternative for EC2 access |

---
