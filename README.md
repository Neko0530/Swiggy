
# Swiggy Restaurant & Cuisine Intelligence Dashboard

An end-to-end, high-performance Power BI analytics platform built using a custom Star Schema to evaluate restaurant density, cost-to-rating distributions, and multi-valued cuisine performance across Indian markets.

---

## Key Highlights & Features

* **Star Schema Architecture**: Configured a normalized dimensional model featuring a bridge table to resolve many-to-many (`M:N`) relationships between restaurants and multi-valued cuisine listings[cite: 1, 2].
* **Bi-Directional Cross Filtering**: Enabled seamless global filtering across dimensions so selecting any cuisine dynamically recalculates city density and price segment metrics in real time[cite: 1, 2].
* **Two-Page Executive Blueprint**:
  * **Page 1 (Executive Overview)**: High-level market concentration, city rankings, price tier distribution, and top-level KPIs[cite: 1, 3].
  * **Page 2 (Cuisine Intelligence & Value Matrix)**: Rating vs. cost scatter quadrant analysis, market share breakdowns, and an interactive restaurant performance leaderboard with conditional status formatting[cite: 1, 3].
* **Custom Brand Palette**: Styled with a bespoke 8-color theme palette aligned with Swiggy's brand identity (`#FC8019`, `#002B49`, `#11999E`)[cite: 1].
* **Custom DAX KPI Engine**: Engineered dedicated business logic for active outlet volumes, rating distributions, and high-value outlet categorizations[cite: 1, 3].

---

## Data Model & Schema Design


```

```
                 +-----------------------+
                 |       Dim_City        |
                 +-----------------------+
                             | (1)
                             |
                             | (*)

```

+-------------------+ (1)  (*) +-------------------+ (*)  (1) +-----------------------+
|   _Measures       | -------- |  Fact_Restaurant  | -------- | Bridge_Rest_Cuisine   |
|  (Calculations)   |          |     (swiggy)      |          +-----------------------+
+-------------------+          +-------------------+                      | (*)
|
| (1)
+-----------------------+
|      Dim_Cuisine      |
+-----------------------+

```

### Relationship Configuration
1. **`swiggy` $\leftarrow$ `Dim_City`**: `1:Many` single direction on `city`.
2. **`swiggy` $\leftrightarrow$ `Bridge_Restaurant_Cuisine` $\leftrightarrow$ `Dim_Cuisine`**: `1:Many` with **Bi-directional Cross Filtering (`Both`)** to handle multi-valued cuisine text arrays cleanly without duplication errors[cite: 1, 2].

---

## Executive Dashboard Pages

### 📄 Page 1: Executive Overview & Market Density
* **KPI Ribbon**: Quick visibility into Total Outlets, Average Rating, Avg Cost for Two, and Total Rated Outlets[cite: 1, 3].
* **Top 10 Cities Concentration**: Horizontal bar visualization showing top market presence[cite: 1, 3].
* **Price Tier Segmentation**: Categorized cost metrics into *Budget ($\le$ ₹200)*, *Mid-Range (₹201–₹400)*, and *Premium (> ₹400)* tiers[cite: 1].
* **Header Slicers**: Instant filtering by city dropdowns and numeric rating sliders[cite: 1, 3].

### 📄 Page 2: Cuisine Intelligence & Value Matrix
* **Rating vs. Cost Value Matrix**: Scatter graph using median reference lines (₹250 cost / 3.9 rating) to segment outlets into four distinct performance quadrants[cite: 1, 3]:
  * **Sweet Spot**: High Rating / Low Cost
  * **Premium Dining**: High Rating / High Cost
  * **Mass Budget**: Low Rating / Low Cost
  * **Value at Risk**: Low Rating / High Cost
* **Cuisine Share Treemap & Bar Chart**: Multi-valued cuisine distribution powered by the bridge model[cite: 1, 2, 3].
* **Performance Leaderboard**: High-density table featuring background conditional formatting (`#2ECC71` for top tier, `#E74C3C` for low tier)[cite: 1, 3].

---

## Key DAX Expressions

### Total Active Outlets
```dax
Total Restaurants = COUNTROWS('swiggy')

```

### Average Customer Rating

```dax
Avg Rating = AVERAGE('swiggy'[rating])

```

### High Value Outlets ($\ge 4.2$ Rating)

```dax
High Value Outlets = 
CALCULATE(
    [Total Restaurants],
    FILTER(
        'swiggy',
        NOT(ISBLANK('swiggy'[rating])) && 'swiggy'[rating] >= 4.2
    )
)

```

### Calculated Price Segments

```dax
Price Segment = 
VAR CostVal = 'swiggy'[cost]
RETURN
    SWITCH(
        TRUE(),
        ISBLANK(CostVal), "Unpriced",
        CostVal <= 200, "1. Budget (<= ₹200)",
        CostVal <= 400, "2. Mid-Range (₹201-₹400)",
        "3. Premium (> ₹400)"
    )

```

---



---

## How to Run This Project

1. Clone or download this repository to your local machine.
2. Open `swiggy-analytics.pbix` in **Power BI Desktop** (October 2023 build or newer recommended).
3. If prompted for data source updates, point the file path to `data/swiggy.csv`.
4. Use **`Ctrl + Click`** on page navigation buttons to explore interactively within Desktop mode.



```

```
