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

![pipeline](https://assets.datacamp.com/production/repositories/5657/datasets/d30c50bc96e3595d3426d00d35dbe8ad85baaaa1/pdf-conversion-pipeline-ud.png)

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

In the previous exercise, we have created a list of DataFrames with each DataFrame contains values of each of the 5 interest sizes. Next, we will create the Jinja2 template file. We will generate one report for each of the 5 DataFrames with the same Jinja2 template file, and generate them by passing each data frame to the template along with the interest rate used.

To populate those variable, we need to first create a Jinja environment and get our template. After creating this template, we then write a simple Python code to produce 5 HTML files for our reports. This creates 5 html reports corresponding for each of the interest rate.

Note: The `myreport.html` template and `data_frames` variables have been loaded for you

`@instructions`
- Create a Jinja environment and get our template
- Give a name called "pdf_interest_report.html" to the html
- Populate the variables in the jinja2 template and render the template into HTML

`@hint`
- Watch the video on how to create a jinj2 environment and prepare the template 
- Make sure you assign the name "pdf_interest_report.html" to the template_file
- Jinja2 has a method called `render` for rendering to html

`@pre_exercise_code`
```{python}
import pandas as pd
import jinja2

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

templateLoader = jinja2.FileSystemLoader(searchpath="./")
templateEnv = jinja2.Environment(loader=templateLoader)
template_file = "pdf_interest_report.html"
template = templateEnv.get_template(template_file)

for d in data_frames:
    outputText = template.render(df=d['df'],
            interest_rate=d['interest_rate'])
    html_file = open(str(int(d['interest_rate'] * 100)) + '.html', 'w')
    html_file.write(outputText)
    html_file.close()
```

`@sample_code`
```{python}
- Create a Jinja environment and get our template
templateLoader = jinja2.FileSystemLoader(searchpath="./")
templateEnv = jinja2.Environment(loader=____)

- Give a name called "pdf_interest_report.html" to the html
template_file = ____
template = templateEnv.get_template(____)

- Populate the variables in the jinja2 template and render the template into HTML 
for d in ____:
    outputText = template.____(____=d['df'],
            interest_rate=____['interest_rate'])
    html_file = open(str(int(____['interest_rate'] * 100)) + '.html', 'w')
    html_file.____(____)
    html_file.close()
```

`@solution`
```{python}
- Create a Jinja environment and get our template
templateLoader = jinja2.FileSystemLoader(searchpath="./")
templateEnv = jinja2.Environment(loader=templateLoader)

- Give a name called "pdf_interest_report.html" to the html
template_file = "pdf_interest_report.html"
template = templateEnv.get_template(template_file)

- Populate the variables in the jinja2 template and render the template into HTML 
for d in data_frames:
    outputText = template.render(df=d['df'],
            interest_rate=d['interest_rate'])
    html_file = open(str(int(d['interest_rate'] * 100)) + '.html', 'w')
    html_file.write(outputText)
    html_file.close()
```

`@sct`
```{python}
Good job on creating the Jinja2 template and converting it to html. Get ready to turn this into beautiful pdf reports in the next exercise
```

---

## Create an automated PDF report - Part III

```yaml
type: NormalExercise
key: c5e9f9ac74
xp: 100
```

The final step of the creating an automated PDF report in Python is that we need to convert the HTML files that we created to PDFs. To do this, we use `pdfkit`. All you have to do is iterate through your HTML files and then use a single line of code from `pdfkit` to each file to convert it into a PDF. All of this code combined will pop out the following HTML files with PDF versions.

Please note that the html files that you have created in previous exercise have already been loaded for you.

`@instructions`
- Import the pdfkit package into Python environment
- Use a for loop to convert html documents to pdf documents each time changing the html suffix to pdf suffix

`@hint`
- pdfkit can be imported using `pdfkit` package
- There are 5 html files that you have created earlier. Check the video on converting html to pdf in pdfkit package

`@pre_exercise_code`
```{python}
import pandas as pd
import pdfkit
import jinja2

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

templateLoader = jinja2.FileSystemLoader(searchpath="./")
templateEnv = jinja2.Environment(loader=templateLoader)
template_file = "pdf_interest_report.html"
template = templateEnv.get_template(template_file)

for d in data_frames:
    outputText = template.render(df=d['df'],
            interest_rate=d['interest_rate'])
    html_file = open(str(int(d['interest_rate'] * 100)) + '.html', 'w')
    html_file.write(outputText)
    html_file.close()
    
for i in range(1,6):
    pdfkit.from_file(str(i) + '.html', str(i) + '.pdf')
```

`@sample_code`
```{python}
- Import the pdfkit package into Python environment
import pdfkit

- Use a for loop to convert html documents to pdf documents each time changing the html suffix to pdf suffix
for i in range(1,____):
    pdfkit.____(str(i) + ____, str(i) + ____)
```

`@solution`
```{python}
- Import the pdfkit package into Python environment
import pdfkit

- Use a for loop to convert html documents to pdf documents each time changing the html suffix to pdf suffix
for i in range(1,6):
    pdfkit.from_file(str(i) + '.html', str(i) + '.pdf')
```

`@sct`
```{python}
Execellent work on automating the PDF reporting in Python. Now you can go ahead and play with templates and generate even beautiful pdf reports
```
