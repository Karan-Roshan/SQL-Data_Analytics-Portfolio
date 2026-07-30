# Pizza Hut Sales Analysis: A SQL Case Study

## Overview

This project analyzes a full year of transactional data from a Pizza Hut outlet to uncover the story behind its sales performance. Using SQL as the primary analytical engine and Excel for supporting data preparation, the analysis moves from basic order-level metrics to advanced revenue ranking, translating raw transaction records into decisions a restaurant manager can act on.

---

## Problem Statement

A Pizza Hut location had a full year of order data sitting in four separate tables, but no clear picture of what that data was actually saying. Orders were being placed, pizzas were being sold, and revenue was coming in, but nobody could answer the questions that actually drive a food business forward:

- Which pizzas are carrying the business, and which ones are just taking up menu space?
- When do customers actually order, and is staffing lined up with that demand?
- Is the size customers prefer the size the kitchen is prepping for?
- Which few products are quietly generating a disproportionate share of revenue?

Without answers, decisions on staffing, inventory, pricing, and menu design were being made on instinct rather than evidence. The goal of this project was to turn four raw tables of orders, order details, pizzas, and pizza types into a structured, evidence-based narrative of how the business actually performs.

---

## Tools Used

- **SQL (MySQL)**: core engine for data modeling, joins, aggregation, window functions, and all analytical querying
- **Excel**: used for initial inspection, cleaning, and validation of the raw CSV files before they were loaded into the database

---

## Dataset

The dataset spans **January 1, 2015 to December 31, 2015** and consists of four related tables:

| Table | Description |
|---|---|
| `orders` | Order-level records with date and time of each order |
| `order_details` | Line-item detail linking each order to specific pizzas and quantities |
| `pizzas` | Pizza SKUs with size and price |
| `pizza_types` | Pizza names, categories, and ingredients |

**Files included in this repository:**
- `orders.csv`
- `order_details.csv`
- `pizzas.csv`
- `pizza_types.csv`
- `pizza_hut_analysis.sql` (all queries used in this analysis)

---

## Solution Approach

The analysis was structured in three progressive stages, mirroring how a real business question typically unfolds:

1. **Basic Analysis** — establish the fundamentals: total orders, total revenue, pricing extremes, and customer size preference.
2. **Intermediate Analysis** — layer in time and category dimensions: category-wise demand, hourly order distribution, and daily order volume.
3. **Advanced Analysis** — apply window functions and nested queries to rank products by revenue contribution and track cumulative growth over time.

All queries were written and executed in MySQL against a database (`Pizza_Hut`) built from the four source tables, with joins performed on `pizza_id` and `pizza_type_id` as the relational keys.

---

## Key Findings

### Basic Metrics

| Metric | Result |
|---|---|
| Total orders placed | 21,350 |
| Total revenue generated | $817,860.05 |
| Highest-priced pizza | The Greek Pizza — $35.95 |
| Most common pizza size | Large (L) — 18,526 orders, ahead of Medium (15,385) and Small (14,137) |

### Category and Product Performance

**Top 5 most ordered pizza types (by quantity):**

| Rank | Pizza | Quantity Sold |
|---|---|---|
| 1 | The Classic Deluxe Pizza | 2,453 |
| 2 | The Barbecue Chicken Pizza | 2,432 |
| 3 | The Hawaiian Pizza | 2,422 |
| 4 | The Pepperoni Pizza | 2,418 |
| 5 | The Thai Chicken Pizza | 2,371 |

**Category-wise quantity ordered:**

| Category | Quantity Sold |
|---|---|
| Classic | 14,888 |
| Supreme | 11,987 |
| Veggie | 11,649 |
| Chicken | 11,050 |

Classic pizzas lead in volume, but the picture changes once revenue is examined instead of unit count.

### Order Timing

Orders are heavily concentrated during two windows: a **lunch peak around 12:00–13:00** (over 2,400 orders each hour) and a **dinner peak from 17:00–19:00** (over 2,300 orders each hour), with activity tapering off sharply before 11:00 and after 22:00. This pattern points directly to when staffing and inventory prep should be at their highest.

The business averages **138.47 pizzas sold per day** across the year.

### Revenue Performance

**Top 5 pizza types by revenue:**

| Rank | Pizza | Revenue |
|---|---|---|
| 1 | The Thai Chicken Pizza | $43,434.25 |
| 2 | The Barbecue Chicken Pizza | $42,768.00 |
| 3 | The California Chicken Pizza | $41,409.50 |
| 4 | The Classic Deluxe Pizza | $38,180.50 |
| 5 | The Spicy Italian Pizza | $34,831.25 |

This is the most important insight in the dataset: **Chicken pizzas dominate the revenue leaderboard despite Chicken being the lowest-volume category overall.** Higher per-unit pricing on chicken pizzas more than compensates for lower order counts, meaning volume alone is a misleading measure of product value.

**Revenue contribution by category:**

| Category | Share of Total Revenue |
|---|---|
| Classic | 26.91% |
| Supreme | 25.46% |
| Chicken | 23.96% |
| Veggie | 23.68% |

Revenue is distributed fairly evenly across all four categories, with no single category overwhelmingly dominant, indicating a well-balanced menu rather than reliance on one product line.

**Top 3 revenue-generating pizzas within each category:**

| Category | Top 3 Pizzas by Revenue |
|---|---|
| Chicken | Thai Chicken ($43,434.25), Barbecue Chicken ($42,768.00), California Chicken ($41,409.50) |
| Classic | Classic Deluxe ($38,180.50), Hawaiian ($32,273.25), Pepperoni ($30,161.75) |
| Supreme | Spicy Italian ($34,831.25), Italian Supreme ($33,476.75), Sicilian ($30,940.50) |
| Veggie | Four Cheese ($32,265.70), Mexicana ($26,780.75), Five Cheese ($26,066.50) |

---

## Overall Business Insights

- The business processed **21,350 orders** generating **$817,860.05** in revenue over the year, confirming healthy, sustained sales activity.
- **Large pizzas are the clear customer preference**, which should directly inform ingredient and dough prep ratios in the kitchen.
- **Chicken-category pizzas punch above their weight**, generating top-tier revenue despite lower order volume, making them strong candidates for premium positioning or targeted promotion.
- **Classic pizzas remain the volume leader**, making them the reliable backbone of daily operations even though they are not the single highest revenue driver.
- **Order volume peaks at lunch and dinner**, giving management a clear, data-backed basis for shift scheduling and inventory timing.
- **Revenue is well distributed across categories** (roughly 24-27% each), suggesting the current menu structure is balanced rather than over-reliant on any one product line.
- **Cumulative revenue tracking** confirms steady, compounding growth across the year rather than isolated spikes, supporting confidence in the business's underlying stability.

## How to Reproduce
1. Create the database and load the four CSV files into their respective tables (`orders`, `order_details`, `pizzas`, `pizza_types`) using MySQL Workbench's import wizard or `LOAD DATA INFILE`.
2. Run `pizza_hut_analysis.sql` sequentially; each section is labeled by analysis stage (Basic, Intermediate, Advanced).
3. Review the result sets against the Key Findings section above to validate the numbers.
