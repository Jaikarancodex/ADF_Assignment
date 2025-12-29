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

---

# 💥 Question 4

* Database name: sales_view 

<img width="1918" height="400" alt="image" src="https://github.com/user-attachments/assets/e37e48a0-db2e-41a4-871f-fdaa613fc27a" />

---

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

## ✅ Final Step
<img width="1918" height="937" alt="image" src="https://github.com/user-attachments/assets/43e0dd23-55c5-4117-b74c-52a232bd68cf" />

## ✔  Step 9
 Write based on upsert [table_name: customer] (in silver layer path is silver/sales_view/tablename/{delta pearquet} 
 * remove leading/trailing spaces
 * replace spaces with underscore
 * fix double underscores
 <img width="1918" height="587" alt="image" src="https://github.com/user-attachments/assets/8a41e854-9e62-438b-ae17-56ae13b4fbcb" />

### 📌 Create an container for silver and save path to store
<img width="1918" height="371" alt="image" src="https://github.com/user-attachments/assets/15c1c273-337e-4998-a6e0-8727887c7048" />

### ✅ Final Result
<img width="1918" height="788" alt="image" src="https://github.com/user-attachments/assets/2c113a2c-34d9-4936-b05a-152f33e60efc" />

---

# 💥 Question 6 (Product File) 

###  Create DataFrame with Load Path
<img width="1918" height="870" alt="image" src="https://github.com/user-attachments/assets/3ff2e4f1-5340-4bdf-85f9-a5b6f238c13a" />

## ✔  Step 1
All the column header shlould be in snake case in lower case (use same UDF Function) 
<img width="1918" height="898" alt="image" src="https://github.com/user-attachments/assets/884d8e50-2d51-4f6c-a6d9-864e53ae9194" />

## ✔  Step 2
Create a column sub_category (Use Category columns id category_id=1, "phone"; 2, "laptop"; 3,"playstation"; 4,"e-device" 
<img width="1918" height="942" alt="image" src="https://github.com/user-attachments/assets/d7bb9225-e255-4803-bb18-a9799941405c" />

## ✔  Step 3
Write based on upsert [table_name: product](in silver layer path is silver/sales_view/tablename/{delta pearquet} 
<img width="1918" height="841" alt="image" src="https://github.com/user-attachments/assets/e5bc056a-8fdf-4d87-af77-f82ba5d685e3" />

---

# 💥 Question 6 (Store File) 

###  Create DataFrame with Load Path
<img width="1918" height="842" alt="image" src="https://github.com/user-attachments/assets/296ab58c-2cda-4444-9923-def1bddc6a32" />

##  ✔  Step 1
Read the data make sure header shlould be in snake case in lower case (use same UDF Function) 
<img width="1917" height="821" alt="image" src="https://github.com/user-attachments/assets/689bdeea-a41b-439f-9c76-321ed139b54b" />

##  ✔  Step 2
Create a store category columns and the value is exatracted from email Eg: "electromart" from johndoe@electromart.com 
<img width="1918" height="787" alt="image" src="https://github.com/user-attachments/assets/151e5987-50f9-46dd-a815-0da32b448c32" />

##  ✔  Step 3
created_at, updated_at date as yyyy-MM-dd format 

### 📌 Substep 1
<img width="1917" height="423" alt="image" src="https://github.com/user-attachments/assets/65b30875-b2c7-4fe2-ad74-96dabfb892dc" />

### 📌 Substep 2
<img width="1918" height="597" alt="image" src="https://github.com/user-attachments/assets/55b7766f-546e-4432-81c8-ddc9df391af0" />

<img width="1918" height="643" alt="image" src="https://github.com/user-attachments/assets/e56f3ff6-3bfe-45be-8b94-593a913470ea" />

##  ✔  Step 4
Write based on upsert [table_name: store] (in silver layer path is silver/sales_view/tablename/{delta pearquet} 
<img width="1918" height="922" alt="image" src="https://github.com/user-attachments/assets/ffa366b0-ba42-4a23-92e7-5523b592cabf" />

---

# 💥 Question 7 (Sales File) 

###  Create DataFrame with Load Path
<img width="1917" height="842" alt="image" src="https://github.com/user-attachments/assets/afd7055a-3e70-4e5c-983f-83d7574ccae4" />

##  ✔  Step 1
Read the data make sure header shlould be in snake case in lower case (use same UDF Function) 
<img width="1918" height="687" alt="image" src="https://github.com/user-attachments/assets/55a90aa7-f7ce-457a-abdc-2e253d8fc042" />

##  ✔  Step 2
Write based on upsert [table_name: customer_sales] (in silver layer path is silver/sales_view/tablename/{delta pearquet} 
<img width="1917" height="798" alt="image" src="https://github.com/user-attachments/assets/c2aa1b2d-4194-475a-98b2-d3d82db6fe90" />

---

# 💥 Question 7 (gold layer) 

###  Config Ssetup and Create DataFrame with Load Path 
<img width="1918" height="892" alt="image" src="https://github.com/user-attachments/assets/45f9a700-0d2e-4d2b-aa43-b1c08eca1abe" />


##  ✔  Step 1
### using product and store table get the below data 
store_id,store_name,location,manager_name,product_name,product_code,description,category_id,price,stock_quantity,supplier_id,product_created_at,product_updated_at,image_url,weight,expiry_date,is_active,tax_rate. 


### 📌 Substep 1
Check Schema
<img width="1918" height="463" alt="image" src="https://github.com/user-attachments/assets/77403d6e-cb99-4b49-a1c9-745928cdb8ac" />

### 📌 Substep 2
#### Due to No Id Match Can't perfrom **inner join**
<img width="1918" height="721" alt="image" src="https://github.com/user-attachments/assets/a00d3b24-122c-4406-a736-3bd4d19b1d7c" />

### 📌 Substep 3
#### Left Join
<img width="1917" height="743" alt="image" src="https://github.com/user-attachments/assets/13602016-f24c-45c1-9a7e-58c5f6419d4b" />

### ✅  Final Step
<img width="1918" height="893" alt="image" src="https://github.com/user-attachments/assets/6cd2abb5-b0e3-4f7e-916f-0e551a75dd1c" />

##  ✔  Step 2
Write based on overwrite (table_name : StoreProductSalesAnalysis )(in gold layer path is gold/sales_view/tablename/{delta pearquet} 
<img width="1918" height="410" alt="image" src="https://github.com/user-attachments/assets/d9d78f83-a173-4a11-a2e7-a133fe781e4c" />

##  ✔  Step 3
Read the delta table (using UDF functions) 
<img width="1913" height="887" alt="image" src="https://github.com/user-attachments/assets/da61f97b-3099-4e50-a92e-a151004e3617" />

---

# 🎯 So, at the end below are things you need to have. 
## ✔ In ADLS  
###  Paths: Are mentioned above 
## 📂 Bronze Layer (4 folder and raw files inside) Copied from ADF 

* 1)customer 
* 2)product 
* 3)store 
* 4)sales 

<img width="1918" height="555" alt="image" src="https://github.com/user-attachments/assets/c5707d0c-666f-41f6-87c8-52eb106ea685" />

---

## 📂 Silver Layer (4 folders with table name inside silver) 

* 1)customer 
* 2)product 
* 3)store 
* 4)customer_sales

