# 🚀ADF_Assignment

---

# 💥 Question 1
* Create a container in ADLS with the project name as sales_view_devtst (Due to name constraint I used "salesviewdevst")
* Create a folder for customer, product, store, and sales - Upload the files in sequence to make it work in real-time. 
<img width="1918" height="582" alt="image" src="https://github.com/user-attachments/assets/57134db2-4f7a-4c3d-90d4-539e217521f9" />

---

# 💥 Question 2
* Create an ADF Pipeline to get the latest modified files the folders of the ADLS  
* Parameterize the Pipeline to work dynamically. 
* Values need to be passed through parameters and it should work for all day's file	 

## ✔  Step 1 Pipeline Objective
* Dynamically scan ADLS folders
* Identify **latest modified file** for any day
* Copy only that file into **Bronze layer**
* Fully parameterized (container, entity, project)

<img width="1918" height="837" alt="image" src="https://github.com/user-attachments/assets/f7e7d2eb-8483-449a-9b01-e7dff0b6ca12" />


---


## ✔  Step 2 Pipeline Parameters

| Parameter            | Purpose               |
| -------------------- | --------------------- |
| `p_container`        | Source ADLS container |
| `p_entity`           | Folder / entity name  |
| `p_bronze_container` | Bronze container      |
| `p_project`          | Project name          |

<img width="1427" height="256" alt="image" src="https://github.com/user-attachments/assets/41d987c0-5157-4bfb-87eb-226de00ebd6e" />


---

## ✔  Step 3 Get File List (GM_ListFiles)

* Uses **Get Metadata**
* Retrieves `childItems`
* Recursive folder scan
* Dataset path is parameterized

<img width="1918" height="955" alt="image" src="https://github.com/user-attachments/assets/35c2da59-7167-47c1-a607-21715c8b3129" />


---

## ✔  Step 4 Filter Only Files (FL_OnlyFiles)

* Filters out folders
* Keeps only items where:

```text
item().type == 'File'
```

<img width="1918" height="696" alt="image" src="https://github.com/user-attachments/assets/0b7ce0a3-f982-4431-820e-bd8e1693bed2" />


---

## ✔  Step 5 Loop Through Files (FE_ProcessFiles)

* Iterates file list sequentially
* For each file:

  * Fetches `lastModified`
  * Compares with `v_latestTime`

<img width="1918" height="688" alt="image" src="https://github.com/user-attachments/assets/3584afb6-0568-4436-a175-68b60b2142e6" />
<img width="1918" height="937" alt="image" src="https://github.com/user-attachments/assets/a217edf8-e8a5-419f-9287-19e03746aea4" />


---

## ✔  Step 6 Identify Latest File (IF_IsLatestFile)

* Condition:

```text
lastModified > v_latestTime
```

* If true:

  * Update `v_latestTime`
  * Update `v_latestFile`

<img width="1918" height="825" alt="image" src="https://github.com/user-attachments/assets/ea7d413b-696e-446d-baab-11cb8ee921b0" />

---

## ✔  Step 7 Variables Used

| Variable       | Description                         |
| -------------- | ----------------------------------- |
| `v_latestTime` | Stores max `lastModified` timestamp |
| `v_latestFile` | Stores latest file name             |

<img width="1918" height="737" alt="image" src="https://github.com/user-attachments/assets/6f48206a-8408-4a72-bb80-51b3bad38dea" />

---

## ✔  Step 8 Copy Latest File to Bronze

* Copy Activity uses:

```text
@variables('v_latestFile')
```

* Dynamically copies **only latest file**
* Sink path is fully parameterized

<img width="1918" height="846" alt="image" src="https://github.com/user-attachments/assets/31d2da87-c225-4a74-bdc5-e56dd2018a34" />


---

## ✔  Step 9 End Result

* Works for **any date**
* No hard-coded file names
* Fully reusable pipeline

<img width="1917" height="342" alt="image" src="https://github.com/user-attachments/assets/3c9ba49f-9100-4ccb-af91-8bd46da49496" />

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/289937d7-1088-4c09-8bbb-bdc545131f09" />

---


# 💥 Question 3

* Create a Bronze/sales_view/  (Bronze – container) 
* Subfolders->  customer, product, store, sales, and store the raw data copied from the ADF pipeline	 

