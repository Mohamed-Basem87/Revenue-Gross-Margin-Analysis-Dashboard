# Revenue & Gross Margin Analysis Dashboard — Project Description

**Tool:** Microsoft Power BI | **Type:** Profitability Analytics Portfolio Project

## Overview
The Revenue & Gross Margin Analysis Dashboard is a Power BI report built on product, supplier, salesperson and transactional revenue data. It transforms raw Excel sources into a star schema and measures operational revenue, cost, gross margin and margin percentage across suppliers, product categories and sales teams.

## What It Does
- Cleans and reshapes two raw Excel workbooks (`Product.xlsx` and `Revenue.xlsx`) into four Power Query tables (products, suppliers, salespeople and revenue), including header promotion, preamble-row removal, type casting and a product-to-supplier merge that attaches supplier logos.
- Loads a star schema with `Revenue` as the fact table and `dProduct`, `Supplier`, `dsalesperson` and a DAX-generated `dCalendar` as dimensions.
- Calculates Revenue, Cost, Gross Margin and Gross Margin % with DAX, plus a dynamic "best month" label driven by TopN filters.
- Breaks profitability down by supplier (5), product group (23), category (Food / Drinks) and sales team (Retail / Wholesales / Online).
- Presents one live report page (Revenue & Gross Margin) plus two hidden design placeholders (Income Statement and Simulation).

## Data Model
A star schema with one fact table, four dimensions and five DAX measures. Relationships: Revenue → dProduct → Supplier, and Revenue → dsalesperson.

## Key Findings
- $9.68M total revenue, $4.02M gross margin, 41.5% blended margin.
- Two Brothers Mill led revenue at $5.12M but had the lowest margin (37.2%).
- Kappa Drinks had the best margin at 56.4% ($878K revenue, $495K gross margin).
- On a Due Date basis, November was the best month ($1.19M revenue, $496K margin); January had the best margin percentage (42.3%). By Order Date, October was the revenue peak ($1.17M).
- Retail generated the most Food revenue at $3.87M.

## Skills Demonstrated
Power BI data modeling, Power Query transformations, DAX measure design, star-schema design, profitability analysis and dashboard storytelling.
