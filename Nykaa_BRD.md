# Business Requirements Document: Nykaa Business Report (Power BI)

**Version:** 1.1 | **Date:** 28 Oct 2026 | **Author:** Tanishka Gaikwad | **Status:** Sample / portfolio document

> **Note:** This BRD was written for a self-initiated portfolio project that uses simulated data. Stakeholder roles are assumed personas, not real clients. Findings illustrate the analysis method, not real Nykaa performance.

## 1. Purpose and Problem Statement

A beauty e-commerce business records 26,000 order lines (15,493 orders from 4,459 customers, Jan 2024 to Sep 2026) in a flat file. Teams cannot easily see which categories and discount levels earn a profit, where returns and cancellations concentrate, or how reliable delivery is. This document defines the requirements for a 3-page Power BI dashboard that answers these questions in one place.

## 2. Business Objectives

1. Show **profit and margin**, not only sales, by category, region, brand and discount level.
2. Identify which **campaigns and brands** drive profit.
3. Reduce avoidable **returns and cancellations** by showing where they concentrate.
4. Improve **delivery reliability** and track **repeat-customer** behaviour.

## 3. Scope

| In scope | Out of scope |
|---|---|
| Dashboard with 3 pages: **Sales Analysis**, **Profit Analysis**, **Customer Analysis** | Real-time or live data feeds |
| Star-schema model built with Power Query ETL (1 fact and 4 dimension tables, plus a date table) | Forecasting or predictive models |
| 23 DAX measures for the KPIs below | Row-level security and data write-back |
| Written business problems with recommendations | Integration with Nykaa's real systems |

## 4. Stakeholders (assumed personas)

| Persona | Need |
|---|---|
| Category / Merchandising Manager | Which categories, brands and products earn or lose money |
| Regional Sales Manager | Sales, profit and margin by region and state |
| Promotions / Marketing Manager | Discount effectiveness and campaign profit |
| Operations Manager | Delivery reliability, returns and cancellations |
| CRM Lead | Customer base and repeat purchase behaviour |

## 5. User Stories and Acceptance Criteria

| ID | User story | Acceptance criteria | Priority |
|---|---|---|---|
| US-01 | As a Leader, I want headline KPIs on one screen, so that I can judge performance quickly. | Sales Analysis page shows Total Sales, Total Profit, Profit Margin %, Units Sold and Total Orders as KPI cards. | Must |
| US-02 | As a Category Manager, I want sales by month, category and sales channel, so that I can see trends and mix. | Monthly sales trend, category treemap (sales and orders) and sales-channel donut shown. | Must |
| US-03 | As a Regional Manager, I want margin by state and profit by region, so that I can find weak and strong areas. | Margin % bar chart by state; profit waterfall by region; top region shown as a KPI. | Must |
| US-04 | As a Promotions Manager, I want margin and sales by discount band and order status, so that I can find where discounting destroys margin. | Bands: 0%, 1-10%, 11-20%, 21-30%, 31%+; margin % and sales shown per band. | Must |
| US-05 | As a Merchandising / Marketing Manager, I want top brand, best-selling brand, top-10 product share and profit by campaign and brand, so that I can focus spend. | KPIs for the above; "Top Campaign By Profit" excludes "No Campaign"; campaign and brand profit bar charts. | Should |
| US-06 | As an Operations Manager, I want on-time delivery % overall and by city tier, so that I can target delivery problems. | KPI card plus a column chart by city tier, calculated on delivered lines only. | Must |
| US-07 | As an Operations Manager, I want return and cancellation rates over time, return reasons and cancellations by payment method, so that I can cut avoidable losses. | Monthly trend of both rates; return-reason donut; payment-method scatter (cancellation rate, orders, sales). | Must |
| US-08 | As a CRM Lead, I want total customers and repeat-customer %, so that I can monitor loyalty. | Both shown as KPI cards on Customer Analysis, using the definition in Section 6. | Should |
| US-09 | As any user, I want slicers for year, brand, category, state, city, sales channel, city tier and region, so that I can drill into my own area. | Slicers available per page; a selection updates every visual on that page. | Must |

## 6. KPI Definitions

| KPI | Definition (as implemented) |
|---|---|
| Total Sales | Sum of Net Sales (after discount) |
| Profit Margin % | Total Profit ÷ Realized Revenue |
| On-Time Delivery % | Delivered order lines that arrived within the estimated days ÷ all delivered order lines |
| Return Rate % | Distinct orders with status "Returned" ÷ distinct orders |
| Cancellation Rate % | Distinct orders with status "Cancelled" ÷ distinct orders |
| Repeat Customers % | Customers with 2+ distinct orders ÷ total customers |

## 7. Data Requirements

- **Source:** Excel workbook with six sheets: `fact_orders` (26,000 rows), `dim_products` (450), `dim_customers` (6,000), `dim_geography` (26), `dim_campaigns` (12) and a date table.
- **Model:** many-to-one relationships from the fact table to products, customers, campaigns and geography.
- **Derived columns:** Delivery Status (On Time, Delayed or Not Delivered) and Discount Percentage Band.
- **Quality rules:** valid dates, no duplicate order-line IDs, no blank sales or profit fields, and all cleaning done in Power Query so it is repeatable.

## 8. Assumptions, Open Questions and Risks

- Data is simulated, so insights show the method, not real business results.
- **Open question:** one order can hold lines with different statuses, so the order-level return and cancellation rates count an order if any line is affected (14.1% and 9.3% overall, versus 8.8% and 5.7% at line level). The business should confirm which view it wants.
- **Open question:** margin uses Realized Revenue, not Net Sales, and on-time % is counted per order line. Confirm both definitions with stakeholders.
- **Data issue:** 14 order lines have no payment method and need a rule (exclude or label "Unknown").

## 9. Outcomes Delivered

The dashboard surfaced the following findings, documented as **18 business problems with recommendations**:

- Luxe drives 28% of sales but 61% of profit.
- Appliances loses money, at a -13.1% margin.
- Margins turn negative once discounts pass about 20-25% (the 21-30% band is at -0.8% and 31%+ is at -13.2%).
- COD orders cancel at 15.0% versus 7.8% for UPI.
- Only 56% of delivered order lines arrive on time.
