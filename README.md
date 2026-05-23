# Quantium-customer-data-analytics

This repository contains a Python-based implementation of the Quantium Retail Strategy & Analytics virtual internship project for the chips category. It covers customer behaviour analysis, an in-store layout experiment (trial vs control stores), and preparation of a client-facing presentation based on the insights.

All analysis is implemented in Python using Jupyter notebooks and CSV/XLSX data.

Project structure
quantium_analysis.ipynb
End-to-end analysis notebook, including data cleaning, feature engineering, exploratory analysis, trial vs control store experiment, statistical testing, and final insights.

QVI_transaction_data.xlsx / QVI_data.csv
Transaction-level dataset for chips purchases, with store, product, customer and time fields.

QVI_purchase_behaviour.csv
Customer-level demographic segments (lifestage, premium/mainstream/budget) linked by loyalty card number.

QVI_data.csv
Aggregated / prepared dataset exported from Task 1, reused for Task 2 and visualisation.

requirements.txt
Python dependencies for running the notebook and reproducing the analysis.

Data description
Transaction data
Core transaction fields used in the analysis:

DATE – Transaction date (converted from integer to proper date type)

STORE_NBR – Store identifier

LYLTY_CARD_NBR – Loyalty card identifier (links to customer segment)

TXN_ID – Transaction identifier

PROD_QTY – Quantity of packs purchased

TOT_SALES – Total dollar spend per transaction line

PROD_NAME – Product description used to derive:

PACK_SIZE (e.g. 175g, 150g)

BRAND (first word, cleaned and consolidated)

Key cleaning steps implemented in Python:

Remove non-chip products (e.g. salsa) from the dataset.

Convert integer DATE to a proper date using the appropriate origin.

Identify and remove extreme outlier transactions (e.g. 200 packs in a single purchase by a non-typical customer).

Ensure continuous daily coverage between July 2018 and June 2019; identify expected gaps (e.g. Christmas Day store closures) via transaction counts over time.

Customer segment data
Customer segments are brought in via QVI_purchase_behaviour.csv:

LIFESTAGE – e.g. YOUNG FAMILIES, OLDER FAMILIES, YOUNG SINGLES/COUPLES, RETIREES.

PREMIUM_CUSTOMER – e.g. Budget, Mainstream, Premium.

These are left-joined onto transaction data via LYLTY_CARD_NBR to create a combined dataset for segment-level analysis.

Task 1 – Customer behaviour and segment analysis (Python)
Objective: Understand chip-buying behaviour across customer segments and identify which segments drive sales.

Feature engineering
Implemented in quantium_analysis.ipynb:

PACK_SIZE: Extracted numeric pack size from PROD_NAME, treated as a categorical feature (e.g. 70g–380g range).

BRAND: First word from PROD_NAME, then cleaned to consolidate variants (e.g. RED and RRD both mapped to “Red Rock Deli”).

Merged transaction and customer segment data to produce a unified analysis dataset (QVI_data).

Exploratory analysis
Key analyses performed:

Sales by lifestage
Total sales aggregated by LIFESTAGE, showing that older families, older singles/couples, and retirees are key revenue contributors.

Sales by premium status
Total sales aggregated by PREMIUM_CUSTOMER, highlighting that budget and mainstream customers collectively contribute the majority of revenue.

Segment-level combination
A combined view of LIFESTAGE × PREMIUM_CUSTOMER to identify high-value segments (e.g. mainstream young singles/couples, budget older families).

Purchase frequency
Number of unique transactions (TXN_ID) per segment to understand frequency vs basket size.

Average quantity per transaction
Average PROD_QTY per transaction by segment to see which segments buy more packs per visit.

Pack size distribution
Histogram and summary of PACK_SIZE showing dominance of 175g packs and strong preference for mid-sized options (e.g. 150g, 134g).

Brand performance
Total sales by BRAND, identifying leading brands such as Kettle, Doritos, and Smiths.

Key insights (Task 1)
Based on the Python analysis and visualisations:

Revenue is primarily driven by older families, older singles/couples, and retirees.

Budget and mainstream customers contribute most of the total sales; premium customers are a smaller share.

Purchase frequency is the main driver of revenue rather than significantly higher spend per transaction.

Average transaction value and pack sizes are relatively consistent across segments; behaviour differences come more from “how often” than “how much per visit”.

