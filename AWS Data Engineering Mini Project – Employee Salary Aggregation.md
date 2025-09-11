---

# 🚀 AWS Data Engineering Mini Project – Employee Salary Aggregation

## 📌 Core Concepts

* **S3 (Raw Zone):** Source of employee data (hourly incoming CSV/JSON).
* **Trigger:** Whenever file lands, pipeline starts automatically.
* **Lambda / Glue:** Reads data → aggregates salary by country.
* **S3 (Processed Zone):** Stores aggregated results (e.g., Parquet/CSV).

**Use-case:**
👉 Real-world scenario where raw HR data comes hourly, and management needs **aggregated salary insights per country**.

---

## 🔄 System Design / Workflow

```
[Step.1].S3.(Raw.Data).──►.Event.Trigger
.........................|
.........................▼
[Step.2].Lambda/Glue.Job.──►.Read.employee.file
.........................|
.........................▼
[Step.3].Aggregate.salary.by.country.(SUM)
.........................|
.........................▼
[Step.4].Write.back.to.S3.(Processed.Data.Bucket)

```

---

## 🛠️ Step-by-Step Implementation

### Step 1: S3 Setup

* Create **2 buckets**:

  * `employee-data-raw` → incoming hourly files.
  * `employee-data-aggregated` → output salary aggregates.

### Step 2: Event Trigger

* Configure **S3 Event Notification** on `employee-data-raw`.
* Trigger → AWS Lambda (or AWS Glue if dataset is big).

### Step 3: Lambda Function (Python Example)

* Reads employee file.
* Groups data by `country`.
* Aggregates `salary`.
* Writes back results to output bucket.

```python
import boto3
import pandas as pd
import io

s3 = boto3.client('s3')

def lambda_handler(event, context):
    # 1. Get file info
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # 2. Read file from S3
    response = s3.get_object(Bucket=bucket, Key=key)
    df = pd.read_csv(response['Body'])
    
    # 3. Aggregate salary by country
    agg_df = df.groupby('country')['salary'].sum().reset_index()
    
    # 4. Write back to output bucket
    csv_buffer = io.StringIO()
    agg_df.to_csv(csv_buffer, index=False)
    s3.put_object(
        Bucket="employee-data-aggregated", 
        Key=f"aggregated/{key}", 
        Body=csv_buffer.getvalue()
    )
    return {"status": "success"}
```

### Step 4: Validation

* Check **S3 aggregated bucket** → aggregated CSV appears after each file upload.
* Example output:

```
country, total_salary
India, 200000
USA, 300000
UK, 150000
```

---

## ✅ Best Practices

* **Schema consistency:** Ensure all hourly files have same schema.
* **Lambda limits:** If file > 200MB, use **Glue/Spark**.
* **S3 foldering:**

  * `raw/yyyy/mm/dd/`
  * `processed/yyyy/mm/dd/`
* Use **Parquet format** for efficiency if data grows.
* Apply **IAM roles** with least privilege.

---

## 🧠 Mnemonics / Visual Memory Aid

Think: **“R-A-A-P” → Raw → Aggregate → Append → Processed”**

```
Raw Data (S3) → Aggregate (Lambda/Glue) → Append Results → Processed Data (S3)
```

---

## ⚠️ Common Pitfalls

❌ Forgetting to configure **S3 event notifications** → pipeline won’t trigger.
❌ Large files → Lambda timeouts → use Glue.
❌ Schema mismatch in CSV files → Aggregation errors.
❌ Overwriting output files instead of partitioning by time.

---

## 📝 Short Revision Notes

* **S3 → Lambda → Salary Aggregation → S3 Output**
* Use **event-driven triggers** for automation.
* Aggregate using **groupby SUM(country)**.
* Store results in a **separate processed bucket**.
* Watch out for **file size + schema mismatch**.

---
