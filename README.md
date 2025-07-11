![](./README_files/img/fig0.png)

# InferBlock: A Visual Meta-Programming Framework for Divisible and Transparent Data Analysis Pipelines


## Untangling Data: A Scientist's Guide to InferBlock vs. Manual Excel Methods

For scientists accustomed to wrangling data in spreadsheets, the prospect of learning a new programming language like R can be daunting. However, for many data manipulation tasks, **InferBlock** offers a user-friendly, more efficient and reproducible workflow compared to manual approaches in Excel. 
This guide provides direct comparisons of common tasks – e.g. calculating group means – to illustrate the power and simplicity of InferBlock.

### Group Averages
#### The Scenario: Calculating Group Averages

Imagine you have experimental data in a simple table with three columns: 'x' (e.g., sample ID), 'y' (e.g., a measured value), and 'group' (e.g., the treatment group). Your goal is to calculate the average of the 'y' values for each group and add this information as a new column, 'y_mean', to your original dataset.

**Sample Dataset:**

| x | y | group |
|---|---|---|
| 1 | 10 | A |
| 2 | 12 | A |
| 3 | 14 | A |
| 4 | 20 | B |
| 5 | 22 | B |
| 6 | 24 | B |
| 7 | 15 | C |
| 8 | 17 | C |
| 9 | 19 | C |


#### The "Naive" Excel Approach

For an Excel user unfamiliar with advanced functions like Pivot Tables, this task becomes a multi-step, manual process prone to errors:

1.  **Sort by Group:** First, you would sort the entire table by the 'group' column to bring all the 'A's, 'B's, and 'C's together.
2.  **Calculate Group Means Separately:** You would then select the 'y' values for the first group (A), and in an empty cell, use the `AVERAGE` function to calculate their mean. You would repeat this for group B and group C, placing their respective means in other empty cells.
3.  **Create the New Column:** You would add a new column header, "y_mean".
4.  **Manual Copy and Paste:** Finally, you would manually copy the calculated mean for group A and paste it into the 'y_mean' column for all the rows belonging to group A. You would then repeat this tedious process for groups B and C.

This method, while functional for a small dataset, quickly becomes cumbersome and error-prone with larger and more complex datasets. Any changes to the original data would require repeating the entire process.

#### The InferBlock Approach: Using a Grammar for Data Manipulation

InferBlock provides a set of intuitive "blocks" for data manipulation. To achieve the same result as the manual Excel method, you would nest concise and readable visual blocks, where data flows from the innermost block outwards, with each containing block applying its function to the result of the one inside it. 

Here are the nested visual blocks:
![Group average nested blocks](./README_files/img/fig1.png)

Let's break down these nested blocks:

*   `READ_EXCEL`: This starts the chain of operations on our `data`.
*   `GROUP_BY`: This is the core function. It tells to perform the next operation separately for each unique value in the 'group' column. It doesn't change the appearance of the data, but it changes how subsequent functions work.
*   `MUTATE`: The `MUTATE`block is used to create a new column. Here, we're creating a column named `y_mean`. The value of this new column is the `mean()` of the 'y' column for each group defined by `GROUP_BY`.

The result, will be:

| x | y | group | y_mean |
|---|---|---|---|
| 1 | 10 | A | 12 |
| 2 | 12 | A | 12 |
| 3 | 14 | A | 12 |
| 4 | 20 | B | 22 |
| 5 | 22 | B | 22 |
| 6 | 24 | B | 22 |
| 7 | 15 | C | 17 |
| 8 | 17 | C | 17 |
| 9 | 19 | C | 17 |

#### Side-by-Side Comparison

| Feature | Naive Excel Approach | InferBlock Approach |
| :--- | :--- | :--- |
| **Steps** | Multiple manual steps (sort, calculate, copy, paste) | A single, visual nested block |
| **Reproducibility** | Difficult to reproduce accurately; prone to human error | Fully reproducible; the visual is a clear record of the steps |
| **Scalability** | Becomes extremely time-consuming and error-prone with large datasets | Handles large datasets efficiently |
| **Readability** | The process is not self-documenting | The visual is clear and easy to understand, following a "grammar" of data manipulation |
| **Updating** | Requires repeating the entire process if data changes | Simply re-run InferBlock to update the results with new data |