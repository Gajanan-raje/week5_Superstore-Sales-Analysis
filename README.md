# Week 5: Power BI & Business Intelligence

### Superstore Sales Dashboard | InternNova Internship

**Name:** Gajanan Raje  
**Course:** TYBCA, MGM's College of Computer Science & IT, Nanded  
**Tool:** Microsoft Power BI Desktop  
**Dataset:** Sample Superstore (9,994 rows)

---

## 1. Project Overview

This project turns raw retail sales data into a clean data model and an interactive Power BI dashboard. The data is imported and cleaned in Power Query, modeled as a star schema, analysed with DAX measures, and presented in a dark-themed dashboard with KPI cards, charts and slicers. The goal is to help a business understand **what sells, who buys, where profit comes from, and where money is lost.**

---

## 2. Dashboard Preview

![Dashboard](18_dashboard.png)

### Filtered view (Year = 2017, Region = West)

![Filtered Dashboard](19_filtered.png)

All cards and visuals update together when a slicer value is selected.

---

## 3. Dataset

The Superstore data was split into three CSV files to build a proper star schema.

| Table | Type | Main columns |
|---|---|---|
| **Orders** | Fact | Order ID, Order Date, Ship Date, Ship Mode, Customer ID, Region, City, State, Product ID, Sales, Quantity, Discount, Profit |
| **Customers** | Dimension | Customer ID, Customer Name, Segment |
| **Products** | Dimension | Product ID, Category, Sub-Category, Product Name |
| **Calendar** | Dimension (DAX) | Date, Year, MonthNo, Month |

---

## 4. Work Done (Task by Task)

### Task 1: Introduction & Data Import
- Power BI Desktop was used to import `Orders_fixed.csv`, `Customers.csv` and `Products.csv` (Home > Get data > Text/CSV).
- The files were opened in Power Query using **Transform Data**.

**Screenshots:**

![Orders](1_orders.png)
![Customers](2_customers.png)
![Products](3_products.png)


### Task 2: Data Cleaning (Power Query)
- Used **Use First Row as Headers** on Customers and Products.
- Fixed data types (dates, decimals, whole numbers, text).
- Fixed the date locale problem: dates in M/D/YYYY were read wrongly, so the dates were loaded in ISO format (YYYY-MM-DD).
- Checked **Column quality / Column profile** for errors, empty values and duplicates.
- Removed duplicates and unnecessary columns.
- Added a custom column: `ShipDays = Duration.Days([Ship Date] - [Order Date])`.
- Clicked **Close & Apply**.

**Screenshots:**

![Column quality](4_quality.png)
![Applied steps](5_steps.png)
![Table view](8_table_view.png)


### Task 3: Data Modeling
Star schema with Orders as the fact table. All relationships are **Many-to-one** with **single direction** filtering.

| From (Many) | To (One) |
|---|---|
| Orders[Customer ID] | Customers[Customer ID] |
| Orders[Product ID] | Products[Product ID] |
| Orders[Order Date] | Calendar[Date] |

**Screenshots:**

![Data model](9_model.png)


### Task 4: DAX Measures & Calculated Columns

```DAX
Calendar = ADDCOLUMNS(
    CALENDAR(DATE(2014,1,1), DATE(2017,12,31)),
    "Year", YEAR([Date]),
    "MonthNo", MONTH([Date]),
    "Month", FORMAT([Date], "MMM"))

Total Sales    = SUM(Orders[Sales])
Total Profit   = SUM(Orders[Profit])
Profit %       = DIVIDE([Total Profit], [Total Sales])
Average Sales  = AVERAGE(Orders[Sales])
Total Records  = COUNTROWS(Orders)
Total Orders   = DISTINCTCOUNT(Orders[OrderID])
Max Sale       = MAX(Orders[Sales])
Min Sale       = MIN(Orders[Sales])

Profit Status  = IF(Orders[Profit] < 0, "Loss", "Profit")   // calculated column
```

| Measure | Result |
|---|---|
| Total Sales | 2.30M |
| Total Profit | 286.40K |
| Profit % | 12.47% |
| Average Sales | 229.86 |
| Total Records | 9,994 |
| Total Orders | 5,009 |
| Max Sale | 22,638.48 |
| Min Sale | 0.44 |

**Screenshots:**

![Measures](10_measure.png)
![KPI cards](11_cards.png)


### Task 5: Visualizations
| Visual | Fields | What it shows |
|---|---|---|
| Column chart | Category, Total Sales | Sales by product category |
| Line chart | Month, Total Sales | Monthly sales trend |
| Donut chart | Segment, Total Sales | Share of each customer segment |
| Bar chart | Sub-Category, Total Profit | Profit by sub-category |
| Table | Product Name, Total Sales, Total Profit | Top products by sales |

**Screenshots:**

![Column chart](13_column.png)
![Line chart](14_line.png)
![Donut chart](15_donut.png)
![Bar chart](17_bar.png)
![Table](16_table.png)


### Task 6: Interactive Dashboard
- **4 KPI cards:** Total Sales, Total Profit, Profit %, Total Orders.
- **5 visuals:** Sales by Category, Monthly Sales Trend, Sales by Customer Segment, Profit by Sub-Category, Top Products table.
- **2 slicers:** Year and Region.
- Dark theme with a consistent palette (teal, blue, orange) for easy reading.
- Cross-filtering: clicking a bar or donut slice filters the other visuals.

### Task 7: Insights & Recommendations

**Key insights**
1. **Consumer** is the largest segment with about **50.56%** of sales (1.16M). Corporate has 30.74% and Home Office 18.7%.
2. **Technology** is the top category by sales. Furniture and Office Supplies are close to each other.
3. **Copiers** give the highest profit. The **Canon imageCLASS 2200 Advanced Copier** is the top product (Sales 61,599.82, Profit 25,199.93).
4. Some high-selling products make a **loss**, for example Cisco TelePresence System EX90 (-1,811.08) and GBC DocuBind P400 (-1,878.17).
5. The overall **profit margin is 12.47%**, i.e. about 12 rupees profit on every 100 rupees of sales.
6. Sales rise in the last quarter of the year (September to December), which points to **seasonal demand**.

**Recommendations**
1. Reduce discounts or revise the price of loss-making products such as Cisco TelePresence and GBC DocuBind P400.
2. Give more focus (stock and promotion) to Technology and Copiers, because they bring the most profit.
3. Prepare stock and offers before September to December to use the seasonal demand.
4. Launch special offers for Corporate and Home Office customers to grow their share.

---

## 5. Repository Contents

| File / Folder | Description |
|---|---|
| `Week5_PowerBI_Gajanan.pdf` | Full report with all tasks and screenshots (Task 1 to 7) |
| `Week5_PowerBI.pbix` | Power BI project file (open with Power BI Desktop) |
| `Data/` | Dataset files: `Orders_fixed.csv`, `Customers.csv`, `Products.csv` |
| `1_orders.png` to `19_filtered.png` | Step-by-step screenshots of the work |
| `README.md` | This file |

---

## 6. How to Open the Project

1. Install **Power BI Desktop** (free, Windows).
2. Download `Week5_PowerBI.pbix` from this repository.
3. Open it in Power BI Desktop. If asked, update the data source path to the files in the `Data/` folder (Home > Transform data > Data source settings).
4. Use the **Year** and **Region** slicers to explore the dashboard.

---

## 7. Conclusion

Power BI helped to convert raw sales data into a clean model and an interactive dashboard. Sales, profit, customers and products can now be seen in one place, which supports better business decisions.
