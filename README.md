# Velocity-Protect-Insurance-Car-Insurance-Claims-Analysis-Power-BI-
An interactive Power BI dashboard that analyses car insurance claims for Velocity Protect Insurance to uncover which customer, vehicle and location factors drive claim frequency and claim amount, so underwriting and risk teams can make better pricing and segmentation decisions. practiced as a part of a Great Learning Mentored Learning Session: *Building Advanced Visuals and Reports Using Power BI*.


## Business Context

Velocity Protect Insurance wants to understand the patterns behind car insurance claims in order to improve policy management, reduce loss ratios and improve customer satisfaction. As Lead Data Analyst, the task was to analyse a claims dataset and deliver an interactive dashboard for stakeholders.

## Objectives

- Analyse claims data across demographic and vehicle attributes
- Identify the factors that increase claim frequency and claim amount
- Provide actionable insights on potential risks for policy optimisation
- Detect anomalies/trends that could help reduce fraud or predict future claims
- Deliver an intuitive, interactive dashboard for data-driven decisions

---

## Repository Structure

```
├── Velocity_Protect_Insurance.pbix   # Power BI report
├── Dataset.xlsx                      # Source data (Data + Dictionary sheets)
├── README.md
└── images/
    ├── 1-dashboard-overview.jpg
    ├── 2-support-graphs.jpg
    ├── 3-data-tables.jpg
    ├── 4-key-influencers.jpg
    ├── 5-decomposition-tree.jpg
    ├── 6-performance-analyzer.jpg
    ├── 7-claim-amount-by-occupation-Q&A.jpg
    └── 8-mobile-layout.jpg
```

---

## Dataset

`Dataset.xlsx` contains **7,648 records and 21 columns** (sheet `Data`) plus a `Dictionary` sheet. Assumptions: the car owner and driver are the same person; all amounts are in US dollars.

| Column | Description |
|---|---|
| ID | Identification variable |
| KIDSDRIV | Teenage children who can drive |
| BIRTH | Driver's date of birth |
| HOMEKIDS | Number of children |
| YOJ | Years on job |
| INCOME | Income of the driver |
| PARENT1 | Single parent (Yes/No) |
| HOME_VAL | Value of the owner's house |
| MSTATUS | Marital status |
| GENDER | Gender of the driver |
| EDUCATION | Maximum education level |
| OCCUPATION | Occupation of the car owner |
| TRAVTIME | Average commute time |
| CAR_USE | Private / Commercial |
| BLUEBOOK | Worth of the car |
| CAR_TYPE | Type of car |
| OLDCLAIM | Total claims in past 5 years |
| CLM_FREQ | Number of claims in past 5 years |
| CLM_AMT | Currently claimed amount |
| CAR_AGE | Age of the car |
| URBANICITY | Where the car is primarily driven |

---

##  Approach

1. **Load & transform** – import `Dataset.xlsx`, fix data types, ensure consistency.
2. **Calculated columns / bins (DAX)** – see the full code below.
3. **Build the dashboard** – KPI cards, combo chart, pie, tree maps, tables, graphs, AI visuals.
4. **Navigation & interactivity** – page buttons, bookmarks (Tables ⇄ Graphs toggle), slicers.
5. **Optimise** – Performance Analyzer and a dedicated mobile layout.

---

## DAX Calculated Columns

### Customer Age
```DAX
CUST_AGE = DATEDIFF('Data'[BIRTH], TODAY(), YEAR)
```

### Age Bins
```DAX
Age_Bin =
SWITCH(
    TRUE(),
    'Data'[Cust_age] < 50, "<50",
    'Data'[Cust_age] < 60, "50-59",
    'Data'[Cust_age] < 70, "60-69",
    'Data'[Cust_age] < 80, "70-79",
    'Data'[Cust_age] < 90, "80-89",
    'Data'[Cust_age] >= 90, "90+",
    "NA"
)
```

