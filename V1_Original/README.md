## Exploratory Data Analysis ( EDA ) & Business Intelligence

This Repository contain the complete working process for **Task-2** , 
       Where I Performed Exploratory Data Analysis (EDA) , Executed SQL Business Queries , Conducted Multivariate Analysis , Built Static Dashboard Using **Walmart Retail Sales Dataset**.

The goal of this task was to explore the dataset deeply, identify meaningful business patterns, and translate them into insights for decision-making.
-----

# 🔧 **1️⃣ Data Loading & Preprocessing (Working Process)**

### ✔ Loaded Dataset  
- Imported `Walmart_Sales.csv` into Python  
- Used Pandas for initial data inspection  

### ✔ Date Conversion  
- Converted the `Date` column from **DD-MM-YYYY** using `pd.to_datetime()`  
- Extracted **Month**, **Year**, and **Week** for time-series analysis  

### ✔ Data Checks  
- Checked for missing values  
- Verified datatypes  
- Ensured numeric columns were correctly interpreted  
- Prepared the dataset for EDA & SQL processing  

---

# 📊 **2️⃣ Exploratory Data Analysis (EDA)**

### ✔ Univariate Analysis  
- Summary statistics  
- Distribution plots for Weekly Sales  
- Store count and basic patterns  

### ✔ Time-Based Analysis  
- Monthly sales trends  
- Weekly movement patterns  
- Seasonal peaks  

### ✔ Store-Level Performance  
- Identified top & bottom performing stores  
- Compared total sales across all 45 stores  

---

# 🔍 **3️⃣ Multivariate Analysis**

Using Seaborn & Matplotlib, I explored relationships between multiple variables:

### 🔥 Visuals Created  
- **Heatmap:** Correlations between Temperature, Fuel Price, CPI, Unemployment, and Weekly Sales  
- **Scatter Plots:**  
  - Temperature vs Weekly Sales  
  - Fuel Price vs Weekly Sales  
  - CPI vs Weekly Sales  
  - Unemployment vs Weekly Sales  
- **Boxplot:** Holiday vs Non-Holiday Sales  

### 🔥 Key Findings  
- Holiday weeks showed significantly higher median sales  
- Temperature influenced shopping behavior (moderate weather = more sales)  
- Fuel Price, CPI, and Unemployment had weaker correlations  
- Store performance had the strongest influence on weekly sales  

---

# 🧮 **4️⃣ SQL Business Questions (Executed Using SQLite in Python)**

I integrated SQL inside Python using SQLite and answered 7 key business questions:

### ✔ Queries Answered  
1. Total sales by store  
2. Highest monthly average sales  
3. Holiday vs Non-Holiday performance  
4. Top 10 revenue-generating weeks  
5. Fuel price impact on sales  
6. Yearly sales summary  
7. CPI impact on average weekly sales  

### ✔ Purpose  
Combining SQL + Python helped cross-verify insights and simulate real analytics workflow.

---

# 📈 **5️⃣ Static BI Dashboard Mockup**

Designed a dashboard containing:

### ⭐ KPI Cards  
- Total Sales  
- Avg Weekly Sales  
- Best Performing Store  
- Best Month  
- Holiday Week Revenue Increase  

### ⭐ Visuals  
- Weekly Sales Trend Line Chart  
- Total Sales by Store Bar Chart  
- Monthly Sales Bar Chart  
- Temperature vs Sales Heatmap   

---

# 🛠 **Tools & Technologies Used**
- **Python:** Pandas, NumPy, Seaborn, Matplotlib  
- **SQL:** SQLite (executed inside Jupyter Notebook)  
- **Jupyter Notebook**  
- **PowerPoint / Canva** (for dashboard mockup)  

---

# 📝 **6️⃣ Key Insights (Summary)**
- Store 20 consistently leads in overall sales  
- March shows the highest monthly average sales  
- Holiday weeks boost sales by a large margin  
- Moderate temperature correlates with higher sales  
- External economic factors impact sales less than store performance  
- Strong seasonal patterns exist in weekly sales  
