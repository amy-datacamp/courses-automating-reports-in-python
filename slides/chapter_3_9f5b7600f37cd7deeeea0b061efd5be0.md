---
key: 9f5b7600f37cd7deeeea0b061efd5be0
title: 'Insert title here'
---

## Create an automated PDF report using Jinja2 and pdfkit

```yaml
type: TitleSlide
key: 57a490b043
```

`@lower_third`
name: Upendra Devisetty
title: Data Scientist, Greenlight Biosciences

`@script`
Automated report generation takes away the pain of manually generating reports. In this video, we will explore how we can automate the process of report generation using a simple python script

---

## Need for an automated PDF report generation

```yaml
type: FullSlide
key: 4cb6ae0739
```

`@part1`
- Automated Excel reporting with Python {{1}}

- PDF format is the preferred method of reporting in Data Science {{2}}

- Open source tools to aid in automated pdf report generation in Python: {{3}}

	* Pandas {{4}}
    
    * Jinja2 {{5}}
    
    * pdfkit {{6}}

`@script`
In the previous chapter, we looked at how we can automate Excel reporting using Python. However, the most preferred method of report generation for a Data Scientist is a PDF report. In order to do that, we will use the open-source tools which you have been introduced to in the previous lessons such as Pandas, Jinja2 and pdfkit.

---

## Introduction to the example data

```yaml
type: FullSlide
key: 0b641e6ccd
```

`@part1`
- Example dataset

![](https://assets.datacamp.com/production/repositories/5657/datasets/06ebc5cd542e62ecacdcc7aefbcc6feaaa09b92c/df_0_ss.png)

`@script`
Let’s say you want to print PDFs of tables that show the growth of a bank account. Each table shows the growth rate year by year of $100, $500, $20,000, and $50,000 dollars. Each separate pdf report uses a different interest rate to calculate the growth rate. We’ll need 5 different reports, each of which prints tables with 1%, 2%, 3%, 4%, 5% interest rates, respectively.

---

## Create a Jinja2 template file

```yaml
type: FullSlide
key: 40ab409493
code_zoom: 66
```

`@part1`
```
<head></head>
<body>
    <h1>Interest Rate: {{ interest_rate * 100 }}%</h1>
    <table>
        <tr>
            {% for column in df.columns %}
            <th>{{ column }}</th>
            {% endfor %}
        </tr>
        {% for idx, row in df.iterrows() %}
        <tr>
            {% for colname in df.columns %}
            <td>${{ row[colname] | round(1) }}</td>
            {% endfor %}
        </tr>
        {% endfor %}
    </table>
</body>
```


`@script`
First, we will create the Jinja2 template file. We will generate one report for each of the 5 data frames with the same Jinja2 template file, and generate them by passing each data frame to the template along with the interest rate used. 


---

## Create HTML reports using Jinja2 template

```yaml
type: FullSlide
key: bef722fe00
code_zoom: 85
```

`@part1`
```
import jinja2

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


`@script`
After creating this template, we then write this simple Python code to produce 5 HTML files for our reports. This creates 5 html reports corresponding for each of the interest rate.

---

## An example HTML report

```yaml
type: FullSlide
key: 9adc490b39
```

`@part1`
![](https://assets.datacamp.com/production/repositories/5657/datasets/9eab5a3d6554d6ded212f5b8224747865cac764b/html_report_df_0_ss.png)

`@script`
The html report looks something when you open it on a browser on on your computer

---

## Automatic conversion of HMTL to PDF

```yaml
type: FullSlide
key: 72106380d2
```

`@part1`
```
import pdfkit
for i in range(1,11):
    pdfkit.from_file(str(i) + '.html', str(i) + '.pdf')
```

`@script`
The final step of the process is that we need to convert these HTML files to PDFs. To do this, we use pdfkit. All you have to do is iterate through your HTML files and then use a single line of code from pdfkit to each file to convert it into a pdf. All of this code combined will pop out the following HTML files with PDF versions.

---

## Final script

```yaml
type: FullSlide
key: c1692b4b00
code_zoom: 40
```

`@part1`
```
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

`@script`
Here I have shown you the full script using which you can create reports using python in an automated way. Using this example and with more work, you can develop much more sophisticated reports. 

---

## Let's practice!

```yaml
type: FinalSlide
key: bdbaf150de
```

`@script`
For now, let's practice creating some automated reports on a real-world dataset now.
