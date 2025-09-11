# Topic: AWS Lambda — Creating & Triggering Functions

## Core concepts

* **What it is:** Serverless compute service that runs code in response to events.
* **Supported runtimes:** Python, Node.js, Java, Go, etc.
* **Trigger sources:** S3 events, API Gateway, DynamoDB streams, CloudWatch events, Step Functions.

## Practical steps: Create Lambda Function


[User Upload] 
     |
     v
   ( S3 )  --->  ( Lambda Trigger: Resize Image )  
     |                       |
     v                       v
 [Original]           [Resized Image]
                             |
                             v
                          [Client]



1. **Open AWS Console → Lambda → Create function.**
2. **Choose authoring method:** Author from scratch.
3. **Enter details:** Function name, runtime (e.g., Python 3.9).
4. **Set permissions:** Create/choose an IAM role with required policies (e.g., S3 read/write).
5. **Write/upload code:** Use inline editor, upload zip, or connect to S3.
6. **Configure environment variables (if needed).**
7. **Deploy function.**

## Practical steps: Add Trigger

1. **In Lambda console → Select your function.**
2. **Go to Configuration → Triggers → Add trigger.**
3. **Select trigger source:** (e.g., S3 bucket).
4. **Configure event:** Example → S3 → Event type = “PUT (ObjectCreated)” → Select bucket.
5. **Save changes.**
6. **Test:** Upload a file to S3 bucket → Lambda executes automatically.

## Best practices

* Use least-privilege IAM role for Lambda.
* Keep deployment package small.
* Set appropriate timeout & memory.
* Monitor using CloudWatch Logs.

## Visual memory aid

```
[S3 File Upload] ---> [Lambda Function] ---> [Process Data / Store / Notify]
```

**Mnemonic:** LAMBDA = **L**ightweight **A**ctions **M**ade **B**y **D**ynamic **A**utomation.

## Common pitfalls

* Missing IAM role permissions (causes access denied).
* Large deployment package slows cold start.
* Trigger misconfiguration (wrong bucket/event type).

## Short revision notes

* Event-driven serverless compute.
* Create function → attach IAM role → add trigger.
* Common triggers: S3, API Gateway, CloudWatch.
