
# Student Engagement Data Analysis Pipeline
This project contains Python code for cleaning, transforming, and performing exploratory data analysis (EDA) on three related CSV datasets: `enrollments`, `daily_engagement`, and `project_submissions`.

The primary goal of this analysis is to compare the first-week engagement metrics of students who eventually pass a core project (the "Subway Project") against those who do not, to identify potential early indicators of student success.

##📁 Data SourcesThe analysis relies on three distinct datasets, assumed to be in CSV format and located in the project directory:

1. `enrollments.csv`: Contains student enrollment records, including join and cancellation dates.
2. `daily_engagement.csv`: Contains daily records of student interaction with the course (minutes spent, lessons completed).
3. `project_submissions.csv`: Contains records of student project submissions and assigned ratings.

## Key Steps in the Analysis
###1. Data Loading and Cleaning* 
**Loading:** Data is loaded from CSV files into Python lists of dictionaries using `csv.DictReader`.
* **Data Type Fixing:** Custom functions (`parse_date`, `parse_maybe_int`) are used to convert string representations of dates, integers, and booleans into proper Python types (`datetime`, `int`, `float`, `bool`) across all three datasets.
* **Consistency:** The key field in `daily_engagement` is renamed from `'acct'` to `'account_key'` for consistency.

###2. Data Filtering and Problem Identification* 
**Unique Students:** The total number of unique students across all three datasets is calculated.
* **Missing Engagement Records:** Identifies students who enrolled but have no recorded engagement data, helping to pinpoint data integrity issues.
* **Udacity Test Accounts:** Udacity's internal test accounts are identified and systematically **removed** from all datasets to prevent data skewing.
* **Free Trial Filters:** Students who canceled their enrollment within the first 7 days (free trial period) are **removed**, creating the final `paid_students` set for the core analysis.

###3. Feature Engineering and Grouping* 
**First-Week Engagement:** A custom function `within_one_week` is created to filter engagement records, keeping only activities that occurred within the first 7 days of a student's most recent enrollment date.
* **Binary Feature:** A new binary feature, `'has_visited'`, is added to the engagement data (1 if `num_courses_visited > 0`, 0 otherwise).
* **Data Aggregation:** Functions (`group_data`, `sum_grouped_items`) are implemented to aggregate first-week data (total minutes, lessons completed, days visited) by `account_key`.

###4. Exploratory Data Analysis (EDA)
The core analysis compares two groups of students based on their future success:

* **Passing Students:** Students who eventually submitted and passed the "Subway Project" (identified by specific `lesson_key` IDs and `'PASSED'` or `'DISTINCTION'` ratings).
* **Non-Passing Students:** Students who did not meet the passing criteria for the Subway Project.

Metrics are computed and visualized for both groups:

| Metric | Purpose |
| --- | --- |
| `total_minutes_visited` | Total time spent in the course. |
| `lessons_completed` | Number of lessons finished. |
| `has_visited` (Days Visited) | Number of days with at least one course visit. |

Visualization using `matplotlib` and `seaborn` is employed to compare the distribution of these metrics between the two groups, providing evidence for how early engagement patterns correlate with later project success.

## Dependencies
This analysis requires the following Python libraries:

* `csv` (Standard library)
* `datetime` (Standard library)
* `collections` (Standard library for `defaultdict`)
* `matplotlib.pyplot`
* `numpy`
* `seaborn`

```bash
pip install numpy matplotlib seaborn

```