### Income Bins
```DAX
Income_Bin =
SWITCH(
    TRUE(),
    'Data'[INCOME] < 10000,  "<10k",
    'Data'[INCOME] < 20000,  "10k-20k",
    'Data'[INCOME] < 30000,  "20k-30k",
    'Data'[INCOME] < 40000,  "30k-40k",
    'Data'[INCOME] < 50000,  "40k-50k",
    'Data'[INCOME] < 60000,  "50k-60k",
    'Data'[INCOME] < 70000,  "60k-70k",
    'Data'[INCOME] < 80000,  "70k-80k",
    'Data'[INCOME] < 90000,  "80k-90k",
    'Data'[INCOME] < 100000, "90k-100k",
    'Data'[INCOME] < 150000, "100k-150k",
    'Data'[INCOME] > 150000, "150k+",
    "NA"
)
```

> `CUST_AGE` is calculated with `TODAY()`, so customer ages (and the age bins) change with the report refresh date.

---

## Dashboard Walkthrough

### 1. Dashboard Overview (Page 1)
images/1-dashboard-overview.jpg

KPI cards (total claim frequency, average car age, average claim frequency, car types, average income, average customer age), a combo chart of total vs. average claim amount by age bin, claim distribution by car use, and tree maps by marital status, urbanicity and gender.

### 2. Supporting Graphs (Page 2 – Graphs view)
images/2-support-graphs.jpg

Percentage of claims by car type & car use, education & single-parent status, occupation & urbanicity, and income & gender. Slicers for `CAR_USE` and `PARENT1`.

**Bookmarks and buttons used:** two buttons, **Tables** and **Graphs**, sit at the top of the page. Each is linked to a bookmark that captures a different view of the same page, so one click toggles between the graph visuals and the table visuals without leaving the page.

### 3. Data Tables (Page 2 – Tables view)
images/3-data-tables.jpg

The same breakdowns as tables, shown when the **Tables** button (bookmark) is clicked. The **Graphs** button switches back.

### 4. Key Influencers (Page 3)
images/4-key-influencers.jpg

Shows what drives `CLM_AMT` – e.g. customers with `CLM_FREQ` above 0 and those in highly urban areas have a higher average claim amount (Top segment: highly urban, avg ≈ 1.78K vs. 1.48K overall).

### 5. Decomposition Tree (Page 4)
images/5-decomposition-tree.jpg

Drill-down of total claim amount by Income Bin → Car Type → Gender → Occupation.

### 6. Performance Analyzer
images/6-performance-analyzer.jpg

Used to measure visual load times and optimise the report.

### 7. Claim Amount by Occupation – Q&A Graph (Page 5)
images/7-claim-amount-by-occupation-Q&A.jpg

A graph created with the **Q&A feature**, which allows natural-language questions on the data. Typing a question such as "sum of claim amount by occupation" produced this bar chart. Blue Collar occupations account for the largest share of total claim amount, followed by Clerical and Professional.

### 8. Mobile Layout
images/8-mobile-layout.jpg

A phone-optimised version of the report.

---

## Key Insights

- **Private cars ≈ 66.5%** of policies vs. ≈ 33.5% commercial.
- Total claim amount is **highest for the 60–69 age bin**, while average claim amount per customer is highest in the youngest bin.
- **SUVs, minivans and sports cars** account for the largest share of claim frequency among private vehicles.
- **Blue Collar and Clerical** occupations contribute the most to claim frequency and amount.
- **Highly urban** drivers show a higher average claim amount than the overall average.
- Prior claim history (`CLM_FREQ > 0`) is the strongest influencer of a higher claim amount.

> Review these against the final report before publishing, as figures depend on filters and the refresh date (`CUST_AGE` uses `TODAY()`).

---

## Tools & Features Used

Power BI Desktop · DAX · Power Query · Key Influencers · Decomposition Tree · Q&A · Bookmarks & Buttons · Slicers · Performance Analyzer · Mobile Layout

## How to Use

1. Download or clone this repository.
2. Open `Velocity_Protect_Insurance.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
3. If prompted, point the data source to the local `Dataset.xlsx` (Home → Transform data → Data source settings).
4. Use the page tabs, slicers and Tables/Graphs buttons to explore.

## Author

**Muthulakshmi Viswanathan** – Data Analyst, Toronto, Canada


