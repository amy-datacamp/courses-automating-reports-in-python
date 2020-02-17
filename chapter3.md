---
title: 'Building HTML and PDF reports in Python'
description: 'This lesson teaches how to build html and pdf reports in Python'
---

## Create an automated PDF Report - Part I

```yaml
type: NormalExercise
key: 8282a22196
xp: 100
```

The most preferred method of report generation for a Data Scientist is a PDF report. In this 3-part capstone exercise, we are going to use the following process flow to create a multi-page PDF document using Pandas, Jinja2 and pdfkit

![https://assets.datacamp.com/production/repositories/5657/datasets/d30c50bc96e3595d3426d00d35dbe8ad85baaaa1/pdf-conversion-pipeline-ud.png](pipeline)

Let’s say you want to print PDFs of tables that show the growth of a bank account. Each table shows the growth rate year by year of $100, $500, $20,000, and $50,000 dollars. Each separate PDF report uses a different interest rate to calculate the growth rate. We will need 5 different reports, each of which prints tables with 1%, 2%, 3%, 4%, 5% interest rates, respectively.

First, we will create the list of pandas dataFrames each in turn contains the DataFrames corresponding to each of the 5 interest sizes

`@instructions`
- Create a list containing the five different interest rates (1%, 2%, 3%, 4%, 5%) and four different account sizes (100, 500, 20000, 50000) and assign it to the initial_account_sizes
- Use a for loop to create a list of DataFrames in which each column of each of the DataFrame has the growth rate year by year information
- Append each of the individual DataFrames to the data_frames list

`@hint`
- List of interest rates and account sizes should correspond to `1%, 2%, 3%, 4%, 5%` and `100, 500, 20000, 50000` respectively
- Growth rate year by year is calculated as `initial_account_size * (1 + interest_rate) ** year`
- Use Python's list's `append` method to append the dataframes to the list

`@pre_exercise_code`
```{python}
import pandas as pd

interest_rates = [i*.01 for i in range(1,6)]

initial_account_sizes = [100, 500, 20000, 50000]

data_frames = []
for interest_rate in interest_rates:
    df = {}
    for initial_account_size in initial_account_sizes:
        df['Account Size: ' + str(initial_account_size)] = [initial_account_size * (1 + interest_rate) ** year for year in range(1, 21)]
    df = pd.DataFrame(df)
    df.index.name = 'year'
    data_frames.append({'df':df,
        'interest_rate':interest_rate})
```

`@sample_code`
```{python}
- Create a list containing the five different interest rates (1%, 2%, 3%, 4%, 5%) and four different account sizes (100, 500, 20000, 50000)
interest_rates = ____
initial_account_sizes = ____

- Use a for loop to create a list of DataFrames in which each column of each of the DataFrame has the growth rate year by year information
for interest_rate in interest_rates:
    df = ____
    for initial_account_size in ____:
        ____['Account Size: ' + str(initial_account_size)] = [____ * (1 + ____) ** year for year in range(1, 21)]
    df = pd.DataFrame(____)
	____.index.name = 'year'
    
- Append each of the individual DataFrames to the data_frames list
data_frames = ____
data_frames.____({'df':____,'interest_rate':____})
```

`@solution`
```{python}
- Create a list containing the five different interest rates (1%, 2%, 3%, 4%, 5%) and four different account sizes (100, 500, 20000, 50000)
interest_rates = [i*.01 for i in range(1,6)]
initial_account_sizes = [100, 500, 20000, 50000]

- Use a for loop to create a list of DataFrames in which each column of each of the DataFrame has the growth rate year by year information
for interest_rate in interest_rates:
    df = {}
    for initial_account_size in initial_account_sizes:
        df['Account Size: ' + str(initial_account_size)] = [initial_account_size * (1 + interest_rate) ** year for year in range(1, 21)]
    df = pd.DataFrame(df)
	df.index.name = 'year'
    
- Append each of the individual DataFrames to the data_frames list
data_frames = []
data_frames.append({'df':df,'interest_rate':interest_rate})
```

`@sct`
```{python}
Wonderful!!! You have successfully created a list of DataFrames for each of the 5 interest rates. Let us create a HMTL template in the next capstone exercise
```

---

## Create an automated PDF report - Part II

```yaml
type: NormalExercise
key: e75b38a431
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{python}

```

`@sample_code`
```{python}

```

`@solution`
```{python}

```

`@sct`
```{python}
# Examples of good success messages: https://instructor-support.datacamp.com/en/articles/2299773-exercise-success-messages.
```
