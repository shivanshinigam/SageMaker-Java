# SageMaker Java + Python Demo

## What we did 

* Set up **Amazon SageMaker Unified Studio** in AWS
* Created a **SageMaker Domain** (workspace)
* Opened **SageMaker Studio** and created a notebook
* Ran **sample Python ML code** inside SageMaker
* Used **Java (AWS SDK)** locally to connect to SageMaker
* Verified Java access by listing the SageMaker **Domain**

---

## 1. SageMaker setup (AWS Console)

```bash
# Login to AWS Console
# Region: us-east-1

# Open SageMaker
# Setup SageMaker Unified Studio
# Auto-create execution role
# Use AWS-owned KMS key
```

This creates a **SageMaker Domain** (your ML workspace).

---

## 2. Run sample ML code in SageMaker (Python)

Inside SageMaker Studio notebook:

```python
import numpy as np
from sklearn.linear_model import LinearRegression

X = np.array([[1], [2], [3], [4]])
y = np.array([2, 4, 6, 8])

model = LinearRegression()
model.fit(X, y)

print("Model trained successfully in SageMaker")
```

Output:

```text
Model trained successfully in SageMaker
```

<img width="1440" height="900" alt="Screenshot 2025-12-15 at 9 56 38 PM" src="https://github.com/user-attachments/assets/1a3e94b4-c331-4d5f-8a40-43c0febd770a" />


This proves **ML/AI code runs inside SageMaker**.

---

## 3. Java setup (local machine)

Set AWS credentials (local only, not committed):

```bash
export AWS_ACCESS_KEY_ID=XXXX
export AWS_SECRET_ACCESS_KEY=YYYY
export AWS_DEFAULT_REGION=us-east-1
```

---

## 4. Java Maven dependency

```xml
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>sagemaker</artifactId>
    <version>2.25.24</version>
</dependency>
```

---

## 5. Java code (Controller)

```java
import software.amazon.awssdk.services.sagemaker.SageMakerClient;
import software.amazon.awssdk.services.sagemaker.model.ListDomainsRequest;
import software.amazon.awssdk.regions.Region;

public class SageMakerJavaDemo {
    public static void main(String[] args) {
        SageMakerClient client = SageMakerClient.builder()
                .region(Region.US_EAST_1)
                .build();

        client.listDomains(ListDomainsRequest.builder().build())
              .domains()
              .forEach(d -> System.out.println("Domain: " + d.domainName()));

        client.close();
    }
}
```

Example output:

```text
Domain: d-xxxxxxxxxxxx
```

This proves **Java connects to SageMaker and controls resources**.

---

## 6. Java example – list training jobs (optional)

```java
package com.shivanshi.aws;

import software.amazon.awssdk.services.sagemaker.SageMakerClient;
import software.amazon.awssdk.services.sagemaker.model.ListTrainingJobsRequest;
import software.amazon.awssdk.regions.Region;

public class ExampleListTrainingJobs {

    public static void main(String[] args) {

        SageMakerClient client = SageMakerClient.builder()
                .region(Region.US_EAST_1)
                .build();

        ListTrainingJobsRequest request =
                ListTrainingJobsRequest.builder().build();

        client.listTrainingJobs(request)
              .trainingJobSummaries()
              .forEach(job ->
                  System.out.println(job.trainingJobName())
              );

        client.close();
    }
}
```

<img width="1440" height="900" alt="Screenshot 2025-12-15 at 9 58 07 PM" src="https://github.com/user-attachments/assets/54255772-9b2a-4a8f-b38c-e3661912766f" />



**What this does:**

* Java asks SageMaker for existing **training jobs**
* If no training jobs exist, output will be blank
* Blank output still proves Java → SageMaker connectivity

---

## Final summary

* **Java** = controller (connects to SageMaker, manages resources)
* **Python** = ML code (runs inside SageMaker)
* **SageMaker Domain** = workspace that holds notebooks and ML work

