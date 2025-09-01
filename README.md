# Maven Cafe Rewards: 30-Day Offer Test — Results & Recommendations

## Executive Summary

Over a 30-day period, Maven Cafe tested different types of offers with Rewards members. The results were clear: **offers drive bigger baskets and more revenue, even though they account for fewer transactions overall.**

Key insights:

* **Older Adults and Seniors** were the most engaged and delivered the highest value.
* **High-income customers** purchased less often but spent the most per visit.
* **Women spent more per customer than men**, even though men made more transactions.
* **Best-performing offers**: `discount-10-2-10` and `discount-7-3-7`.
* **Most effective communication strategy**: using **all four channels** (web, email, mobile, social).

These findings give us a clear roadmap to optimize future campaigns.

---

## 🎯 Project Goal

The goal of this project was to answer three main questions:

1. **Which customers respond best to offers?**
2. **What types of offers work best?**
3. **How should we reach them to maximize impact?**

Insights from this test will guide **future promotional campaigns** to bring in more revenue and improve customer loyalty.

---

## 🛠 Data Preparation & Approach

This is a project from Maven Analytics (https://mavenanalytics.io/challenges/maven-rewards-challenge)

* **Datasets Used**:
  
  1. Customer demographics
  2. Offer details
  3. Customer activities (offer received, viewed, completed, and transactions)

* **Cleaning Steps**:

  * Fixed missing values
  * Removed extreme outliers (e.g., age 118)
  * Grouped customers into **age cohorts** (18–34, 35–49, 50–64, 65–79, 80–110)
  * Grouped customers into **income cohorts** (Low: 0–44k, Middle: 44–84k, High: 84k+)

* **Offer-to-transaction rule**:
  A transaction was counted as **offer-related only if it occurred at the same time as the offer completion event.**

  * This conservative rule ensured accuracy and avoided over-crediting offers.

---

## 📊 Key Results

1. **Offer transactions = 24%** of total transactions but made up **35% of revenue**.
2. **Average transaction size**:

   * With offers: **\$20.50**
   * Without offers: **\$12**
     
3. **Age cohorts**:

   * Seniors (65–79): Avg \$23 per transaction with offers
   * Older Adults (50–64): Avg \$22 per transaction with offers
   * Elderly (80–110): Smaller group but high spend (**\$56 per customer**)
     
4. **Income cohorts**:

   * High-income (\$84k+): Fewer purchases but highest spend (**\$78 per customer, \$29 per transaction**)
   * Middle-income (\$44–84k): Completed the most offers (19403) overall.
     
5. **Gender**:

   * Men completed slightly more offers (51% vs. 48%),
   * But women spent **more per customer** (\$58 vs. \$44.5).
     
6. **Best offers**:

   * `discount-10-2-10`: **74.5% completion**
   * `discount-7-3-7`: **72.8% completion**
   * Weak: BOGOs and `discount-20-5-10`.
     
7. **Channels**:

   * Campaigns sent via all four channels had **65% completion**.
     
8. **Customer behavior**:

   * Avg wait to view: **3 days after receiving** an offer
   * Avg time to purchase after viewing: **5 hours**
   * Some customers completed up to **6 offers** in 30 days
     
9. **Reward-to-sales ratio**: **26%** (for every \$1 generated, \$0.26 went to rewards).

---

## ✅ Recommendations

1. **Target Age Groups**:

   * Focus on **Older Adults (50–64) and Seniors (65–79)** → high engagement & strong spending.
   * Don’t ignore Elderly (80–110) → high transaction values; use email & in-store promos instead of app/social.

2. **Income Strategy**:

   * High-income (\$84k+): Premium bundles & exclusive offers.
   * Middle-income (\$44–84k): Simple, steady discounts for consistent traffic.

3. **Gender Strategy**:

   * Women: Loyalty rewards & curated bundles.
   * Men: Maintain transaction-driving offers but push value per basket.

4. **Offer Strategy**:

   * Make `discount-10-2-10` the **main offer**.
   * Use `discount-7-3-7` as a **strong secondary**.
   * Reduce focus on BOGOs and `discount-20-5-10`.

5. **Channel Strategy**:

   * Use **all four channels** for big campaigns.
   * Segment channel mix by age group (younger: social & mobile; older: email & in-store).

6. **Engaging Younger Customers**:

   * Small referral rewards
   * Social-sharing incentives
   * Experience-driven promotions

---

## 🧰 Tech Stack

* **Python**: Pandas, NumPy
* **Jupyter Notebook** for data exploration & analysis
* **Excel/CSV** for data sources
