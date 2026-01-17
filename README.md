
---

# SuperStore Sales Dashboard – End-to-End Process & Analysis

---

## 1. Business Objective

The objective of this dashboard is to provide a **comprehensive performance overview** of the SuperStore business by analyzing:

* Overall **Sales, Profit, and Return Percentage**
* **Year-over-Year (YoY)** comparison with Previous Year (PY)
* Performance by **Region, State, Category, Sub-Category, and Product**
* Identification of:

  * High-profit and loss-making products
  * Regions and categories with high return rates
  * Monthly sales trends and seasonality

This dashboard supports **data-driven decision making** for sales, operations, and supply-chain optimization.

---

## 2. Data Understanding

### Fact Table: Orders

Key columns used:

* Sales, Profit, Quantity, Discount
* Order Date, Ship Date
* Category, Sub-Category, Product Name
* Region, State/Province, City
* Ship Mode
* Order ID, Customer ID

### Dimension Table: Date Table

Key columns:

* Date
* Year
* Month
* Start of Month

A dedicated Date table is required to enable **time-intelligence calculations**.

---

## 3. Data Preparation (Power Query)

Steps performed in Power Query Editor:

1. Removed null values from critical fields such as Sales, Profit, and Order Date
2. Removed error rows from numeric columns
3. Assigned correct data types:

   * Sales, Profit → Decimal
   * Quantity → Whole number
   * Date fields → Date
4. Created derived columns:

   * Year
   * Month
   * Start of Month
5. Loaded clean data into the data model

---

## 4. Data Modeling

* Relationship created:

  * Date Table[Date] → Orders[Order Date] (One-to-Many)
* Date table marked as **Date Table**
* Star schema followed for better performance and scalability

---

## 5. Key Measures (DAX)

### Base Measures

```DAX
Sales = SUM(Orders[Sales])

Profit = SUM(Orders[Profit])

Returned Orders =
DISTINCTCOUNT(Orders[Order ID])
```

---

### Return Percentage

```DAX
% Returned Orders =
DIVIDE(
    [Returned Orders],
    DISTINCTCOUNT(Orders[Order ID])
)
```

---

### Previous Year (PY) Measures

```DAX
Sales PY =
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR('Date Table'[Date])
)
```

```DAX
Profit PY =
CALCULATE(
    [Profit],
    SAMEPERIODLASTYEAR('Date Table'[Date])
)
```

```DAX
Returned Orders PY =
CALCULATE(
    [Returned Orders],
    SAMEPERIODLASTYEAR('Date Table'[Date])
)
```

---

### Year-over-Year (Vs PY) Measures

```DAX
Vs PY - Sales =
DIVIDE(
    [Sales] - [Sales PY],
    [Sales PY]
)
```

```DAX
Vs PY - Profit =
DIVIDE(
    [Profit] - [Profit PY],
    [Profit PY]
)
```

```DAX
Vs PY - % Returned Orders =
DIVIDE(
    [% Returned Orders] - [% Returned Orders PY],
    [% Returned Orders PY]
)
```

---

## 6. Visuals and Their Purpose

### KPI Cards

* Total Sales
* Total Profit
* Return Percentage
* Previous Year values and YoY change

**Purpose:**
Provides an instant high-level snapshot of business performance.

---

### Sales vs Previous Year Over Time (Line/Area Chart)

* X-Axis: Start of Month
* Values: Sales and Sales PY

**Purpose:**
Tracks monthly trends, growth patterns, and seasonality while comparing current year performance with the previous year.

---

### Profit by State (Map Visual)

* Location: State/Province
* Size: Profit

**Purpose:**
Identifies high-profit and loss-making states geographically.

---

### Return Percentage by Region

* Regions: South, Central, East, West

**Purpose:**
Highlights operational or logistical issues by identifying regions with high return rates.

---

### Sales by Ship Mode (Donut Chart)

* Standard Class
* Second Class
* First Class
* Same Day

**Purpose:**
Analyzes customer preference for shipping methods.

---

### Return Percentage by Category

* Technology
* Furniture
* Office Supplies

**Purpose:**
Identifies categories with higher return risk.

---

### Profit by Product / Sub-Category (Column Chart)

* X-Axis: Sub-Category
* Y-Axis: Profit

**Purpose:**
Identifies high-performing and loss-making product segments.

---

### Profit by Category and Sub-Category (Treemap)

**Purpose:**
Shows hierarchical contribution of sub-categories within each category.

---

## 7. Interactivity and Filters

* Date slicer using Start of Month
* Cross-filtering enabled across all visuals
* Dynamic updates based on user selections

---

## 8. Dashboard Analysis & Insights

### Sales Performance

* Strong overall sales growth
* Approximately **47% YoY increase**, indicating business expansion

### Profitability

* Overall profit is positive
* Some sub-categories (e.g., Tables, Bookcases) generate losses and require cost or pricing review

### Returns Analysis

* Return percentage has reduced compared to the previous year, which is a positive sign
* South region and Technology category show relatively higher return rates

### Shipping Insights

* Standard Class dominates total sales
* Same-Day shipping has minimal contribution, suggesting limited demand or higher cost

---

## 9. Conclusion

This SuperStore Sales Dashboard delivers a **holistic and interactive analytical view** of sales, profit, and return performance. By leveraging **Power BI data modeling, DAX time-intelligence functions, and interactive visuals**, the dashboard enables stakeholders to monitor trends, identify risk areas, and make informed strategic decisions.

---
