
# 📊 End-to-End Power BI Dashboard Development

Creating a Power BI dashboard involves a structured sequence of steps:

1. Understanding Requirements
2. Gathering Relevant Data
3. Transforming and Modeling the Data
4. Planning the Visual Structure
5. Designing the Dashboard Layout
6. Incorporating Interactive Elements and Navigation
7. Conducting Thorough Testing
8. Publishing and Sharing the Dashboard
9. Ongoing Maintenance and Scheduled Data Refreshes

Below is a complete walkthrough of each step as if I'm actively building the dashboard from scratch.

---

## 🧩 Step 1: Requirement Gathering

To kick off the dashboard development, I identified key stakeholders—primarily healthcare domain experts and decision-makers who will use the dashboard to monitor patient waiting lists. A clear point of contact was established to streamline communication.

I conducted meetings and calls to understand the business goals. By asking open-ended questions, I gathered insights on how the dashboard could support strategic decisions and improve visibility into wait times.

I also assessed the available data by reviewing:

* 📁 **Data Sources**
* 📊 **Column Descriptions & Data Types**
* 🔄 **Volume and Update Frequency**
* ⚠️ **Data Quality – Missing Values & Anomalies**

### 🎯 Problem Statement & Objectives

* Monitor the **current status** of the patient waiting list
* Analyze **monthly trends** in inpatient and outpatient categories
* Provide **granular insights** by specialty and age profile

### 📅 Data Scope

* Data from **2018 to 2021**

### 📈 Metrics to Track

* **Average & Median** Waiting List
* **Current Total** Wait List

### 🖥️ Dashboard Views

* **Summary Page** for high-level insights
* **Detailed Page** for deeper analysis

---

## 📥 Step 2: Data Collection

For this project, I selected a **Folder connection** to serve as the central data source. This setup supports automated refreshes and is scalable.

### Common connectors considered:

* Excel / CSV
* Folder Connection
* SQL Server
* SharePoint
* Web / JSON APIs
* Cloud Platforms (Azure, AWS, GCP)
* Power BI Service
* ERP Systems (Salesforce, SAP)

---

## 🛠️ Step 3: Data Transformation

All transformations were performed in the **Power Query Editor**.

### ✏️ Renaming Columns

Renamed `Specialty` in the Outpatient table to `Specialty_Name` for consistency.

### 📑 Rearranging Columns

Rearranged columns in the Outpatient table to match the Inpatient structure. Added a `Case_Type` column with the value `"Outpatient"`.

### 🔗 Appending Tables

Appended the two tables:

* Used **Append Queries as New**
* Created `All_Data` as the combined table
* Finalized with **Close & Apply**

### 🧹 Cleaning Data

* Standardized values in `Age_Profile` and `Time_Band`
* Used **Replace Values** and **Trim** functions

### 🧩 Data Modeling

* Hid original tables and disabled load
* Imported **Specialty Mapping** table to group specialties
* Created relationship: `All_Data[Specialty_Name]` → `Mapping[Specialty]`

---

## 📊 Step 4: Visualization Blueprint

Before development, I sketched a blueprint/wireframe and aligned it with stakeholder feedback.

<img width="711" height="310" alt="image" src="https://github.com/user-attachments/assets/6c18026b-8ed1-43a2-89be-78261738fddb" />

---

## 🎨 Step 5: Dashboard Layout & Design

Enabled **Gridlines** and **Snap to Grid** in the View tab to maintain layout consistency.

### 🧮 Creating Key Measures (DAX)

```DAX
Latest Month Wait List = 
CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date] = MAX(All_Data[Archive_Date])) + 0

PY Latest Month Wait List = 
CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date] = EDATE(MAX(All_Data[Archive_Date]), -12)) + 0
```

### 📋 Toggle: Average vs. Median

Created a table with two options: `Average`, `Median`

```DAX
Median Wait List = MEDIAN(All_Data[Total])
Average Wait List = AVERAGE(All_Data[Total])

Avg/Med Wait List = 
SWITCH(VALUES('Calculation Method'[Calc Method]), 
    "Average", [Average Wait List], 
    "Median", [Median Wait List])

Dynamic Title = 
SWITCH(VALUES('Calculation Method'[Calc Method]), 
    "Average", "Key Indicators - Patient Wait List (Average)", 
    "Median", "Key Indicators - Patient Wait List (Median)")
```
## 📈 Step 6: Building Dashboard Pages

### 📌 Summary Page

* **Card Visuals** for KPIs
* **Doughnut Chart** by `Specialty`
* **Clustered Column Chart** for trend
* **Multi-row Card** for top 5 specialties
* **Line Chart** with split by `Case_Type`
* **Slicers** for `Archive_Date`, `Case_Type`, `Specialty`

### 📄 Detailed View Page

Used **Matrix Visual** with:

* `Archive_Date`
* `Specialty_Name`
* `Age_Profile`
* `Time_Bands`
* `Case_Type`
* `Total`

### 💡 Tooltip Page

* **Chart**: `Specialty` vs. `Total Wait List`
* **Card**: Overall total
* Enabled **“Allow Use as Tooltip”**
* Linked tooltip to **line chart** in Summary

---

## 🧭 Step 7: Adding Interactivity

* **Navigation Buttons** between pages
* **Alt Text** for accessibility
* **Tooltips** for charts
* **Dynamic Titles** via DAX

---

## ✅ Step 8: Testing & Sharing

Performed **User Acceptance Testing (UAT)** with real stakeholders.

Security Measures:

* Implemented **Row-Level Security (RLS)** to restrict data views
* Verified with test user roles before publishing

Published to **Power BI Service** and set permissions.

---

## 🔄 Step 9: Scheduled Refresh & Maintenance

* Configured **data refresh schedule** in Power BI Service
* Documented refresh logic, error handling, and admin contacts
* Set reminders for **monthly checks** on data quality & structure

