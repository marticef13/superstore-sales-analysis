# Superstore Sales Analysis

Exploratory data analysis on the [Sample Superstore dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) from Kaggle, using MySQL for querying and Python for visualization.

---

## Objectives

Starting from the raw sales data, I wanted to answer three business questions:

1. Which product category generates the most profit?
2. Are there regions that sell a lot but earn little?
3. How did sales and profit trend year over year?

The answers led to deeper follow-up questions, documented below.

---

## Tools

- **MySQL** — data storage and querying
- **Python** — pandas, matplotlib, seaborn
- **VS Code** — development environment

---

## Analysis

### 1. Profit margin by category

Technology and Office Supplies both show healthy margins (~17%), while Furniture stands out with a margin of only ~2.5%.

**Follow-up question:** what is dragging Furniture down?

Drilling into subcategories revealed that Tables and Bookcases both have negative total profit. Inspecting the raw data pointed to the `Discount` column as the likely cause.

### 2. The discount problem in Furniture

By grouping orders by subcategory and discount level, a clear pattern emerged:

| Subcategory | Break-even discount threshold |
|---|---|
| Tables | ~0.2 |
| Bookcases | ~0.3 |
| Chairs | ~0.3 |
| Furnishings | ~0.4 |

Above these thresholds, average profit turns negative — and the volumes are large enough (50–250 orders per band) to rule out outliers.

Tables at a 0.4 discount alone account for **-$16,187** in total losses.

### 3. Profit margin by region

The Central region has the lowest margin (~8%), well below West (~15%) and East (~14%).

**Follow-up question:** does Central sell a lot of low-margin products?

Filtering by region confirmed that Furniture in the Central region generates **negative profit (-$2,871)**, despite sales volume comparable to Technology. The cause: Central applies the highest average discounts across categories, particularly on Furniture.

### 4. Year-over-year trend (2014–2017)

Sales grew ~52% over the period, but profit grew much more slowly and stayed compressed at the bottom. This is consistent with the discount findings — revenue is scaling, but aggressive discounting is eroding the margin gains.

---

## Key Finding

The underperformance of Furniture — both as a category and in the Central region — is not a product problem. It is a **discount policy problem**. Furniture is profitable at low or zero discounts; it becomes loss-making when discounts exceed 20–30%. The Central region systematically applies higher discounts, which explains both its low margin and Furniture's negative profit there.

---

## Repository Structure

```
superstore-analysis/
├── README.md
├── superstore_analysis.ipynb
└── images/
    ├── superstore_analysis.png       # margin by category and region, sales trend
    ├── superstore_analysis2.png      # mean profit by discount level per subcategory
    └── superstore_analysis3.png      # scatter plot: discount vs profit, Furniture
```

---

## Setup

1. Clone the repository
2. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) and import it into MySQL as a database named `superstore`
3. Create a `.env` file in the root folder:
   ```
   MYSQL_PASSWORD=your_password
   ```
4. Install dependencies:
   ```bash
   pip install pandas matplotlib seaborn mysql-connector-python python-dotenv
   ```
5. Run `superstore_analysis.ipynb`
