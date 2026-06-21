
# ** SWIFT ROOT LOGISTICS DASHBOARD (REAL-TIME POWER BI PROJECT)**

## **1. Business Problem Understanding**

Swift Root Logistics is a **real-time logistics company** in the US dealing with **movement of goods from one place to another place** with right time, right person, right route, and right cost. They analyzed **28,000 delivery records** across **2 years from 2023 to 2024** and faced **5 critical business challenges**:

* **19% delivery delays** against an industry target of less than 15%, which impacts customer trust and SLA compliance.
* **Hub overload** where Dallas Main Hub operates at **110% capacity** versus optimal 85%, causing bottlenecks and driver burnout.
* **Driver performance variation** with top driver Jessica Lee at **92% on-time rate** and worst driver Trevor Smith at **68% on-time rate** with 24 delays, creating inconsistent service quality.
* **Vehicle breakdowns** where Freliner M2 has **153 breakdowns** costing **5 million rupees annually** in maintenance costs.
* **Report latency** where stakeholders wait **36-72 hours** for updated data instead of getting real-time insights.

These issues result in **15-20% revenue loss** from customer churn and refund claims. The business goals were to optimize hub capacity utilization, manage driver performance, reduce delivery delays, track vehicle breakdowns, and provide real-time analytics dashboards with 4 fully functional dashboards.

---

## **2. Project Architecture & Data Modeling**

I designed a **5-table Star Schema architecture** following industry best practices for Power BI:

* The **fact table** is Orders with **28,000 rows** and **15 columns** containing transactional data like order ID (primary key), order date, actual delivery date, delay reason, is delayed flag, customer satisfaction score, and delivery time hours. The **primary key** is order ID which is unique.
* The **dimension tables** include:
* **Hubs** with 6 hubs containing hub name (primary key) and hub capacity
* **Drivers** with 55-56 drivers containing driver ID (primary key), driver name, employment type, hire date, experience years, and driver rating
* **Vehicles** with approximately 50 vehicles containing vehicle code (primary key), vehicle name, vehicle type, vehicle model, and status
* **Date table** with 730 days for time intelligence covering 2 years and 12 months



I established relationships where order ID links to driver ID, hub name, and vehicle name with **4 foreign key links**. The **cardinality** is many-to-one meaning multiple orders per driver but unique driver in dimension. The **cross-filter direction** is single meaning dimension filters fact but not bidirectional to avoid performance issues.

I chose Star Schema because the fact table has 28,000 rows surrounded by dimensions which are small tables resulting in **optimal query performance**, **simple DAX**, and **fast filtering**. Instead of merging queries which would create $28,000 \times 30 = 840,000$ data points, I used relationships to keep data lightweight.

### **Data Model Diagram**
<img width="1167" height="733" alt="Logistics_Data_model" src="https://github.com/user-attachments/assets/cde9c10d-5a8e-4d27-bb22-06e69935c58e" />

---

## **3. Data Sources & ETL Pipeline**

The data came from **4 CSV files** requiring consolidation:

* The **primary data source** was CSV files containing **Orders table with 28,000 delivery records**, **Hubs table with 6 hubs**, **Drivers table with 55 drivers**, and **Vehicles table with 50 vehicles**.

I performed **data cleaning and transformation in Power Query**:

* Removed top 2-8 rows containing headers mixed with data.
* Used first row as header for all tables.
* Removed duplicate order IDs in Orders table.
* Selected required columns: order ID, driver ID, hub name, vehicle name, order date, actual delivery date, delay reason, is delayed flag, customer satisfaction score, delivery time hours.
* **Key transformation**: Created **delay reason groups** using IF logic where Is Delayed = TRUE provides delay reason, Is Delayed = FALSE has blank (intentional for on-time orders).

The **ETL pipeline** runs smoothly: CSV ingestion $\rightarrow$ Power Query transformation $\rightarrow$ Data validation (duplicates/nulls check) $\rightarrow$ Data loading to Power BI dataset. The entire pipeline runs in **5 seconds** demonstrating efficient ETL processing.

---

## **4. Power BI Dashboard Design**

I designed **4 interactive dashboards** with comprehensive operational coverage:

### **Page 1: Overview Dashboard (Landing Page)**

* **5 KPI cards**: Total Orders (12,000/month, 4.3% MoM growth), On-Time Rate (80.88%, -1% MoM vs 85% target), CSAT (84%, stable), Avg Delivery Time (18.5 hours, +4% MoM vs 15-hour target), Total Hubs (6).
* **3 sections**: Hubs (order vs capacity bar chart, hub performance ranking), Drivers (experience vs ratings scatter plot, top 5 drivers with delays with tooltip), Vehicles (active/inactive pie chart, orders by vehicle model bar chart).
* **Interactive features**: Year slicer (2023/2024), Month slicer (Jan-Dec), dynamic tooltips showing 7 delay reasons.
<img width="1343" height="748" alt="Logistics_Overview(page1)" src="https://github.com/user-attachments/assets/48fa5a59-e390-4f08-8df2-8cd70d664300" />