175g packs dominate sales, indicating a standard preferred size, with mid-size packs also performing well.

Kettle, Doritos, and Smiths are the core must-stock brands across segments.

There is limited differentiation between premium and non-premium customers in chips purchasing behaviour; chips purchases appear more habit-driven than luxury-driven.

Task 2 – Trial vs control store experiment (layout uplift)
Objective: Evaluate the impact of a new shelf layout for chips in trial stores 77, 86, and 88 by comparing to suitable control stores and measuring uplift.

Experimental design
Using the combined prepared dataset (QVI_data) and transaction-level data:

Trial stores: 77, 86, 88.

Trial period: February 2019 – April 2019.

Candidate controls: All stores with complete monthly sales coverage from July 2018 to June 2019.

Metrics for similarity:

Monthly total sales revenue

Monthly number of customers

Average number of transactions per customer

A control store is selected for each trial store by ranking potential controls on similarity (e.g. correlation or distance metrics) and choosing the closest match.

Analysis steps
Implemented in Python in quantium_analysis.ipynb:

Aggregate transaction data to monthly store level: total sales, number of customers, and transactions per customer.

Compute similarity between each trial store and candidate control stores, then select one control per trial store.

For each trial–control pair, compare:

Pre-trial trends to validate similarity.

Trial-period total sales to estimate uplift.

Use statistical testing (two-sample t-tests) to determine whether differences in trial-period total sales between trial and control are significant.

Statistical testing and drivers
A t-test is run for each trial–control pair on trial-period total sales:

Store 88 shows a statistically significant uplift (p-value well below 0.05).

Store 77 is borderline (p-value close to 0.05).

Store 86 is clearly not significant.

Driver analysis decomposes uplift into:

Change in number of customers

Change in transactions per customer

Change in average spend per customer

Findings:

The primary driver of uplift is increased customer count; transactions per customer and average spend change only marginally.

Layout appears to increase store traffic more than it changes in-aisle purchasing behaviour.

Caveats and validation
In some cases (notably Store 88), the control store is trend-similar but not magnitude-similar, which can inflate measured uplift and requires caution when generalising results.

Store 86 shows weaker pre-trial alignment to its control, reducing confidence in conclusions for that store.

Task 3 – Client presentation and storytelling
Objective: Translate the technical analysis into a clear, commercially relevant presentation for the Category Manager of chips, using the Pyramid Principle structure.

Presentation structure
The Task 3 deliverable include:

Executive summary

Who drives chips sales (key segments).

Overall impact of the new layout trial.

High-level recommendation on rollout.

Customer behaviour insights (Task 1)

Segment-level revenue contributions.

Frequency vs basket size drivers.

Pack size and brand preferences.

Implications: which segments and products to prioritise.

Trial vs control experiment (Task 2)

Methodology for selecting control stores.

Pre-trial trend checks.

Trial-period results, uplift charts, and statistical significance.

Driver analysis (customer count vs transactions vs spend).

Recommendations and next steps

Rollout strategy: focus on stores similar to Store 88 (and partly Store 77), not blanket rollout.

Complementary actions to increase basket size (bundles, promotions, cross-sell).

Suggestions for further trials with improved control matching and broader store coverage.

Cover email to client summarising key outcomes and offering to discuss results.

Environment and dependencies
All analysis is done in Python using Jupyter Notebook.

Typical stack (see requirements.txt):

pandas – Data manipulation

numpy – Numerical computing

matplotlib / seaborn – Visualisation

scipy – Statistical tests (e.g. t-tests)

openpyxl or similar – Reading Excel transaction data

To set up a compatible environment:

bash
# Create virtual environment (optional but recommended)
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows

# Install dependencies
pip install -r requirements.txt
How to run
Clone the repository and place the raw data files (QVI_transaction_data.xlsx, QVI_purchase_behaviour.csv, QVI_data.csv) in the repository root (or update paths in the notebook accordingly).

Install dependencies using pip install -r requirements.txt.

Open the notebook:

bash
jupyter notebook quantium_analysis.ipynb
Run all cells to reproduce:

Notes and limitations
Analysis is tailored to the chips category and the specific timeframe July 2018–June 2019.

The experimental uplift results depend heavily on control store selection and pre-trial similarity; interpretation should consider these limitations.