<img width="1918" height="578" alt="image" src="https://github.com/user-attachments/assets/0bd955a0-9feb-4363-802d-be4949de5748" />

### Example delta file inside Customer
<img width="1918" height="462" alt="image" src="https://github.com/user-attachments/assets/60bd21b7-6e3c-41b4-a6cb-f59d6b4b03ad" />

---

## 📂  Gold Layer (1 folder with table name inside gold) 

* 1)StoreProductSalesAnalysis	 
<img width="1917" height="427" alt="image" src="https://github.com/user-attachments/assets/2af00a15-37c6-4b54-9684-199e17263517" />

---

## 🚀 In databricks 

* bronze_to_silver - creating of silver tables 
* silver_to_gold - creating of gold tables 

<img width="1918" height="667" alt="image" src="https://github.com/user-attachments/assets/79aff22a-15ac-40d5-9458-740ced2c00d8" />

---

## 💥 Question1 - What is Data Profiling? 

**Data Profiling** is the process of **examining, understanding, and validating data** to assess its **quality, structure, patterns, and anomalies** before using it for analytics or downstream processing.

### Why it is done

* Identify **data quality issues**
* Understand **data distributions & patterns**
* Validate **business rules**
* Decide **transformation & cleansing logic**
* Reduce **pipeline failures in production**

📌 *In short:*
 Data profiling tells **what data you really have**, not what you assume.

---