---

### **Page 2: Driver Overview Dashboard**

* **5 Key features**: Top 10 delayed drivers table, Experience vs ratings scatter plot, Dynamic driver profile (select driver $\rightarrow$ shows hire date, experience, deliveries, star rating dynamically), Monthly trend chart (orders/month and on-time rate for selected driver), Parameter selector (toggle between total orders and on-time rate).
<img width="1261" height="733" alt="Drivers Overview(Page2)" src="https://github.com/user-attachments/assets/520de23f-caea-48a6-8cf9-170803166b3d" />

---

### **Page 3: Hub Overview Dashboard**

* **4 Core charts**: Order processed vs capacity bar chart, Hub performance ranking by on-time rate, Daily processing time heatmap (Monday-Sunday across 6 hubs with red=slow 24hrs, green=fast 12hrs), Hub capacity utilization analysis.
<img width="1253" height="734" alt="Hubs_Overview(Page3)" src="https://github.com/user-attachments/assets/961060c1-233e-4460-ba3b-99c2524e7666" />

---

### **Page 4: Vehicle Overview Dashboard**

* **5 Analytics charts**: Active/inactive/maintenance vehicles pie chart, Total orders by vehicle model bar chart, Age vs breakdown scatter plot, Breakdowns by vehicle code table, Breakdowns by vehicle model table (Freliner M2 = 153 breakdowns highest).
<img width="1260" height="733" alt="Vehicles_Overview(Page4)" src="https://github.com/user-attachments/assets/33f705d4-5c13-4ba4-9eff-a959ef22a070" />

---

## **5. KPIs & DAX Measures**

I created **advanced DAX calculations** for all KPIs using robust time intelligence logic:

* **Total Orders with MoM Growth**:

```dax
Total Orders = COUNTROWS(Orders)

MoM Growth = 
DIVIDE(
    COUNTROWS(Orders) - CALCULATE(COUNTROWS(Orders), PREVIOUSMONTH(Date[Date])), 
    CALCULATE(COUNTROWS(Orders), PREVIOUSMONTH(Date[Date])), 
    0
)

```

* **On-Time Delivery Rate**:

```dax
On-Time Rate = 
DIVIDE(
    COUNTROWS(FILTER(Orders, Orders[Is On Time] = TRUE)), 
    COUNTROWS(Orders), 
    0
) * 100

```

* **Customer Satisfaction Score (CSAT)**:

```dax
CSAT = AVERAGE(Orders[Customer Satisfaction Score])

```

* **Average Delivery Time**:

```dax
Avg Delivery Time = AVERAGE(Orders[Delivery Time Hours])

```

* **Hub Capacity Utilization**:

```dax
Capacity Utilization = 
DIVIDE(
    COUNTROWS(Orders), 
    SUM(Hubs[Hub Capacity]), 
    0
) * 100

```

* **Driver Performance Ranking**:

```dax
Driver Rank = RANKX(ALL(Drivers[Driver Name]), [On-Time Rate], DESC, DENSE)

```

* **Vehicle Breakdown Analysis**:

```dax
Breakdowns = COUNTROWS(FILTER(Orders, Orders[Delay Reason] = "Vehicle Breakdown"))

```

## **6 Business Insights & Recommendations**

1. **On-Time Rate Deficit**: The overall rate stands at **80.88%** (a 4.12% gap vs the 85% target threshold). **Action:** Optimize regional routing frameworks and cut down operational queue bottlenecks.
2. **Hub Redirection**: Hub B is unsustainably overloaded at **110% volume capacity**. **Action:** Implement balancing logic to re-route incoming load distributions systematically down to under-capacity hubs (e.g., Target Hub B down to 95%, Hub A up to 100%).
3. **Hub Lag Mitigation**: Hub C shows a slow turnaround metric at **22 hours** (8 hours slower than Hub A). **Action:** Augment sorting shift crews at Hub C to stabilize operations at a 15-hour target ceiling.
4. **Driver Mentorship**: Jessica Lee represents peak execution with a **92% metric**, whereas Trevor Smith registers a weak performance at **68%** with 24 individual lag instances. **Action:** Deploy targeted performance improvement programs using peer coaching models.
5. **Fleet Modernization**: The Freightliner M2 sub-fleet recorded **153 unique breakdown indicators** driving up maintaining costs to 5M rupees. **Action:** Transition the model out of commission immediately to mitigate annual financial overhead. Focus expansion on the **Van asset variant** which handled **8,500 highly efficient operations**.

## **Author**
Kosireddy Akash | Data Analyst | Python • SQL • Power BI Analytics
