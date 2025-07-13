[![DOI](https://zenodo.org/badge/1017868548.svg)](https://doi.org/10.5281/zenodo.15862292)
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


---

### Normalization to control
#### The Scenario: Normalizing Data to a Control Group

In many biological experiments, it's standard practice to compare the results of treatment groups to a control group. Normalization expresses each data point as a fold change relative to the control's average, making it easier to compare the effects of different treatments.

Let's use a slightly modified dataset. Our goal is to calculate the mean of the 'y' values for the control group (`ctl`) and then divide each individual 'y' value by this control mean to get a new 'y_normalized' column.

**Sample Dataset:**

| x | y | group |
|---|---|---|
| 1 | 10 | ctl |
| 2 | 12 | ctl |
| 3 | 11 | ctl |
| 4 | 22 | treat1 |
| 5 | 24 | treat1 |
| 6 | 23 | treat1 |
| 7 | 5 | treat2 |
| 8 | 6 | treat2 |
| 9 | 4 | treat2 |

#### The "Naive" Excel Approach

For a user not employing advanced functions, normalizing data in Excel is a manual and fragmented process:

1.  **Isolate the Control Group:** The user would first identify all the rows belonging to the `ctl` group.
2.  **Calculate the Control Mean:** In a separate, empty cell (say, `F1`), they would use the `AVERAGE` function to calculate the mean of the 'y' values for *only* the `ctl` rows. For our sample data, this would be `AVERAGE(B2:B4)`, which is 11.
3.  **Create the New Column:** They would add a new column header, "y_normalized".
4.  **Write the Division Formula:** In the first cell of the new column (e.g., `D2`), they would write a formula to divide the corresponding 'y' value by the control mean calculated in step 2. **Crucially**, they must use an absolute reference (`$F$1`) for the control mean so it doesn't change when the formula is dragged down. The formula in cell `D2` would be `=B2/$F$1`.
5.  **Fill Down the Formula:** The user would then click on the small square at the bottom-right of cell `D2` and drag it down to apply this normalization formula to all rows in the dataset.

This process has several pitfalls. If the location of the calculated control mean changes, all formulas break. Furthermore, ensuring the correct use of absolute (`$F$1`) versus relative (`B2`) references is a common source of errors for inexperienced users.


#### The InferBlock Approach

**InferBlock** handles this task elegantly within a single chain of blocks. The logic is clear: first, calculate the control mean, and then use that single value to transform the entire dataset.

Here are the nested visual blocks:

![](./README_files/img/fig2.png)

Let's trace the data through this pipe step-by-step:

*   `READ_EXCEL`: This starts the chain of operations on our `data`.
* `MUTATE`: This is the first transformation.
    *   It calculates the control mean `mean(y[group == "ctl"])`, which is 11.
    *   It then creates a new column called `ctl_mean` and fills **every row** of that column with the value 11.
* `MUTATE`: This is the second transformation.
    *   It takes the data frame from the previous step (which now includes the `ctl_mean` column).
    *   It creates a final column, `y_normalized`, by dividing the value in the `y` column by the value in the `ctl_mean` column for each row.

The final output, contains the intermediate column, making the calculation completely transparent.

| x | y | group | control_mean | y_normalized |
|---|---|---|---|---|
| 1 | 10 | ctl | 11 | 0.909 |
| 2 | 12 | ctl | 11 | 1.091 |
| 3 | 11 | ctl | 11 | 1.000 |
| 4 | 22 | treat1 | 11 | 2.000 |
| 5 | 24 | treat1 | 11 | 2.182 |
| 6 | 23 | treat1 | 11 | 2.091 |
| 7 | 5 | treat2 | 11 | 0.455 |
| 8 | 6 | treat2 | 11 | 0.545 |
| 9 | 4 | treat2 | 11 | 0.364 |


#### Side-by-Side Comparison

| Feature | Naive Excel Approach | InferBlock Approach |
| :--- | :--- | :--- |
| **Readability** | Poor. The logic is fragmented between a value in one cell and a formula in another. | Excellent. The nested blocks read like a narrative: "First, create a column with the control mean, then create a normalized column by dividing by it." |
| **Debugging** | Difficult. If a result is wrong, you have to manually check the formula and the referenced cell for errors. | Easy. You can run the blocks to the first `MUTATE` to check if the `ctl_mean` column was calculated correctly before proceeding. |
| **Clarity of Intent** | The purpose of the random cell with the average (`F1`) is not inherently clear. | The column name `ctl_mean` is explicit and self-documenting. The intent is unmistakable. |
| **Encapsulation** | The logic is spread out across the worksheet. | The entire logical flow is contained within a single visual nested block. |
| **Reproducibility** | Relies on manual steps that are hard to document and repeat reliably. | The visual is a perfect, repeatable recipe for the entire transformation. |

This step-by-step method within a single visual nested block is often the best of all worlds for collaborative work. It is powerful and reproducible and its explicit, sequential logic from the innermost block outwards makes it exceptionally easy for colleagues to read, understand, and trust the process.
