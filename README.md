# Azure-Data-Factory-Material

# 🚀 Azure Data Factory — File Movement Pipeline

## 📌 Project Overview

This project is my **first hands-on Azure Data Factory (ADF) pipeline**.

The goal of this pipeline is to identify a specific file from a source container based on its filename and move it to a destination container.

This project helped me understand the fundamentals of:

- Azure Data Factory
- ADF Pipelines
- Get Metadata Activity
- ForEach Activity
- If Condition
- ADF Expressions
- Copy Activity
- Delete Activity
- Conditional File Processing
- Data Movement
- Pipeline Orchestration

---

## 🎯 Project Objective

The source container contains two CSV files:

```text
fact_sales_!.csv
fact_sales_2.csv
````

The requirement is:

> Move only the file whose filename contains `!` to another container.

Therefore:

```text
fact_sales_!.csv
        ↓
    MOVE ✅
        ↓
Destination Container
```

While:

```text
fact_sales_2.csv
        ↓
    IGNORE ❌
```

---

# 🏗️ Pipeline Architecture

```text
┌─────────────────────┐
│   Source Container  │
│                     │
│ fact_sales_!.csv    │
│ fact_sales_2.csv    │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│    Get Metadata     │
│                     │
│     childItems      │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│      ForEach        │
│                     │
│ Process each file   │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│    If Condition     │
│                     │
│ contains "!" ?      │
└──────────┬──────────┘
           │
       ┌───┴───┐
       │       │
     TRUE    FALSE
       │       │
       ↓       ↓
   Copy       Ignore
  Activity
       │
       ↓
┌─────────────────────┐
│ Destination         │
│ Container           │
│                     │
│ fact_sales_!.csv    │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│   Delete Activity   │
│                     │
│ Remove original     │
└─────────────────────┘
```

---

# ☁️ Azure Service Used

# 🔧 Pipeline Activities

## 1. Get Metadata Activity

The first step is to retrieve information about the files available in the source container.

The `childItems` property is used to get the list of files.

Example:

```text
Source Container
│
├── fact_sales_!.csv
└── fact_sales_2.csv
```

The metadata activity returns the files so that they can be processed individually.

---

<img width="1916" height="903" alt="image" src="https://github.com/user-attachments/assets/e0825c14-8405-4cf5-9e1f-2880d5f1e638" />


## 2. ForEach Activity

The `ForEach` activity processes each file returned by the Get Metadata activity.

Conceptually:

```text
childItems
    ↓
ForEach
    ↓
File 1 → Check
File 2 → Check
File 3 → Check
...
```

The current item can be accessed using:

```text
@item()
```

The current file name can be accessed using:

```text
@item().name
```
<img width="1364" height="903" alt="image" src="https://github.com/user-attachments/assets/6f30e93b-9453-4a2d-8de3-8bf991114081" />
<img width="1328" height="669" alt="image" src="https://github.com/user-attachments/assets/03075c3f-01c7-4992-898f-89acf739e0b6" />

---

# 3. If Condition

The pipeline checks whether the current filename contains the character:

```text
!
```

The expression used is:

```text
@contains(item().name, '!')
```

### How it works

For:

```text
fact_sales_!.csv
```

The result is:

```text
TRUE
```

Because the filename contains `!`.

For:

```text
fact_sales_2.csv
```

The result is:

```text
FALSE
```

Because the filename does not contain `!`.

---

<img width="1346" height="742" alt="image" src="https://github.com/user-attachments/assets/3847f21d-857c-4b31-9a20-9beea520319b" />


# 4. Copy Activity

When the If Condition returns `TRUE`, the Copy Activity copies the matching file from the source container to the destination container.

```text
Source Container
       ↓
Copy Activity
       ↓
Destination Container
```

For example:

```text
Source:

fact_sales_!.csv

        ↓ Copy

Destination:

fact_sales_!.csv
```
<img width="1312" height="714" alt="image" src="https://github.com/user-attachments/assets/ee02a039-f289-41a3-b169-00d6bb398682" />

### Important

The Copy Activity only **copies** the file.

It does not remove the original file.

---

# 5. Delete Activity

To make the operation a real **MOVE**, a Delete Activity is executed after the Copy Activity.

```text
Copy Activity
      ↓
Delete Activity
```

Therefore:

```text
COPY + DELETE = MOVE
```

After successful execution:

```text
Source Container
│
└── fact_sales_2.csv
```

And:

```text
Destination Container
│
└── fact_sales_!.csv
```

---

# 🔄 Complete Data Flow

The complete pipeline works like this:

```text
Source Container
       ↓
Get Metadata
       ↓
Get childItems
       ↓
ForEach
       ↓
Read current filename
       ↓
If filename contains "!"
       │
       ├────────────── FALSE ──────────────→ Ignore
       │
       ↓ TRUE
Copy Activity
       ↓
Destination Container
       ↓
Delete Activity
       ↓
Original file removed
```

---

# 📁 Before Pipeline Execution

## Source Container

```text
Source/
│
├── fact_sales_!.csv
└── fact_sales_2.csv
```

## Destination Container

```text
Destination/
│
└── Empty
```

---

# 📁 After Pipeline Execution

## Source Container

```text
Source/
│
└── fact_sales_2.csv
```

## Destination Container

```text
Destination/
│
└── fact_sales_!.csv
```

Only the required file has been moved.

---

# 🧠 Important ADF Expressions

## Get Current Item

```text
@item()
```

Returns the current item being processed inside the ForEach activity.

---

## Get Current File Name

```text
@item().name
```

Returns the name of the current file.

Example:

```text
fact_sales_!.csv
```

---

## Check Filename

```text
@contains(item().name, '!')
```

Checks whether the current filename contains `!`.

---

# 📊 Logic Table

| File Name          | Contains `!` | Result |
| ------------------ | -----------: | ------ |
| `fact_sales_!.csv` |        Yes ✅ | Move   |
| `fact_sales_2.csv` |         No ❌ | Ignore |

---



<img width="1919" height="855" alt="image" src="https://github.com/user-attachments/assets/a56e02df-75aa-4162-906a-3d0bed5ea398" />
<img width="1686" height="756" alt="image" src="https://github.com/user-attachments/assets/339475fd-9f85-41bc-8265-c9ebaad35a33" />
<img width="1466" height="289" alt="image" src="https://github.com/user-attachments/assets/1e015106-3b98-4e1e-a8fc-3964f81f6cde" />