## 💥 Question2 - How Data Profiling Is Done (General Steps) 

Typical profiling activities include:

1. **Schema Profiling**

   * Column names
   * Data types
   * Nullable vs non-nullable fields

2. **Content Profiling**

   * Min / Max values
   * Distinct values
   * Null counts

3. **Pattern & Format Profiling**

   * Date formats
   * Email patterns
   * String casing

4. **Relationship Profiling**

   * Derived columns
   * Split / extract logic
   * Business-rule based columns

5. **Quality Rule Validation**

   * Threshold checks
   * Standardization rules
   * Conditional logic

---

## 💥 Question3 - Now write a usecase based on your understanding what is the architecture followed, completed understanding from end to end. 

Below are the **exact profiling steps That I performed**, mapped directly to my implementation:

### 🔹 Step 1: Column Name Profiling

* Observed **mixed / camel case headers**
* Identified inconsistency across files
* Applied **dynamic camelCase → snake_case**

📌 *Profiling Outcome:* Column standardization required

---

### 🔹 Step 2: Name Column Profiling

* Detected full name stored in single column
* Identified space `" "` as delimiter
* Split into `first_name` and `last_name`

📌 *Profiling Outcome:* Attribute normalization needed

---

### 🔹 Step 3: Email Pattern Profiling

* Analyzed email format: `name@domain.com`
* Extracted **domain** as a new analytical column

📌 *Profiling Outcome:* Valuable derived attribute available

---

### 🔹 Step 4: Gender Value Profiling

* Identified textual values: `male`, `female`
* Standardized into categorical values: `M`, `F`

📌 *Profiling Outcome:* Data normalization & storage optimization

---

### 🔹 Step 5: Date-Time Profiling

* Found combined date & time in single column
* Split into:

  * `date`
  * `time`
* Reformatted date to `yyyy-MM-dd`

📌 *Profiling Outcome:* Analytical & partition-friendly format

---

### 🔹 Step 6: Numerical Range Profiling

* Analyzed `spent` column distribution
* Business threshold identified: `200`
* Derived `expenditure_status`:

  * `< 200 → MINIMUM`
  * `>= 200 → MAXIMUM`

📌 *Profiling Outcome:* Business classification logic applied

---

### 🔹 Step 7: Data Type Profiling

* Detected numeric columns stored as string
* Casted required fields to **double**

📌 *Profiling Outcome:* Correct typing for aggregation

---

### 🔹 Step 8: Whitespace & Naming Quality Profiling

* Removed leading/trailing spaces
* Replaced spaces with underscore
* Fixed double underscores

📌 *Profiling Outcome:* Delta table compatibility ensured

---

## 💥 Question4 - Complete the above practical usecase given  

### Use Case: **Sales View Data Platform**

**Objective:**
Build a scalable, metadata-driven ingestion and transformation pipeline to process raw ADLS data into analytics-ready Delta tables.

---

### 🔹 Architecture Followed 

```
Source (ADLS - Landing)
        |
        v
ADF (Latest File Detection)
        |
        v
Bronze Layer (Raw Copy)
        |
        v
Databricks (Profiling + Transformations)
        |
        v
Silver Layer (Cleaned Delta Tables)
```

---

### 🔹 Architecture Explanation

#### 📂 Source Layer (ADLS – Landing)

* Multiple entities: customer, product, store, sales
* Files arrive daily with no fixed naming convention

---

#### ✔ Ingestion Layer (ADF)

* Parameterized pipeline
* Dynamically identifies **latest modified file**
* Avoids hard-coded dates & file names
* Copies only valid latest data into Bronze

---

#### ✔ Bronze Layer

* Stores raw ingested files
* Acts as **audit & recovery layer**
* No transformations applied

---

#### ✔ Transformation Layer (Databricks)

* Performs complete **data profiling**
* Applies:

  * Standardization
  * Cleansing
  * Derivations
  * Type casting
* Uses UDFs for dynamic scalability

---

#### ✔ Silver Layer

* Stores clean, structured Delta tables
* Upsert logic applied
* Optimized for analytics & reporting

---

## Final Answer (2 Lines) ⭐

> Data profiling is the process of analyzing source data to understand its structure, quality, and patterns before transformation.
> In this project, profiling was performed on column formats, data types, patterns, and business thresholds, enabling a metadata-driven ADF ingestion and Databricks-based Bronze-to-Silver architecture.

---











