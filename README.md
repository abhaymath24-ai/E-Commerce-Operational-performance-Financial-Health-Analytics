# E-Commerce-Operational-performance-Financial-Health-Analytics
End-to-end e-commerce BI pipeline: PostgreSQL relational modeling, custom DAX metrics, and a 2-page interactive Power BI dashboard.
# 📊 E-Commerce Operational Performance & Financial Health Analytics

An end-to-end business intelligence and data analytics project analyzing ~50,000 transaction records. This repository covers relational database modeling in PostgreSQL, explicit DAX business metric formulation, and a two-page executive Power BI dashboard report designed for multi-tier decision-making.

---

## 📸 Dashboard Previews

### Page 1: Executive Overview
*High-level financial KPIs, monthly revenue seasonality, category sales distribution, customer segmentation, and payment gateway health.*

![Page 1 - Executive Overview](executive_overview.png)

---

### Page 2: Operational Deep-Dive
*Granular operational breakdown: discount bracket margins, chronological day-of-week fulfillment volume, regional metro sales, and top product revenue performance.*

![Page 2 - Operational Deep-Dive](operational_deepdive.png)

---

## 🛠️ Tech Stack & Architecture

| Layer | Tools / Technologies | Implementation Details |
|---|---|---|
| **Database** | PostgreSQL | Relational Star Schema, multi-table joins, views, data cleaning |
| **BI & Modeling** | Power BI Desktop | Star schema design, dimensional relationships, sort-order dependencies |
| **Calculations** | DAX (Data Analysis Expressions) | Explicit measures table: `Net Revenue`, `Total Orders`, `AOV`, `Cancellation Rate` |
| **Reporting** | Executive UI Design | Cohesive dual-tier dashboard, container card UX, dynamic slicers |

---

## 📐 Data Model & Star Schema Architecture

The data architecture transitions transactional source data into a clean Star Schema:
* **Fact Table:** `orders` (Transaction keys, quantity, unit price, discounts, order dates, status)
* **Dimension Tables:**
  * `customers` (Customer ID, geographic profile, segment: *Regular, New, VIP*)
  * `products` (Product ID, SKU names, product categories)
  * `payments` (Payment ID, payment method, gateway transaction status)

---

## 🧮 Key DAX Measures Implemented

All calculations reside in a centralized `_Measures` table to optimize model performance and maintain clean separation of concerns:

```dax
// 1. Total Orders
Total Orders = COUNTROWS('orders')

// 2. Net Revenue (Accounting for item quantity and applied discounts)
Net Revenue = 
SUMX(
    'orders',
    'orders'[Quantity] * 'orders'[UnitPrice] * (1 - 'orders'[Discount])
)

// 3. Average Order Value (AOV)
Average Order Value = 
DIVIDE([Net Revenue], [Total Orders], 0)

// 4. Cancellation Rate
Cancellation Rate = 
DIVIDE(
    CALCULATE([Total Orders], 'orders'[OrderStatus] = "Cancelled"),
    [Total Orders],
    0
)

