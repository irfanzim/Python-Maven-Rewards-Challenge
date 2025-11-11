# Maven Cafe Rewards Challenge: Customer Segmentation & Offer Analysis

## Project Description

This project analyzes customer behavior from a 30-day promotional test at Maven Cafe. The goal is to understand how different customer segments respond to various offers (Buy-One-Get-One, Discount, Informational). The analysis identifies key customer segments and provides data-driven recommendations for future marketing strategies to maximize impact, increase revenue, and improve customer loyalty.

This case study follows a structured data analytics workflow, moving from data cleaning and preparation to in-depth attribution modeling, exploratory data analysis, and finally, strategic recommendations.


## Objectives

The primary goals of this analysis are to answer three key business questions:
* **Which customers respond best to offers?**
* **What types of offers work best?**
* **How should we reach customers to maximize impact?**

## Dataset Overview

The dataset you can get from https://mavenanalytics.io/challenges/maven-rewards-challenge

The analysis is based on three core datasets simulating customer activity over the 30-day test period:

* **`customers.csv`**: Contains demographic information for 17,000 unique rewards members, including age, gender, income, and membership start date.
* **`events.csv`**: A transactional log of 306,534 events associated with 17,000 unique customers. Events are categorized as 'offer received', 'offer viewed', 'offer completed', or 'transaction'.
* **`offers.csv`**: A portfolio detailing the 10 unique promotional offers tested, including offer type (BOGO, discount, informational), difficulty (spend threshold), reward, duration, and marketing channels.

## Data Preparation & Cleaning

To prepare the data for analysis, several cleaning, transformation, and feature engineering steps were performed:

* The main three datasets (customer, event, offer) were inspected. From the **customer** table, 2,175 rows were removed as outliers, as their `age` was listed as 118. From the **event** table, 397 duplicated rows were identified and deleted.
* **Timestamp Standardization**: The offer `duration` (in days) was converted to `duration_hours` (e.g., 7 days -> 168 hours) to align with the `time` column (in hours) from the event log.
* **Feature Engineering (Offers)**: A human-readable `offer_key` (e.g., 'bogo-5-5-7') was created from the offer attributes for easier analysis and visualization.
* **Feature Engineering (Customers)**:
    * The `became_member_on` column was converted to datetime objects, and a `membership_year` feature was extracted.
    * Customer demographics were binned into cohorts:
        * `age_group`: 'Young Adult', 'Middle Age Adult', 'Older Adult', 'Senior', 'Elderly'
        * `income_group`: 'Low Income', 'Middle Income', 'High Income'
* **Data Merging & Filtering**:
    * The three datasets were merged into a single master timeline (`df_master`).
    * This process identified 33,749 "orphan" events (events from customers not present in the demographic table), which were dropped to ensure data integrity.
    * The final merged dataset, sorted by customer and time, contained 272,388 event rows.

### Offer Attribution Logic

A core challenge was to attribute transactions to specific offers. A function was built to identify **"truly influenced purchases"** by tracking a customer's journey through a strict funnel. This attribution logic was applied only to BOGO and Discount offers.

A successful journey was defined as:
1.  **Offer Received:** The customer receives the offer.
2.  **Offer Viewed:** The customer views the offer *after* receiving it and *before* it expires.
3.  **Offer Completed:** The customer completes the offer (meets the spend threshold) *after* viewing it and *before* it expires.
4.  **Transaction Match:** A `transaction` event occurs at the *exact same time* as the `offer completed` event.

This process yielded the final analysis-ready dataset (`df_final`) containing **22,533 successful offer completions**.

## Exploratory Data Analysis (EDA) & Insights

### Customer Insights
* **Core Demographics**: The customer base is heavily skewed towards **Older Adults (age 50-64)** and **Middle-Income earners ($45k-$84k)**. The largest single segment is "Older Adult + Middle Income."
  
  <img width="899" height="671" alt="image" src="https://github.com/user-attachments/assets/e45dfc60-dc11-4b81-a610-904330f86916" />
  
  <img width="935" height="712" alt="image" src="https://github.com/user-attachments/assets/ae83c35c-da7e-4eda-997c-95b80db39224" />
  
* **Highest Spenders**: While Middle-Income is the largest group, the **High-Income** segment spends the most per offer transaction (Avg. $29.80).
  
   <img width="1104" height="334" alt="image" src="https://github.com/user-attachments/assets/0838ffad-dfb4-4cf7-a23e-c27a989502bd" />
   
   <img width="395" height="120" alt="image" src="https://github.com/user-attachments/assets/82011314-7f94-4bfa-a26f-94127a095823" />
   
* **Age & Spending**: **Seniors** ($22.62) and **Older Adults** ($22.52) have the highest average spend per offer, significantly more than Young Adults ($13.52).
  
  <img width="372" height="150" alt="image" src="https://github.com/user-attachments/assets/f7aa2b61-47dc-4c72-bee3-4fb52c6182a0" />
  
* **Gender**: **Females** tend to spend more per offer transaction ($22.39) than Males ($18.77).

   <img width="1097" height="284" alt="image" src="https://github.com/user-attachments/assets/57e0e9c5-0f74-4b1c-be18-212e0a4acb65" />

### Transaction Insights
* **Offer Value**: Offers are highly effective at driving value.
    * Average Offer-Driven Transaction: **$20.19**
    * Average Non-Offer Transaction: **$12.69**
      
      <img width="713" height="547" alt="image" src="https://github.com/user-attachments/assets/8c01a7fe-c691-43a9-9cd1-dfad68eedfc4" />

