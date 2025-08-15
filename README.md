**End-End Power BI Dashboard Development**

Creating a Power BI dashboard involves a structured sequence of steps:
- Understanding Requirements
- Gathering Relevant Data
- Transforming and Modeling the Data
- Planning the Visual Structure
- Designing the Dashboard Layout
- Incorporating Interactive Elements and Navigation
- Conducting Thorough Testing
- Publishing and Sharing the Dashboard
- Ongoing Maintenance and Scheduled Data Refreshes
Let’s walk through each of these stages in detail and build a Power BI dashboard from the ground up.
Absolutely! Here's a paraphrased version written as if I'm actively working on the project:



** 🧩 Step 1: Requirement Gathering**

To kick off the dashboard development, I began by identifying the key stakeholders—primarily healthcare domain experts and decision-makers who will be using the dashboard to monitor patient waiting lists. I established a clear point of contact to streamline communication throughout the project.

Next, I conducted a series of meetings and calls to understand the overarching business goals. By asking open-ended questions, I gained valuable insights into how the dashboard could support strategic decisions and improve visibility into patient wait times.

Before diving into detailed metrics, I performed a high-level assessment of the available data. This included reviewing:

- 📁 **Data Sources**  
- 📊 **Column Descriptions & Data Types**  
- 🔄 **Volume and Update Frequency**  
- ⚠️ **Data Quality – Missing Values & Anomalies**

With this foundation, I defined the scope of the dashboard. This involved outlining key metrics, KPIs, and deployment timelines. I documented all necessary calculations and time frames to ensure alignment and transparency. As a precaution, I built in a 20% buffer to our delivery schedule—better to exceed expectations than fall short.

#### 🎯 Problem Statement & Objectives

The goal of this dashboard is to:

- Monitor the **current status** of the patient waiting list  
- Analyze **monthly trends** in both inpatient and outpatient categories  
- Provide **granular insights** by specialty and age profile  

#### 📅 Data Scope  
We’re working with data from **2018 to 2021**

#### 📈 Metrics to Track  
- Average & Median Waiting List  
- Current Total Wait List  

#### 🖥️ Dashboard Views  
- A **Summary Page** for high-level insights  
- A **Detailed Page** for deeper analysis  

---

** 📥 Step 2: Data Collection**

Power BI offers over 200 connectors to bring in data, but for this project, I selected a centralized folder as the data source. This folder will house all necessary files and support automated refreshes once the dashboard is deployed.

Common connectors I considered include:

- Excel / CSV  
- Folder Connection  
- SQL Server / Databases  
- Power BI Services  
- Cloud Platforms (Azure, AWS, GCP)  
- ERP Systems (Salesforce, SAP)  
- SharePoint  
- Web / JSON APIs  

Choosing the folder connection ensures a streamlined refresh process and keeps the solution scalable and easy to maintain.

#3 Data Transformation
Data transformation is a process of changing the structure of your data or applying additional steps which will clean or process your data for final usage. We do these transformations in the Power Query Editor which is inbuilt into Power BI. 
Go to the Report View
Click on the drop down on the Transform data Icon
You will see 3 Options, select the Transform Data option

Absolutely! Here's how I’d walk through the process as if I were doing it myself:

---

**🛠️ Step 3. Data Transformation Steps**

There are plenty of transformation techniques available in Power BI—like Pivot/Unpivot, Merge, and Filtering—but for this task, I’m focusing on just three key steps to get things aligned.

---

### ✏️ Renaming Columns

While reviewing the data, I noticed a mismatch: the *Specialty* column in the Inpatient table is labeled as `Specialty_Name`, but in the Outpatient table, it’s simply `Specialty`. To ensure consistency and avoid issues later, I renamed the Outpatient column to `Specialty_Name`. Precision matters here—any deviation could break downstream steps.

---

### 📑 Rearranging Columns

Next, I rearranged the columns in the Outpatient table to mirror the structure of the Inpatient table. It’s a simple drag-and-drop operation in Power BI. During this, I spotted that the Inpatient table includes a column called `Case_Type` that’s missing from Outpatient. So I added it:

- Navigated to **Add Column**
- Selected **Custom Column**
- Named it `Case_Type`
- Used the formula: `"Outpatient"`

Then I positioned this new column exactly where it appears in the Inpatient table to maintain structural consistency.

---

### 🔗 Appending Tables

With both tables now aligned, I proceeded to append them:

- Went to the **Home** tab
- Clicked **Append Queries > Append Queries as New**
- Selected *Inpatient* as the first table and *Outpatient* as the second
- Renamed the resulting table to `All_Data`
- Hit **Close & Apply** to finalize

---

### 🧹 Cleaning Up Redundant Data

In `All_Data`, I noticed inconsistencies in the `Age_Profile` and `Time_Band` columns—like `"18+ months"` vs `"18 month +"`. I used the **Replace Values** function to standardize these entries. Then, I applied the **Trim** function to remove any trailing spaces that could interfere with filtering or grouping.

