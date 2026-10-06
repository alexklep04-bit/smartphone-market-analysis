# Smartphone Market Analysis

What drives the price of a smartphone, and which phones get 5G? A data science project on **980 smartphones sold in India in 2023**, from the raw data to cleaning, exploratory analysis, hypothesis testing and regression.

A two-person team project from the **T4EU Data Science Winter School** in Katowice, Poland.

![Price by RAM](figures/06_price_by_ram.png)

## Dataset

[Smartphones dataset](https://www.kaggle.com/datasets/informrohit1/smartphones-dataset) on Kaggle (MIT license), collected from the Indian comparison site Smartprix. It has 980 phones and 26 columns: price in rupees (₹), rating, chip, RAM, storage, battery, charging, screen, cameras, 5G, NFC and more.

## Progress

| Task | Topic | Notebook |
|---|---|---|
| 1 | Data preprocessing and exploratory data analysis | [Task1_Preprocessing_EDA.ipynb](Task1_Preprocessing_EDA.ipynb) ✅ |
| 2 | Hypothesis testing: one-sample, two-sample and chi-square tests | next |
| 3 | Linear and logistic regression | planned |

## Task 1 in short

- **Missing values** in 10 columns: dropped the half-empty memory-card column, treated "no fast charging" (0 W) differently from "power unknown", and filled ratings using phones in the same price group.
- **Cleaning:** removed 27 phones that were never released (rumours and concepts such as the "Tesla Pi Phone" or the "iPhone 14 Mini"), 3 duplicates and 4 luxury editions, and merged inconsistent chip and brand names.
- **Outliers:** compared the z-score and IQR methods. The IQR method flags far more phones, because half of all phones have exactly 5,000 mAh. Real extremes were kept, and price was log-transformed.
- **New features:** price in euros, price segment, chip maker, pixel density and a foldable flag.
- Every change between the raw and the clean file is listed in [data/changes_log.csv](data/changes_log.csv).

## Key findings so far

- A typical phone costs **₹19,990 (about €222)**. 5G phones have a median price of ₹29,990, against ₹12,499 for 4G-only phones.
- **Processor speed** is the strongest single predictor of price (r = 0.81 with log price), followed by rating (0.74) and RAM (0.70).
- **56% of phones have 5G**, but only 13% of budget phones against 93% of premium ones. MediaTek Dimensity chips are 99% 5G, Helio chips only 1%.

![Share of 5G phones by chip family](figures/08_5g_by_chip_family.png)

## How to run

```bash
pip install -r requirements.txt
jupyter notebook Task1_Preprocessing_EDA.ipynb
```

Run All recreates the charts in `figures/` and `data/smartphones_clean.csv`.

## Project structure

```
├── Task1_Preprocessing_EDA.ipynb    Task 1: cleaning and EDA
├── data/
│   ├── smartphones_cleaned_v6.csv   raw data from Kaggle
│   ├── smartphones_clean.csv        clean data, the output of Task 1
│   └── changes_log.csv              every change between the two files
├── figures/                         charts used in the presentation
└── requirements.txt
```