* **Key Insight**: Purchases influenced by an offer are **59.2% more valuable** than regular, non-promotional purchases.
* **Financial Impact**:
    * Total Attributed Revenue (BOGO/Discount): **$461,130.75**
    * Total Rewards Paid: **$111,879.00**
    * This represents a **24.26% Reward-to-Sales Ratio** (i.e., for every $1.00 in sales, $0.24 was paid in rewards).
 
      <img width="1092" height="448" alt="image" src="https://github.com/user-attachments/assets/8ca8f90f-3bf8-4bc5-88ac-ce154015759b" />

### Offer & Channel Insights
* **Funnel Leakage**: The marketing funnel reveals two major drop-off points:
    1.  **Discovery Leakage (23.8%)**: 23.8% of all BOGO/Discount offers sent were *never viewed* by the customer.
    2.  **Persuasion Leakage (44.4%)**: Of the customers who *did* view an offer, 44.4% did not complete it.
       
  <img width="581" height="157" alt="image" src="https://github.com/user-attachments/assets/4cc78595-48b4-4656-8c9f-52c6eff975af" />
  
* **Offer Preference**: **Discounts are significantly more persuasive than BOGO offers** across all age groups. Customers who view a discount offer are far more likely to complete it.
  
  <img width="683" height="197" alt="image" src="https://github.com/user-attachments/assets/e7fa7892-203d-4ecb-9cf8-86aec89ee341" />

* **Channel Effectiveness**: The "Discovery Leakage" is directly tied to the marketing channels used.
    * **Best Channel**: The 4-channel combo (`web`, `email`, `mobile`, `social`) had the highest view rate (R->V) at **95.9%**.
    * **Worst Channel**: The 2-channel combo (`web`, `email`) had a dismal view rate of **33.8%**.
      
  <img width="718" height="208" alt="image" src="https://github.com/user-attachments/assets/398f0f15-fc91-459d-8311-d4ae64c402da" />

* **Best vs. Worst Offer**:
    * **Best**: `discount-10-2-10` (65.1% R->C rate) and `discount-7-3-7` (61.9% R->C rate). These offers were persuasive (high V->C) and sent on effective channels (high R->V).
    * **Worst**: `discount-20-5-10` (19.5% R->C rate). This was not a bad offer (57.6% V->C rate), but it was a **marketing failure**. It was sent on the worst-performing channel combo (`web`, `email`), so 66% of customers never even saw it.

## Visualizations

* **Histograms**: Customer Age and Income distributions.
* **Count Plots**: Customer distribution by `age_group` and `income_group`.
* **Heatmap (Crosstab)**: A 2D grid showing customer concentration by Age Group vs. Income Group.
* **Bar Plot (Funnel)**: A funnel plot visualizing the drop-off from 'Received' to 'Viewed' to 'Completed'.
  
  <img width="917" height="614" alt="image" src="https://github.com/user-attachments/assets/7149c88c-b167-4412-9cf6-40bd21f8e865" />
  
* **Bar Plot (Offer Performance)**: A horizontal bar chart ranking all BOGO/Discount offers by their final Received-to-Completed (R->C) conversion rate.
  
  <img width="1099" height="609" alt="image" src="https://github.com/user-attachments/assets/87404636-565c-443f-8e50-816b64704a74" />
  
* **Bar Plots (Segment Spend)**: Average offer-driven transaction value broken down by Age Group and Income Group.
  
  <img width="839" height="547" alt="image" src="https://github.com/user-attachments/assets/794fb775-f262-4820-8429-1fab8e7aaefb" />
  
* **Heatmap (Preference)**: A 2D grid showing the View-to-Completed (V->C) conversion rate, segmenting by Age Group and Offer Type (BOGO vs. Discount).
  
  <img width="951" height="699" alt="image" src="https://github.com/user-attachments/assets/8ec31744-0580-45ea-936a-f2c8a8b9d8b2" />


## Key Findings & Strategic Recommendations

1.  **Prioritize Discount Offers**: Discounts are demonstrably more persuasive (higher V-to-C rate) than BOGO offers across all age segments. BOGO offers, especially those with high spend requirements ($10), perform poorly.
    * **Action**: Shift marketing spend towards discount-based promotions.

2.  **Maximize Channel Exposure**: The single biggest point of failure is "Discovery" (23.8% of offers are never seen). This is purely a channel problem.
    * **Action**: Use the 4-channel combination (`web`, `email`, `mobile`, `social`) for all high-priority offers. Avoid using the 2-channel (`web`, `email`) combo, as it was responsible for the worst-performing campaign.

3.  **Target High-Value Segments**: The most valuable customers (highest average spend per offer) are **High-Income earners** ($29.80), **Seniors** ($22.62), and **Older Adults** ($22.52).
    * **Action**: Target these high-value segments with the most effective offers (`discount-10-2-10` and `discount-7-3-7`) to maximize ROI.

4.  **Re-evaluate Low-Performing Offers**: The `discount-20-5-10` offer failed due to poor channel selection, not a bad promotion.
    * **Action**: Re-test this offer using the 4-channel combo; its high V-C rate (57.6%) suggests it will be a strong performer if seen.

## Tools & Technologies Used

* **Python 3**
* **Pandas**: For data manipulation, cleaning, and aggregation.
* **NumPy**: For numerical operations.
* **Matplotlib & Seaborn**: For data visualization (bar plots, histograms, heatmaps).
* **Jupyter Notebook**: For interactive analysis and reporting.

## How to Run the Notebook

To replicate this analysis, follow these steps:
1.  Clone this repository.
2.  Ensure you have the required Python libraries installed (`pandas`, `numpy`, `matplotlib`, `seaborn`).
3.  Place the three CSV files (`cleaned_customer_data.csv`, `cleaned_events.csv`, `cleaned_offers.csv`) in the same directory as the notebook.
4.  Run the `Final Analysis.ipynb` notebook from start to finish.

## Author

* Irfan Zim.