<img width="1918" height="537" alt="image" src="https://github.com/user-attachments/assets/e64aa220-9285-41e9-8986-9a4d35e65574" />

# 💥 Question 4

* Database name: sales_view 

<img width="1918" height="400" alt="image" src="https://github.com/user-attachments/assets/e37e48a0-db2e-41a4-871f-fdaa613fc27a" />

# 💥 Question 5 (Customer file) 

## ✔  Step 1 Config Setting
<img width="1918" height="435" alt="image" src="https://github.com/user-attachments/assets/7ee7f1a2-8f53-46b2-9868-fe4cd38397d1" />

### bronze customer path
<img width="1918" height="947" alt="image" src="https://github.com/user-attachments/assets/c1e68697-8bcf-42fa-9dca-2c4a28b6da6a" />

## ✔  Step 2
All the column header should be in snake case in lower case (By using UDF function dynamically works for all camel case to snake case) 

<img width="1918" height="973" alt="image" src="https://github.com/user-attachments/assets/e385b6f5-020d-42e7-a019-19421df22444" />

## ✔  Step 3
By using the "Name" column split by " " and create two columns first_name and last_name 

<img width="1918" height="841" alt="image" src="https://github.com/user-attachments/assets/54cba5d9-7da9-446a-a60c-606b1f5d6423" />

## ✔  Step 4
Create column domain and extract from email columns Ex: Email = "josephrice131@slingacademy.com" domain="slingacademy" 
<img width="1918" height="812" alt="image" src="https://github.com/user-attachments/assets/fd4577eb-7ad0-427c-9950-b5309a1210e9" />

## ✔  Step 5
Create a column ‘gender where male = "M" and Female="F" 
<img width="1918" height="888" alt="image" src="https://github.com/user-attachments/assets/a1f9962c-177e-4236-b1cf-24c809f796de" />

## ✔  Step 6
From Joining date create two colums date and time by splitting based on " " delimiter. 
<img width="1918" height="386" alt="image" src="https://github.com/user-attachments/assets/ee742e7e-16cc-4d5c-b4c5-1393e028571f" />

## ✔  Step 7
Date column should be on "yyyy-MM-dd" format. 
<img width="1918" height="381" alt="image" src="https://github.com/user-attachments/assets/c5aacbbc-0c86-477f-896a-26942be0e108" />

## ✔  Step 8
Create a column expenditure-status, based on spent column is spent below 200 column value is "MINIMUM" else "MAXIMUM" 
<img width="1918" height="593" alt="image" src="https://github.com/user-attachments/assets/243d6d58-a2cb-4d17-80b6-f3887b6725ea" />

### 📌 Substep 1
<img width="1918" height="651" alt="image" src="https://github.com/user-attachments/assets/9d447b9e-1e82-4ec4-a783-cabb0c3e78bb" />

### 📌 Substep 2 (Snake Case Again)
<img width="1918" height="623" alt="image" src="https://github.com/user-attachments/assets/c89b432b-2900-47b3-91e7-9afebb4f481d" />

### 📌 Substep 2 (Cast in double)
<img width="1918" height="532" alt="image" src="https://github.com/user-attachments/assets/e1009be8-10f5-4367-9b55-003cda51754c" />

## 📌 Final answer
<img width="1918" height="937" alt="image" src="https://github.com/user-attachments/assets/43e0dd23-55c5-4117-b74c-52a232bd68cf" />

## ✔  Step 9
 Write based on upsert [table_name: customer] (in silver layer path is silver/sales_view/tablename/{delta pearquet} 
 * remove leading/trailing spaces
 * replace spaces with underscore
 * fix double underscores
 <img width="1918" height="587" alt="image" src="https://github.com/user-attachments/assets/8a41e854-9e62-438b-ae17-56ae13b4fbcb" />

### 📌 Create an container for silver and save path to store
<img width="1918" height="371" alt="image" src="https://github.com/user-attachments/assets/15c1c273-337e-4998-a6e0-8727887c7048" />

### 📌 Final Result
<img width="1918" height="788" alt="image" src="https://github.com/user-attachments/assets/2c113a2c-34d9-4936-b05a-152f33e60efc" />

