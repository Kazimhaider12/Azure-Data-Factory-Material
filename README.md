# Azure-Data-Factory-Material

# 🚀 My First Azure Data Factory Pipeline

## 📌 Project Overview

This is my **first hands-on pipeline built using Azure Data Factory (ADF)**.

The purpose of this pipeline is to identify a specific file from a source container and **move it to another container** based on a filename condition.

This small project helped me understand how ADF activities, expressions, metadata, looping, conditions, and data movement work together.

---

# 🎯 Project Requirement

The source container contains two CSV files:

```text
fact_sales_!.csv
fact_sales_2.csv
```
Requirement

Move only the file whose name contains:

!

So:

fact_sales_!.csv
        ↓
Move to Destination Container

While:

fact_sales_2.csv
        ↓
Do Nothing

🏗️ Pipeline Architecture
Source Container
       ↓
  Get Metadata
       ↓
     ForEach
       ↓
   If Condition
       ↓
Contains "!"
       ↓
  Copy Activity
       ↓
Destination Container
       ↓
 Delete Activity
🔧 Azure Services / Activities Used
1. Azure Data Factory

Azure Data Factory is used to create and orchestrate the entire pipeline.

It controls:

Pipeline execution
Activity dependencies
File processing
Conditional logic
Data movement

<img width="1919" height="855" alt="image" src="https://github.com/user-attachments/assets/a56e02df-75aa-4162-906a-3d0bed5ea398" />
<img width="1686" height="756" alt="image" src="https://github.com/user-attachments/assets/339475fd-9f85-41bc-8265-c9ebaad35a33" />
<img width="1466" height="289" alt="image" src="https://github.com/user-attachments/assets/1e015106-3b98-4e1e-a8fc-3964f81f6cde" />
