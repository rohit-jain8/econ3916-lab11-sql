# SQL for Economists — Joins, Windows and the Cardinality Trap

## Objective

The objective of this lab was to understand how SQL can be used alongside pandas to analyze data, while exploring how joins, window functions, and missing values can affect statistical results.

## Methodology

- Used DuckDB to translate pandas operations, including selecting, filtering, sorting, grouping, and merging data, into SQL queries and compared the results.
- Performed a one-to-many LEFT JOIN between customer and order datasets to examine how repeated observations can affect statistical analysis.
- Conducted Welch's t-tests before and after the join to compare differences in means, standard errors, and statistical significance.
- Used SQL window functions, including `AVG() OVER (PARTITION BY)` and `RANK()`, to calculate regional averages and customer rankings without reducing the number of observations.
- Compared how SQL and pandas handle missing values when filtering data.
- Queried a public Parquet dataset containing approximately 3 million NYC taxi trips over HTTP, reading only the columns needed for the analysis.
- Used AI to translate three pandas operations into SQL, revised the prompt to address an issue with missing values, and checked the results against the original pandas operations.

## Key Findings

One of the main findings was how significantly a join can affect statistical results. The LEFT JOIN increased the dataset from 600 customer rows to 3,612 rows because customers with multiple orders appeared more than once. Before the join, the Welch t-test produced a t-statistic of 0.07 and a p-value of 0.9413. After the join, the t-statistic increased to 0.71 and the p-value decreased to 0.4749. This showed how treating repeated observations as independent can artificially reduce the standard error and change the estimated difference, even without collecting new customer information.

I also found that window functions are useful because they allow us to calculate group averages and rankings while preserving the original dataset. In this case, all 600 customer observations were retained.

Another important finding was the difference between how SQL and pandas handle missing values. When filtering for incomes not equal to 50,000, SQL returned only 3 of the 5 survey rows because comparisons involving NULL values are treated as unknown. In contrast, pandas kept all 5 rows. Adding `OR income IS NULL` to the SQL condition resolved this difference.

When working with the NYC taxi dataset, DuckDB was able to summarize approximately 3 million trips into just 6 rows without loading the entire dataset into memory at once. This demonstrated how SQL can efficiently process large datasets by reading only the necessary columns.

Finally, the AI-assisted exercise showed me why it is important to verify AI-generated code rather than assuming it is correct. The initial SQL translations matched pandas for the grouping and merging operations, but the filtering query returned only 3 rows instead of 5. After revising the prompt to clarify how missing values should be handled, all three SQL queries matched the original pandas results.
