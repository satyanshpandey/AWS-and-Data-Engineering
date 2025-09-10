# AWS Cheat Sheet — Data Engineer Intro & Key Services

---

# Introduction: AWS for Data Engineers

* **AWS for Data Engineers:** Provides scalable, on-demand infrastructure and services to store, process, and analyze big data.
* **Core workflow:** Ingest → Store → Process → Analyze → Visualize.
* **Common tools used:** S3 (storage), IAM (security), EC2 (compute), Lambda (serverless compute), Glue (ETL), Redshift (data warehouse), EMR (big data processing), Kinesis (streaming), RDS (relational DB), Athena (query S3 data).

---

# Important AWS Services for Data Engineers

* **Amazon S3** – Object storage (data lake foundation).
* **AWS IAM** – Security, users, roles, and permissions.
* **AWS EC2** – Virtual machines for compute.
* **AWS Lambda** – Serverless compute.
* **Amazon RDS** – Managed relational databases.
* **Amazon Redshift** – Data warehouse.
* **AWS Glue** – Serverless ETL (extract, transform, load).
* **Amazon Kinesis** – Real-time streaming.
* **Amazon Athena** – Query S3 data with SQL.
* **Amazon EMR** – Big data (Hadoop, Spark).
* **CloudWatch & CloudTrail** – Monitoring & logging.

---

# Topic: Amazon S3 (Simple Storage Service)

## Core concepts

* **What it is:** Object storage service for storing/retrieving any amount of data.
* **Key components:** Buckets, Objects, Keys, Regions, Storage classes, Versioning.
* **Use-cases:** Data lake, backups, logs, static website hosting.

## Important steps (Practical Setup)

1. **Create bucket:** AWS Console → S3 → Create bucket → unique name + region.
2. **Set options:** Block Public Access (default), encryption, versioning.
3. **Upload file:** Open bucket → Upload → add file → Upload.
4. **Access file:** Default = private. Use pre-signed URL or bucket policy for sharing.
5. **Versioning:** Enable in bucket → recover deleted/modified files.
6. **Lifecycle:** Set rules to move data to Glacier/Intelligent-Tiering.

## Storage classes

* **S3 Standard:** Frequent access.
* **S3 Intelligent-Tiering:** Auto moves between frequent/infrequent.
* **S3 Standard-IA (Infrequent Access):** Lower cost, retrieval fee.
* **S3 One Zone-IA:** Stored in 1 AZ only.
* **S3 Glacier Instant Retrieval:** Cheap archival, ms retrieval.
* **S3 Glacier Flexible Retrieval:** Hours retrieval, cheaper.
* **S3 Glacier Deep Archive:** Lowest cost, 12+ hours retrieval.

## Best practices

* Block public access unless needed.
* Enable versioning + lifecycle.
* Encrypt sensitive data (SSE-KMS).
* Monitor via CloudWatch.

## Visual memory aid

**Mnemonic:** S3 = **S**tore, **S**ecure, **S**cale.

```
[Client] → [Bucket] → {Objects} → Lifecycle → Glacier
```

## Common pitfalls

* Accidentally public bucket.
* Confusing IAM vs bucket policy vs ACL.
* Not using lifecycle → high cost.

## Short revision notes

* Object store = buckets + objects.
* Choose correct storage class.
* Use versioning + encryption.

---

# Topic: AWS IAM (Identity & Access Management)

## Core concepts

* **What it is:** Service for managing access to AWS resources.
* **Key components:** Users, Groups, Roles, Policies.
* **Use-cases:** Secure access control, least-privilege.

## Practical steps: Create IAM Role

1. **Open IAM Console** → Roles → Create role.
2. **Select trusted entity** → AWS service (e.g., EC2, Lambda).
3. **Attach policy** → e.g., AmazonS3FullAccess.
4. **Name role** → Create.
5. **Assign role** → Attach role to EC2/Lambda.

## Best practices

* Never use root user.
* Grant least-privilege.
* Use IAM roles for services instead of access keys.
* Enable MFA for accounts.

## Mnemonic

IAM = **I**dentities, **A**ccess, **M**anagement.

## Common pitfalls

* Using root user for daily tasks.
* Hardcoding access keys.
* Overly broad policies (e.g., `*` permissions).

## Short revision notes

* Manage users/roles/groups.
* Always use roles instead of keys.
* Policies = JSON docs.

---

# Topic: AWS Lambda

## Core concepts

* **What it is:** Serverless compute that runs code on events.
* **Trigger sources:** S3 events, API Gateway, DynamoDB streams, CloudWatch events.
* **Use-cases:** ETL, serverless APIs, automation.

## Practical steps

1. **Create Lambda function:** Console → Lambda → Create function → Author from scratch.
2. **Choose runtime:** Python, Node.js, etc.
3. **Set permissions:** Attach IAM role with necessary permissions.
4. **Write code:** Inline editor or upload zip.
5. **Configure trigger:** Example: S3 → trigger Lambda on file upload.
6. **Test function:** Use test events or actual triggers.

## Best practices

* Keep function small and single-purpose.
* Use environment variables for configs.
* Monitor with CloudWatch.
* Minimize cold start by using right memory/runtime.

## Visual memory aid

**Mnemonic:** LAMBDA = **L**ightweight, **A**utomated, **M**icro, **B**ackend, **D**ynamic, **A**ctions.

```
[S3 Upload] → [Lambda] → [Process/Store Data]
```

## Common pitfalls

* Exceeding timeout limits.
* Missing IAM permissions.
* Large deployment packages.

## Short revision notes

* Event-driven serverless compute.
* Supports Python/Node/Java.
* Trigger from S3, API Gateway, etc.

---

✅ Covered: AWS Intro for Data Engineers, Key services list, **S3 (with storage classes & steps)**, **IAM (role practical)**, **Lambda (intro + practical)**.
