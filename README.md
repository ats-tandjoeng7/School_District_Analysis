# School District Analysis
This project exposed us to the power of the popular pandas package, which is integrated in python-conda environment, and how we can leverage many of its useful functions for performing efficient data analytics and data visualization.

## Table of Contents
- [Overview of Project](#overview-of-project)
  - [Resources](#resources)
  - [Challenge Overview](#challenge-overview)
- [Results and Summary](#results-and-summary)
  - [Deliverable 1 and 2 Summary](#deliverable-1-and-2-summary)
  - [Deliverable 3 Summary](#deliverable-3-summary)
  - [Deliverable 4 Summary](#deliverable-4-summary)
  - [Deliverable 5 Summary](#deliverable-5-summary)
- [References](#references)

## Overview of Project
Module 4 assignment triggered some rigorous exercises for understanding Python coding concepts, especially data manipulation by leveraging pandas and conda environment, and how we further apply some of its useful module, functions, and data structures to perform analysis and visualization of the student data more efficiently. We were required to supply some in-depth data analytics to the chief data scientist for a city school district, including the statistics of student funding and students' standardized test scores.

### Resources
- Data Source: new_full_student_data.csv
- Source Code: Student_Data_Challenge.ipynb
- Output File: new_full_student_data_clean.csv
- Software: [conda 4.14.0](https://github.com/conda/conda/releases), [Python 3.9.12](https://docs.python.org/release/3.9.12/), [Visual Studio Code 1.71.1](https://code.visualstudio.com/updates/v1_71), or their newer releases

### Challenge Overview
Outline of our deliverables and a written report:

- ☑️ Deliverable 1: Collect the student data into a DataFrame.
- ☑️ Deliverable 2: Prepare a cleaned version of the DataFrame.
- ☑️ Deliverable 3: Summarize key pieces of the data.
- ☑️ Deliverable 4: Drill down into the data to analyze specific subsets.
- ☑️ Deliverable 5: Compare and contrast the data through grouping and aggregation functions.
- ☑️ Deliverable 6: A written analysis of your results (this ["README.md"](./README.md)).

All deliverables in Module 4 challenge are committed in this GitHub repo as outlined below.  
main branch  
|&rarr; [./README.md](./README.md)  
|&rarr; [./Student_Data_Challenge.ipynb](./Student_Data_Challenge.ipynb)  
|&rarr; ./Resources/  
  &emsp; |&rarr; [./Resources/new_full_student_data.csv](./Resources/new_full_student_data.csv)  
  &emsp; |&rarr; [./Resources/new_full_student_data_clean.csv](./Resources/new_full_student_data_clean.csv)  

## Results and Summary
Let us assess our results and summary that meet the requirements of each deliverable in the following sections.

### Deliverable 1 and 2 Summary
All data collection, preparation and verification steps are properly done by using `os.path.join()`, `pd.read_csv()`, etc. In Module 4 assignment, we imported student data as a pandas DataFrame. A few examples that might be unique when preparing and cleaning the data are summarized below.

- After data cleaning steps, I copied the original `student_df` DataFrame as `cln_df`, a step that may be different from others'. I then crunched the clean version of the DataFrame based on `cln_df`. This allowed me to save the clean datasets and validate the accuracy of our data manipulation steps when necessary.
- Added a few checkpoints for making sure every step was accurately performed. I mainly used `display()` for printing the analysis results.
  - 2847 lines containing at least one null were removed.
  - 1836 duplicated rows were trimmed.

```
# Drop duplicated rows and verify removal
cln_df = cln_df.drop_duplicates()
duplicate_rows = cln_df.duplicated().sum()
display("Duplicate rows=", duplicate_rows)
```

```
# use np.int64 instead of int to align with image in the instruction
cln_df['grade'] = cln_df['grade'].astype(np.int64)
```

### Deliverable 3 Summary
The average math score for all students was **64.67573326141189**, which matched with the statistics displayed by `describe()`. We calculated the max reading and min reading scores by applying `max()` and `min()` functions, and stored the max_reading_score as well as min_reading_score (**10.5**) values for later use.

### Deliverable 4 Summary
We applied either `loc` or `iloc` functions to analyze specific subsets based on certain condition(s). We also used Boolean operators, like `&`, `|`, or `~`, to filter, index or slice, and select or group specific subsets. We were able to analyze all the required datasets and our results matched well with the reference data.

The mean reading score for all students in grades 11 and 12 combined was **74.900381**. I adopted a simplified conditional statement after verifying that there were no grades beyond grade 12<sup>th</sup> in our datasets.

```
# Find the mean reading score for all students in grades 11 and 12 combined.
ave_reading_score_gr1112 = cln_df.loc[(cln_df['grade'] >= 11), ['reading_score']].mean()
ave_reading_score_gr1112
```

### Deliverable 5 Summary
Since we noticed two conflicting versions of deliverables, I printed reading and math scores in addition to the requested school budget by school type altogether. To assure our results, I also used `loc` along with conditional statements to reconfirm that our data and calculation were solid. Now I feel comfortable providing whatever data immediately that the chief data scientist for a city school district may further inquire.

- Average test scores and school funding per school type are shown in **Table 1**. Overall test scores were slightly higher for Charter schools in general even though they received lower funding.
- Average test scores and school funding by grade level are displayed in **Table 2** for comparison purposes. Math scores were in decreasing trends as grades moved from lower to higher.
- To align with the best practice approach, school budget was rounded to the nearest whole cent in both tables by applying extra `map()` formatting.

<b>Table 1. Average Test Scores and Funding by School Type</b>

| school_type    | reading_score | math_score | school_budget |
| :---           |  ---:         |  ---:      |  ---:         |
| <b>Charter</b> | 72.450603     | 66.761883  | \$872,625.66  |
| <b>Public</b>  | 72.281219     | 62.951576  | \$911,195.56  |

<b>Table 2. Average Test Scores and Funding by Grade</b>

| grade | reading_score | math_score | school_budget |
| :---  |  ---:         |  ---:      |  ---:         |
|  9    | 69.236713     | 66.585624  | \$898,692.61  |
| 10    | 71.647590     | 64.911778  | \$896,341.54  |
| 11    | 77.478417     | 64.204911  | \$885,658.60  |
| 12    | 72.245103     | 62.283795  | \$891,797.76  |

The total number of students per school in descending order was calculated by using `groupby`, `count`, and `sort_values` as shown in the following short code. To retain the clean DataFrame, I added a column named *student_count*, which contained an exact copy of student counts from column *student_id* or any existing column besides *school_name* (**Table 3**).

```
# Use the `groupby`, `count`, and `sort_values` functions to find the
# total number of students at each school and sort from most students to least students.
count_by_school = cln_df.groupby('school_name').count()
count_by_school['student_count'] = count_by_school['student_id']
count_by_school.loc[:, ['student_count']].sort_values(by='student_count', ascending = False)
```

<b>Table 3. Total Number of Students per School</b>

| school_name            | student_count |
| :---                   |          ---: |
| Montgomery High School | 2038 |
| Green High School      | 1961 |
| Dixon High School      | 1583 |
| Wagner High School     | 1541 |
| Silva High School      | 1109 |
| Woods High School      | 1052 |
| Sullivan High School   |  971 |
| Turner High School     |  846 |
| Bowers High School     |  803 |
| Fisher High School     |  798 |
| Richard High School    |  551 |
| Campos High School     |  541 |
| Odonnell High School   |  459 |
| Campbell High School   |  407 |
| Chang High School      |  171 |

The average math scores (and school budget) for each combination of grade and school type, which were analyzed by using `groupby` and `mean`, are shown in **Table 4** and **Table 5**. **Table 5** showed the results when we added `apply(np.round)`, such that numbers matched well with image in the instruction of Module 4 assignment.

```
# Use np.round to match image in the instruction
ave_by_schooltype_grade = cln_df.groupby(['school_type', 'grade']).mean().apply(np.round)
display(ave_by_schooltype_grade.loc[:, ['math_score']])
```

<b>Table 4. Average Math Scores and School Budget per Grade for each School Type</b>

| school_type    | grade | math_score | school_budget  |
| :---           | ---:  | ---:       | ---:           |
| <b>Charter</b> | 9     | 70.1       | \$863,817.29   |
|                | 10    | 66.4       | \$871,823.61   |
|                | 11    | 68.0       | \$874,262.71   |
|                | 12    | 60.2       | \$885,096.34   |
| <b>Public</b>  | 9     | 63.8       | \$926,800.16   |
|                | 10    | 63.8       | \$914,715.36   |
|                | 11    | 59.3       | \$900,248.91   |
|                | 12    | 63.6       | \$895,952.92   |

<b>Table 5. Average Math Scores per Grade for each School Type</b>

| school_type    | grade | math_score |
| :---           | ---:  | ---:       |
| <b>Charter</b> | 9     | 70.0       |
|                | 10    | 66.0       |
|                | 11    | 68.0       |
|                | 12    | 60.0       |
| <b>Public</b>  | 9     | 64.0       |
|                | 10    | 64.0       |
|                | 11    | 59.0       |
|                | 12    | 64.0       |

As a final checkpoint, I exported the clean data into a csv file and rerun the refactored Jupyter Notebook code for making sure everything has been analyzed correctly.

```
# Export the clean data without row index
student_data = os.path.join('./Resources', 'new_full_student_data_clean.csv')
cln_df.to_csv(student_data, index=False)
```

## References
[Pandas User Guide](https://pandas.pydata.org/pandas-docs/stable/user_guide/index.html#user-guide)\
[Label vs Location](https://stackoverflow.com/questions/31593201/how-are-iloc-and-loc-different)
