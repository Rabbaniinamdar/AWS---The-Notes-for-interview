# AWS(Amazon Web Service) Notes

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
