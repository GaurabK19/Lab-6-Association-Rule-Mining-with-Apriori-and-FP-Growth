## Purpose of the Lab

The goal of this lab is to apply association rule mining techniques to a real transactional dataset and compare two classic algorithms: **Apriori** and **FP-Growth**. Using the Book-Crossing ratings data, I built a user–item transaction matrix, mined frequent itemsets under different constraints, and then generated association rules characterized by support, confidence, and lift.

## Approach and Methods

1. **Data preparation and exploration**
   - Loaded the `BX-Book-Ratings.csv` file and kept only **explicit ratings** (`Book-Rating > 0`).
   - Restricted the analysis to the top *N* most frequently rated books to keep the transaction matrix tractable while preserving the most informative items.
   - Built a **user × ISBN** basket matrix
   - Created exploratory visualizations:
     - A Seaborn barplot of the most frequently rated books.
     - A co-occurrence heatmap of the top books to highlight which titles tend to appear together in user baskets.

2. **Frequent itemset mining**
   - Ran the **Apriori** algorithm with a chosen minimum support threshold to identify frequent itemsets and visualized the top 1- and 2-itemsets by support.
   - Ran **FP-Growth** with the **same support threshold** on the same transaction matrix.
   - Compared the number of itemsets of each length and confirmed that both algorithms produced essentially the same patterns, as expected.

3. **Association rules**
   - Generated association rules from both Apriori and FP-Growth frequent itemsets using a minimum confidence threshold.
   - Calculated key metrics for each rule:
     - **Support**: how often the full rule appears in the data.
     - **Confidence**: how often the consequent appears given the antecedent.
     - **Lift**: how much more often the items occur together than if they were independent.
   - Filtered for “strong” rules (lift > 1) and plotted **confidence vs lift** using Seaborn scatter plots to visually identify higher-confidence, higher-lift rules.

4. **Comparative analysis**
   - Constructed a summary table with:
     - number of frequent itemsets for each algorithm,
     - number of strong rules, and
     - runtime in seconds for Apriori and FP-Growth.
   - Plotted a runtime bar chart to compare efficiency and discuss why FP-Growth is generally expected to be faster.

## Key Insights from the Analysis and Rules

- The dataset is highly **sparse** and skewed: a small number of books receive a
  very large number of ratings, while most books are rated infrequently. This
  has a direct impact on how many frequent itemsets and rules can be discovered
  at a given support threshold.
- Frequent itemsets and association rules are dominated by the **most popular
  books**. These titles co-occur in user baskets more often than would be
  expected by chance, producing rules with lift values slightly above 1.
- If a user has rated one popular book, they are somewhat more likely to have rated another specific book. These relationships could be used as a simple recommendation signal alongside other features.
- Both Apriori and FP-Growth find essentially the **same frequent itemsets** and rules when given the same transaction matrix and support threshold. The main  difference between them in this lab is **runtime behavior**, not the discovered patterns.

## Challenges and Key Decisions

Throughout the lab, several practical challenges had to be addressed:

1. **Environment and library installation**
   - The first attempt to import `mlxtend` raised a `ModuleNotFoundError`, and `pip` was not recognized in the terminal. I resolved this by using `pip3` and installing `mlxtend` under the correct Python interpreter.
   - Once `mlxtend` was installed, the Apriori and FP-Growth functions could be imported and used directly in the notebook.

2. **Selecting support and confidence thresholds**
   - With higher minimum support and confidence, there were almost no 2-itemsets and very few association rules, which produced nearly empty plots and made analysis uninformative.
   - I experimented with lower support and confidence values until I reached a balance where there were enough patterns to visualize while still focusing on relationships that were not purely random noise.

3. **Handling empty or misleading visualizations**
   - At one stage, the confidence–lift scatter plots rendered without any points because all rules had been filtered out by strict lift thresholds. This was corrected by slightly relaxing the lift