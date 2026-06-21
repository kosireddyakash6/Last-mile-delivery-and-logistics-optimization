Here is your formatted and structurally optimized **Customer Churn Analysis** content rewritten to match the exact elite engineering depth, professional structure, and formatting style of your logistics project.

---

# **PROJECT: TELECOM CUSTOMER CHURN ANALYTICS (END-TO-END POWER BI PROJECT)**

## **1. Business Problem Understanding**

A leading telecommunications provider analyzed historical customer behavior records covering **7,043 subscribers** to diagnose and combat high customer attrition rates. The business was struggling with an overall **churn rate of 26.54%** (accounting for **1,869 lost customers**), directly threatening market share and reducing customer lifetime value (LTV).

The discovery phase identified **4 critical business friction points**:

* **Contractual Instability:** High concentrations of churn are linked directly to short-term, month-to-month contracts where customer commitment is highly volatile.
* **Service Friction vs. Premium Pricing:** Fiber optic internet users exhibit disproportionately frequent churn compared to DSL users, indicating a severe gap between premium billing charges and actual service satisfaction.
* **Transactional Dropouts:** Customers utilizing manual payment methods, specifically **Electronic Checks**, present significantly higher churn velocities compared to automated streams.
* **Onboarding Retainment Lag:** A severe vulnerability window exists during the early customer lifecycle, with the highest churn risk concentrated in subscribers with a **tenure of less than 1 year**.

These attrition dynamics combined to yield an estimated **15–20% structural revenue loss** annually. The explicit objective of this project was to design a multi-page analytics framework to identify at-risk subscribers, uncover behavioral churn drivers, and deliver programmatic, data-driven customer retention mechanics.

---

## **2. Project Architecture & Data Modeling**

I architected a centralized, high-performance data model deploying a clean **Star Schema** designed to optimize dimensional filtering and analytical query response times in Power BI:

* **Fact Table:** `Fact_Churn_Data` containing **7,043 rows** of customer account profiles and service configurations. Key granular elements include `CustomerID` (Primary Key), `Tenure_Months`, `Monthly_Charges`, `Total_Charges`, and the analytical target flag `Churn_Status`.
* **Dimension Tables:** Outlying customer attribute matrices extracted and isolated to separate dim tables to maximize relational efficiency:
* `Dim_Contracts`: Contains unique contract types (Month-to-Month, One Year, Two Year).
* `Dim_Services`: Contains product offerings (Fiber Optic, DSL, Online Security, Tech Support).
* `Dim_Payments`: Maps transaction methods (Electronic Check, Bank Transfer, Credit Card).
* `Dim_Demographics`: Tracks structural cohort segmentation (Gender, Senior Citizen, Dependents).



Relationships were mapped using **Many-to-One (`*:1`) Cardinality** pointing from the central Fact table out to the respective analytical dimensions. The **cross-filter direction was strictly configured as Single** to maintain unidirectional filtering, preventing downstream query ambiguity and eliminating calculation latency across the dataset.

### **Data Model Diagram**

---<img width="1167" height="733" alt="Logistics_Data_model" src="https://github.com/user-attachments/assets/b57e8c11-b738-45ab-9214-af9b7892b65f" />






## **3. Data Sources & ETL Pipeline**

The core pipeline ingested raw data from disparate operational environments, utilizing **Python** and **Power Query (M Engine)** for heavy extraction, transformation, and structural loading:

* **Ingestion Profile:** Consolidated localized database dumps containing information on customer services, demographic structures, and monthly billing cycles.
* **Data Cleansing Logic (Python & Power Query):**
* Handled structural nulls and type mismatched features (e.g., forcing `Total_Charges` from text to a clean fixed decimal number).
* Cleansed duplicate entry profiles and filtered out top-row artifact headers.
* Extracted conditional buckets to categorize tenure horizons into actionable operational groups (e.g., `Tenure < 1 Year`).
* **Key Transformation:** Constructed programmatic logic where non-internet subscribers tracking ancillary services (like Online Security) were cleanly normalized to avoid bias flags inside advanced DAX arrays.



The optimized ETL pipeline systematically processes the source tables into data structures in under **3 seconds**, creating a clean, production-ready dataset.

---

## **4. Power BI Dashboard Design**

I engineered a responsive dashboard application organized across distinct strategic domains to give executives immediate operational clarity:

### **Page 1: Executive Overview Dashboard (Landing Page)**