---

### 🧩 Data Modeling

Time to jump into the **Model View** in Power BI. Since `All_Data` is now our master table, I hid the original Inpatient and Outpatient tables and disabled their load into the data model:

- Right-clicked each table in Power Query
- Unchecked **Enable Load**

Now, focusing on `Specialty_Name`, which is a key attribute in our analysis. The raw data contains too many unique specialties, which could clutter our visuals. So I imported a **Specialty Mapping** file (available in the resources) to group specialties into buckets.

Power BI usually auto-detects relationships, but just in case it doesn’t:

- I manually dragged `Specialty_Name` from `All_Data` onto the `Specialty` column in the Mapping table
- A directional arrow appeared, confirming the relationship and filter flow from Mapping to All_Data

---

**📊Step 4. Visualization Blueprint**

I’ve already prepared a dashboard blueprint. In real-world scenarios, I’d collaborate with the team to sketch out a wireframe, get stakeholder approval, and then move into development.


<img width="715" height="320" alt="image" src="https://github.com/user-attachments/assets/6e15dd4f-75cd-4bb4-aa52-095ed5a542a4" />
Absolutely! Here's how I’d describe the process as if I were the one building this dashboard from scratch:

---

**🎨Step 5. Dashboard Layout & Design**

Before diving into the visuals, I always head to the **View** tab and enable **Gridlines** and **Snap to Grid**. These help me align visuals neatly and maintain a clean layout throughout the canvas.

---

### 🧮 Creating Key Measures with DAX

To kick things off, I created two essential measures to track patient waitlist trends:

```DAX
Latest Month Wait List = CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date] = MAX(All_Data[Archive_Date])) + 0

PY Latest Month Wait List = CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date] = EDATE(MAX(All_Data[Archive_Date]), -12)) + 0
```

I then dropped these into **card visuals** to highlight the latest and previous year’s waitlist totals.

---

### 📋 Setting Up Calculation Method Toggle

To give users control over how they view the data (Average vs. Median), I manually created a small table with two rows: `Average` and `Median`. Using this, I built a **button slicer** so users can toggle between the two methods.

Next, I created the following measures:

```DAX
Median Wait List = MEDIAN(All_Data[Total])
Average Wait List = AVERAGE(All_Data[Total])

Avg/Med Wait List = SWITCH(VALUES('Calculation Method'[Calc Method]), 
    "Average", [Average Wait List], 
    "Median", [Median Wait List])

Dynamic Title = SWITCH(VALUES('Calculation Method'[Calc Method]), 
    "Average", "Key Indicators - Patient Wait List (Average)", 
    "Median", "Key Indicators - Patient Wait List (Median)")
```

To handle scenarios where data might be missing, I added:

```DAX
NoDataLeft = IF(ISBLANK(CALCULATE(SUM(All_Data[Total]), All_Data[Case_Type] <> "Outpatient")), "No data for selected criteria", "")
NoDataRight = IF(ISBLANK(CALCULATE(SUM(All_Data[Total]), All_Data[Case_Type] = "Outpatient")), "No data for selected criteria", "")
```

---

### 📊 Building the Summary Page

Following the blueprint, I added:

- A **doughnut chart**
- A **clustered column chart**
- A **multi-row card** showing the top five specialties

All of these use the `Avg/Med Wait List` measure for dynamic calculation.

For the **line chart** at the bottom, I used `Total` and `Archive_Date`, applying a visual filter on `Case_Type` to split the chart into **Day Case & Inpatients** vs. **Outpatients**.

I also added slicers for `Archive_Date`, `Case_Type`, and `Specialty` to give users flexible filtering options.

---

### 📁 Detailed View Page

I created a new page and added a **matrix visual** using:

- `Archive_Date`
- `Specialty_Name`
- `Age_Profile`
- `Time_Bands`
- `Case_Type`
- `Total`

This gives a granular view of the data.

---

### 🧠 Tooltip Page

I built a dedicated **tooltip page** with:

- A chart showing `Specialty` vs. `Total Wait List`
- A card displaying the overall waitlist total

Then I enabled **“Allow Use as Tooltip”** in the page settings and linked it to the line chart on the summary page via the **Tooltips** section in formatting.

--
**🧭Step 6: Adding Interactivity**

To make the dashboard more engaging, I added:

- **Navigation buttons**
- **Alt text for charts**
- **Hover tooltips** for additional context


**✅7. Testing & Sharing**

Before publishing, I ran a thorough **User Acceptance Testing (UAT)** session to catch any bugs or data issues. Once everything checked out, I prepared the dashboard for sharing.

I also reviewed **Row Level Security (RLS)** settings to ensure sensitive data is protected. (There’s a great walkthrough on my YouTube channel for setting this up.)



**🔄8. Routine Refresh & Maintenance**

And finally—🎉—the dashboard is complete! I scheduled regular data refreshes and documented maintenance steps to keep everything running smoothly.


