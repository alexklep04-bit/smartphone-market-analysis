# Smartphone Market Analysis

What drives the price of a smartphone, and which phones get 5G? In this project we analyse **980 smartphones sold in India in 2023**: we clean the raw data, explore it, test hypotheses and build regression models.

We made this project as a team of two at the **T4EU Data Science Winter School** in Katowice, Poland.

![Price by RAM](figures/06_price_by_ram.png)

## Dataset

We use the [Smartphones dataset](https://www.kaggle.com/datasets/informrohit1/smartphones-dataset) from Kaggle (MIT license), collected from the Indian comparison site Smartprix. It has 980 phones and 26 columns: price in rupees (₹), rating, chip, RAM, storage, battery, charging, screen, cameras, 5G, NFC and more.

## Progress

| Task | Topic | Notebook |
|---|---|---|
| 1 | Data preprocessing and exploratory data analysis | [Task1_Preprocessing_EDA.ipynb](Task1_Preprocessing_EDA.ipynb) ✅ |
| 2 | Hypothesis testing: one-sample, two-sample and chi-square tests | [Task2_Hypothesis_Testing.ipynb](Task2_Hypothesis_Testing.ipynb) ✅ |
| 3 | Linear and logistic regression | [Task3_Regression.ipynb](Task3_Regression.ipynb) ✅ |

## Task 1 in short

- **Missing values** in 10 columns: we dropped the half-empty memory-card column, treated "no fast charging" (0 W) differently from "power unknown", and filled the missing ratings using phones in the same price group.
- **Cleaning:** we removed 27 phones that were never released (rumours and concepts such as the "Tesla Pi Phone" or the "iPhone 14 Mini"), 3 duplicates and 4 luxury editions, and we merged inconsistent chip and brand names.
- **Outliers:** we compared the z-score and IQR methods. The IQR method flags far more phones, because half of all phones have exactly 5,000 mAh. We kept the real extremes and log-transformed the price.
- **New features:** we added the price in euros, a price segment, the chip maker, the pixel density and a foldable flag.
- We listed every change between the raw and the clean file in [data/changes_log.csv](data/changes_log.csv).

## What we found in Task 1

- A typical phone costs **₹19,990 (about €222)**. 5G phones have a median price of ₹29,990, against ₹12,499 for 4G-only phones.
- **Processor speed** is the strongest single predictor of price (r = 0.81 with log price), followed by rating (0.74) and RAM (0.70).
- **56% of phones have 5G**, but only 13% of budget phones against 93% of premium ones. MediaTek Dimensity chips are 99% 5G, Helio chips only 1%.

![Share of 5G phones by chip family](figures/08_5g_by_chip_family.png)

## Task 2 in short

Do you pay for the specs or for the brand? We tested six hypotheses at α = 0.05:

| Test | Our question | What we found |
|---|---|---|
| One-sample t-test | Is the average battery 5,000 mAh? | **No:** the mean is 4,812 mAh (p < 0.001), although 5,000 mAh is the most common size. |
| One-sample test for a proportion | Can most phones pay contactlessly (NFC)? | **No:** only 39% of phones have NFC (p < 0.001). It's a premium feature: 5% of budget phones vs 97% of flagships. |
| Two-sample t-test (Welch) | Do iPhones have better specs than Android flagships? | **No, the opposite:** spec score 79.5 vs 87.4 (p < 0.001), while iPhones cost more. |
| Two-sample t-test (Welch) | Is a flagship worth twice the price? | **Hardly:** Android flagships cost more than twice as much as Premium phones (€933 vs €444) but score only 2.4% higher (p < 0.001). |
| Chi-square test of homogeneity | Do the five biggest brands follow the same price strategy? | **No:** Realme sells mostly budget phones and no flagships, Samsung has the most flagships (p < 0.001). |
| Chi-square test of independence | Does 5G depend on the brand? | **No:** 48–60% for every brand (p = 0.45). 5G depends on the price instead. |

Our answer: for most phones you pay for the specs, but the extra money buys less and less at the top. iPhones are the exception, where you also pay for the brand.

![iPhones vs Android flagships](figures/t2_03_iphone_vs_android.png)

## Task 3 in short

We built two models and tested them on 190 phones that they had not seen during training (20% of the data). Before that, we corrected three wrong 5G labels: for example, the iPhone 15 was marked as a phone without 5G.

| Model | Our question | What we found |
|---|---|---|
| Multiple linear regression for log(price) | What drives the price of a phone? | Processor speed, RAM, storage, NFC, being an iPhone and being foldable all raise the price (all p < 0.001). The model explains 85% of the variation in log price, and its average error is €84 (23% of the price). |
| Binary logistic regression for 5G | Which phones get 5G? | The chance of 5G rises with the price, the processor speed and the screen refresh rate (all p < 0.001). The model is right for 87% of the phones, with an F1-score of 0.88 and a ROC-AUC of 0.93. |

With exactly the same specs, **an iPhone costs 2.3 times as much as an Android phone**, and a foldable 2.1 times as much.

![How much each feature adds to the price](figures/t3_02_price_effects.png)

## How to run

```bash
pip install -r requirements.txt
jupyter notebook
```

Run the notebooks in order. Task 1 creates `data/smartphones_clean.csv`, which Tasks 2 and 3 use. Each notebook also saves its charts in `figures/`.

## Project structure

```
├── Task1_Preprocessing_EDA.ipynb    Task 1: cleaning and EDA
├── Task2_Hypothesis_Testing.ipynb   Task 2: hypothesis testing
├── Task3_Regression.ipynb           Task 3: linear and logistic regression
├── data/
│   ├── smartphones_cleaned_v6.csv   raw data from Kaggle
│   ├── smartphones_clean.csv        clean data, the output of Task 1
│   └── changes_log.csv              every change between the two files
├── figures/                         charts used in the presentation
└── requirements.txt
```
