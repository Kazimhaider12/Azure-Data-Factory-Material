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
2. Get Metadata

The Get Metadata Activity is used to retrieve information about the files available in the source container.

The pipeline retrieves the list of files using:

childItems

Example output:

fact_sales_!.csv
fact_sales_2.csv
3. ForEach Activity

The ForEach Activity loops through every file returned by Get Metadata.

Conceptually:

Files
  ↓
ForEach
  ↓
File 1 → Check condition
File 2 → Check condition
File 3 → Check condition
...

The current file can be accessed using:

@item()

And the current file name using:

@item().name
4. If Condition

The pipeline checks whether the current filename contains !.

Expression used:

@contains(item().name, '!')
What does contains() do?

It checks whether a particular value exists inside a string.

For example:

fact_sales_!.csv

contains:

!

Therefore:

TRUE

But:

fact_sales_2.csv

does not contain !.

Therefore:

FALSE
5. Copy Activity

If the condition evaluates to TRUE, the Copy Activity copies the matching file from the source container to the destination container.

Source Container
      ↓
Copy Activity
      ↓
Destination Container
Important

Copy Activity copies the file.

It does not automatically remove the original file.

6. Delete Activity

To actually move the file instead of simply copying it, a Delete Activity is executed after the Copy Activity.

Copy Activity
      ↓
Delete Activity

Therefore:

Copy + Delete = Move

The original file is deleted only after it has been copied to the destination.

🔄 Complete Pipeline Logic
             Source Container
                    │
                    ↓
              Get Metadata
                    │
                    ↓
               childItems
                    │
                    ↓
                 ForEach
                    │
            ┌───────┴────────┐
            ↓                ↓
     fact_sales_!.csv   fact_sales_2.csv
            │                │
            ↓                ↓
      If Condition      If Condition
            │                │
     contains "!"?      contains "!"?
            │                │
          TRUE             FALSE
            │                │
            ↓                ↓
      Copy Activity        Ignore
            │
            ↓
   Destination Container
            │
            ↓
      Delete Activity
            │
            ↓
      Original Removed
📊 Before Pipeline Execution
Source Container
Source/
│
├── fact_sales_!.csv
└── fact_sales_2.csv
Destination Container
Destination/
│
└── Empty
📊 After Pipeline Execution
Source Container
Source/
│
└── fact_sales_2.csv
Destination Container
Destination/
│
└── fact_sales_!.csv

The file containing ! has been moved successfully.

🧠 Key Concepts Learned

Through this pipeline, I learned:

Azure Data Factory
ADF Pipelines
Get Metadata Activity
childItems
ForEach Activity
If Condition
ADF Expressions
@item().name
@contains()
Copy Activity
Delete Activity
Data Movement
Pipeline Dependencies
Conditional Data Processing
🔑 Important ADF Expressions
Get current item
@item()
Get current file name
@item().name
Check whether filename contains !
@contains(item().name, '!')
💡 Key Learning

This project helped me understand that Azure Data Factory is not just a data movement tool.

ADF can:

Connect
   ↓
Read Metadata
   ↓
Loop
   ↓
Apply Conditions
   ↓
Move Data
   ↓
Delete / Execute Next Activity

It can therefore be used for data integration and workflow orchestration.

<img width="1919" height="855" alt="image" src="https://github.com/user-attachments/assets/a56e02df-75aa-4162-906a-3d0bed5ea398" />
<img width="1686" height="756" alt="image" src="https://github.com/user-attachments/assets/339475fd-9f85-41bc-8265-c9ebaad35a33" />
<img width="1466" height="289" alt="image" src="https://github.com/user-attachments/assets/1e015106-3b98-4e1e-a8fc-3964f81f6cde" />
