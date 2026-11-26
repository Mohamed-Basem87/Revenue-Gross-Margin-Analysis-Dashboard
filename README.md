# Revenue & Gross Margin Analysis Dashboard

**Transforming raw supplier and sales data into a structured BI model that analyzes $9.68M in operational revenue, $4.02M in gross margin and profitability performance across 5 suppliers and 3 sales teams.**

![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?logo=powerbi&logoColor=black) ![DAX](https://img.shields.io/badge/Language-DAX-7B7FDC) ![Status](https://img.shields.io/badge/Status-Complete-3FB58F) ![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## Project Overview

This project builds a star-schema Power BI model over product, supplier, salesperson and transactional revenue data. It cleans and reshapes the raw Excel sources in **Power Query**, enriches the product dimension with supplier logos, generates a calendar table in **DAX**, and layers revenue, cost and margin measures on top.

The report quantifies revenue and gross margin by month, supplier, product category/group and sales team, then highlights the best performing months through TopN-filtered KPI cards. A short portfolio-style write-up is available in [`docs/Revenue_Margin_Dashboard_Brief_Description.md`](docs/Revenue_Margin_Dashboard_Brief_Description.md).

---

## Project Structure

```
revenue-gross-margin-analysis-dashboard/
│
├── PowerBI/
│   └── Revenue_And_Margin_Analysis.pbix   # Power BI report
│
├── data/
│   ├── Product.xlsx                       # Products, groups, categories, unit costs, suppliers
│   └── Revenue.xlsx                       # Transactional sales report (2019)
│
├── docs/
│   └── Revenue_Margin_Dashboard_Brief_Description.md
│
├── screenshots/
│   ├── Screenshot 2026-03-05 123348.png   # Revenue & Gross Margin dashboard
│   └── Screenshot 2026-03-05 123458.png   # Star schema data model
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## Dashboard Preview

### Revenue & Gross Margin Analysis

![Revenue and Gross Margin Dashboard](screenshots/Screenshot%202026-03-05%20123348.png)

### Star Schema Data Model

![Star Schema Data Model](screenshots/Screenshot%202026-03-05%20123458.png)

---

## Data Architecture

```
Product.xlsx ──► dProduct ──► Supplier
                    ▲
Revenue.xlsx ──► Revenue (fact) ──► dsalesperson
                    ▲
              dCalendar (DAX CALENDARAUTO)
                    │
              Key measure (DAX) ──► 14 visuals
```

Power Query performs header promotion, removal of three preamble rows in the revenue sheet, blank-row removal, type casting, duplicate removal in the salesperson dimension, and a left outer join that attaches supplier images to products.

### Star Schema Relationships

| From | To | Cardinality |
| --- | --- | --- |
| `Revenue[Product ID]` | `dProduct[Product ID]` | Many-to-one |
| `dProduct[Supplier]` | `Supplier[Supplier]` | Many-to-one |
| `Revenue[Salesperson ID]` | `dsalesperson[Salesperson ID]` | Many-to-one |
| `Revenue[Due Date]` | `dCalendar[Date]` | Many-to-one |
| `Revenue[Order Date]` | Hidden local date table used by the Order Date slicer | Many-to-one |

`dCalendar` is a DAX-generated date table used for month labels and the Due Date hierarchy slicer; the Order Date slicer uses a separate hidden local date table. These two date relationships are present in the model but are omitted by `pbixray.relationships` because the date tables are system-flagged. This is why best-month results differ depending on which date slicer is active.

---

## Data Catalog

| Table | Role | Rows | Details |
| --- | --- | --- | --- |
| `Revenue` | Fact | 146,151 lines / 30,847 orders | Order Date, Due Date, Order Number, Product ID, Salesperson ID, Qty Items, Unit Price, Sales Amount |
| `dProduct` | Dimension | 667 | Product ID, Product, Group (23), Category (Food/Drinks), Unit Cost, Supplier, Image |
| `Supplier` | Dimension | 5 | Supplier name and logo URL |
| `dsalesperson` | Dimension | 12 | Salesperson ID, Supervisor (5), Team (Retail / Wholesales / Online) |
| `dCalendar` | Date table | Generated | Month, Month # and a Year/Quarter/Month/Day hierarchy |
| `Key measure` | Measure table | — | Hosts all five DAX measures |

---

## DAX Measures

| Measure | Expression |
| --- | --- |
| `Revenue` | `SUM(Revenue[Sales Amount])` |
| `Cost` | `SUMX(Revenue, RELATED(dProduct[Unit Cost]) * Revenue[Qty Items])` |
| `Gross Margin` | `[Revenue] - [Cost]` |
| `Gross Margin %` | `[Gross Margin] / [Revenue]` |
| `best month` | `"The Best Month Is: " & SELECTEDVALUE(dCalendar[Month])` |

The best-month cards use TopN filters over `dCalendar[Month]`, ordered by the corresponding measure.

---

## Dashboard Pages & Visuals

The PBIX contains three pages:

| Page | Status | Contents |
| --- | --- | --- |
| **Revenue And Gross Margin** | Live, 14 visuals | Revenue / Gross Margin / Margin % cards + best-month cards, three monthly area charts, supplier/product pivot, team-by-category bar chart, date and salesperson slicers |
| **Income Statement** | Hidden design placeholder | SVG background only, no visuals |
| **Simulation** | Hidden design placeholder | SVG background only, no visuals |

---

## Key Findings

- **$9.68M** total revenue, **$4.02M** gross margin and a **41.5%** blended margin.
- **Two Brothers Mill** was the largest supplier at **$5.12M** revenue, but its **37.2%** margin was the lowest of the five suppliers.
- **Kappa Drinks** was the most profitable supplier relative to size: **$878K** revenue and **$495K** gross margin (**56.4%**).
- On a **Due Date** basis, **November** was the best month with **$1.19M** revenue and **$496K** gross margin; **January** had the best margin percentage at **42.3%**. (By Order Date, the revenue peak is October at **$1.17M**.)
- **Retail** generated the most revenue in Food (**$3.87M**), ahead of Wholesales and Online.

---

## Tools & Technologies

| Category | Tool | Purpose |
| --- | --- | --- |
| Data preparation | Power Query (M) | Cleaning, joins, type casting, reshaping |
| Data modeling | Power BI Desktop | Star schema and report design |
| Calculation | DAX | Revenue, Cost, Gross Margin and best-month measures |
| Documentation | Markdown | Project brief and README |
| Version control | Git & GitHub | Project hosting |

---

## Getting Started

1. Clone this repository.
2. Open [`PowerBI/Revenue_And_Margin_Analysis.pbix`](PowerBI/Revenue_And_Margin_Analysis.pbix) in **Power BI Desktop**.
3. If prompted to refresh, repoint the Power Query sources to the local `data/Product.xlsx` and `data/Revenue.xlsx` files — the PBIX retains the original authoring paths.
4. Use the date hierarchy and salesperson slicers to explore supplier, category and team profitability.

---

## License

MIT License — see [`LICENSE`](LICENSE).

## Author

**Mohamed Basem**