* **4 High-Impact KPI Cards:** Total Customers (7,043), Churned Population (1,869), Avg Monthly Charges ($64.76), and Overall Churn Rate (**26.54%**).
* **Core Layout Visuals:** A macro breakdown tracking contract type distributions against explicit churn rates, along with billing tier distributions utilizing interactive scatter plots.
* **Interactive Tooling:** Incorporated active year/tenure slicers alongside dynamic tooltips showcasing demographic risk distributions directly on hover.

---

### **Page 2: Service & Product Performance Analytics**

* **Strategic Visualizations:** A comparative performance matrix isolating customer churn rates across Internet Service types (DSL vs. Fiber Optic), integrated with an adjacent analysis tracking ancillary feature adoptions (Tech Support, Online Security). Includes conditional color mapping alerting teams to low-satisfaction product groupings.

---

### **Page 3: Billing & Financial Cohort Analysis**

* **Operational Focus:** A dedicated payment matrix tracking churn velocity across payment channels (Electronic Check, Paper Check, Auto-Pay). Features an integrated line chart modeling the correlation between climbing `Monthly_Charges` thresholds and actual customer cancellation trends.

---

## **5. KPIs & DAX Measures**

I authored advanced, optimized DAX calculations to extract real-time KPI metrics and execute deep cohort evaluations across the telecom subscriber population:

* **Total Subscriber Ingestion Count:**

```dax
Total Customers = COUNTROWS(Fact_Churn_Data)

```

* **Absolute Attrition Volume:**

```dax
Churned Customers = CALCULATE(COUNTROWS(Fact_Churn_Data), Fact_Churn_Data[Churn_Status] = "Yes")

```

* **Enterprise Churn Rate Percentage:**

```dax
Churn Rate = 
DIVIDE(
    [Churned Customers], 
    [Total Customers], 
    0
) * 100

```

* **Average Monthly Billing Footprint:**

```dax
Avg Monthly Charges = AVERAGE(Fact_Churn_Data[Monthly_Charges])

```

* **Cohort Financial Penetration (High-Value Attrition):**

```dax
High Value Churn Risk = 
CALCULATE(
    [Churned Customers], 
    FILTER(Fact_Churn_Data, Fact_Churn_Data[Monthly_Charges] > 80)
)

```

---

## **6. Technical Challenges & Optimization**

### **Technical Challenges Overcome**

1. **Handling Incomplete Value Strings:** Mixed data types within the `Total_Charges` attribute initially caused mathematical calculation faults. Resolved by engineering custom Power Query conditional mapping layers to cleanse blank spaces and normalize values to floating decimals.
2. **Dynamically Isolating High-Risk Customer Profiles:** Generating real-time risk labels that adapt to active slicers was solved by deploying custom `SELECTEDVALUE` parameters inside DAX conditional statement strings.
3. **Optimizing Volatile Feature Filters:** Managing complex, cross-cutting calculations for independent services (e.g., assessing the combined risk of users with Fiber Optic *but no* Tech Support) was achieved by standardizing filter evaluation contexts via advanced `KEEPFILTERS` matrices.

### **Performance Optimization Strategies**

* **Replaced Merges with Relational Connections:** Abandoned flat table merges in favor of an optimized Star Schema model, avoiding a bloated data footprint and maintaining fast query speeds.
* **Enforced Strict Unidirectional Filtering:** Restricted cross-filtering to a single direction to keep performance fast and prevent circular reference traps.
* **Deployed Safe Arithmetic Formatting:** Utilized the native `DIVIDE` calculation function across all mathematical expressions to handle null parameters safely without throwing dashboard exceptions.

---

## **7. Business Insights & Recommendations**

1. **Mitigate Month-to-Month Exposure:** Subscriptions on month-to-month terms are highly unstable and drive the bulk of structural churn. **Action:** Launch proactive contract migration programs, offering targeted billing discounts or loyalty credits to incentivize transitions into stable 1-year or 2-year commitments.
2. **Address Fiber Optic Service Friction:** Premium Fiber Optic users are leaving at a disproportionately high rate. **Action:** Form an engineering task force to audit fiber line delivery frameworks, fix regional latency issues, and couple high-value tiers with complimentary premium tech support channels.
3. **Automate Transactional Workflows:** The manual Electronic Check payment option correlates strongly with elevated customer churn. **Action:** Incentivize the adoption of automated digital payment channels (Auto-Pay, Credit Card) by providing a one-time invoice credit, successfully reducing passive churn.
4. **Target the First-Year Lifecycle:** Churn risk peaks sharply within a customer's first 12 months. **Action:** Deploy automated post-onboarding engagement workflows, checking in with new users at critical milestones (Months 1, 3, and 6) to resolve setup friction before it leads to cancellation.
