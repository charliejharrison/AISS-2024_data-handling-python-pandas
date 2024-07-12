---
source: Rmd
title: "Dealing with messy data"
teaching: 20
exercises: 10
questions:
- "How do I find and fix problems in my dataset?"
objectives:
- "Teach how to gather basic statistics about a DataFrame"
- "Teach how to count and filter out missing values"
- "Teach how to create basic visualisations"
keypoints:
- "Real-world datasets are often messy"
- "We can clean data and find outliers"
---

{% include links.md %}

We've covered the basic concepts of storing and accessing data in `pandas`. So far this might seem quite abstract. Now we're going to put it into practice.

Real world datasets are often - usually - nearly always? - quite messy. So much so that data scientists spend the majority of their time collecing, organising, and cleaning data [^1]. We'll download a dataset and spend some time exploring it, tidying it up, and analysing it, learning some more `pandas` functions along the way. 

## Download a data set

The website [askamanager.org](https://www.askamanager.org/) runs an annual survey, asking users about their position, location, earnings, race, gender, and education levels. 

Have a look at the 2023 survey [here](https://www.askamanager.org/2023/04/how-much-money-do-you-make-6.html). Scroll through the questions. Try filling in answers, and notice the validation (restrictions on what answers you can give). 

The results are available as a Google Sheet [here](https://docs.google.com/spreadsheets/d/1ioUjhnz6ywSpEbARI-G3RoPyO0NRBqrJnWf-7C_eirs/edit?resourcekey=&gid=1854892322#gid=1854892322). Browse the data. What do you notice? Try to familiarise yourself with some of the basic features. What data types does it contain? Can you anticipate any problems we might have with it? 

Download a copy to work on by clicking File > Download > Comma-separated values (.csv). Move the file to your data directory, and give it an easy-to-use name - I'll call it `salary_survey_2023.csv`. 

## Read the dataset into `pandas`

Let's read in the survey data. If you're starting a new session, remember to import `pandas` first. My data is stored in a directory called data, and it's a level above where I'm writing my code:

```
.
├── code
│   ├── (this is my working directory)
│   └── ...
├── data
│   ├── salary_survey_2023.csv
│   └── ...
```

So the command is:

```python
survey_data = pd.read_csv("../data/salary_survey_2023.csv")
```

So what are we dealing with here? Hopefully you had a browse of the data already. Let's check how many rows and columns we have:

```python
survey_data.shape
```

```output
(17164, 20)
```

This tells us that we have a little over 17,000 rows (the responses), and 20 columns (the questions). 

We can print the first few rows using `head()` function:

```python
survey_data.head(5)
```

```output
            Timestamp How old are you?  \
0  4/11/2023 11:02:00            35-44   
1  4/11/2023 11:02:07            25-34   

                                   Industry  \
0        Government & Public Administration   
1  Galleries, Libraries, Archives & Museums   

                     Functional area of job                 Job title  \
0              Engineering or Manufacturing        Materials Engineer   
1  Galleries, Libraries, Archives & Museums  Assistant Branch Manager   

  Job title - additional context  Annual salary (gross)  \
0                            NaN                 125000   
1                            NaN                  71000   

   Additional monetary compensation Currency Currency - other  \
0                             800.0      USD              NaN   
1                               0.0      USD              NaN   

  Income - additional context        Country       State            City  \
0                         NaN  United States  California      Ridgecrest   
1                         NaN  United States    Virginia  Fairfax County   

  Remote or on-site? Years of experience, overall  \
0            On-site                  11-20 years   
1            On-site                   8-10 years   

  Years of experience in field Highest level of education completed Gender  \
0                  11-20 years                       College degree    Man   
1                    5-7 years                      Master's degree    Man   

    Race  
0  White  
1  White 
```

The backslashes `\` indicate that a line has been broken. 

Let's get some information about the data set.

Remember that a `DataFrame` is a collection of `Series`, and a `Series` has a single data type. When we use the function `read_csv()`, `pandas` tries to guess the right data type to use for each column. Let's print the data types for each column. We can access the column labels of our `DataFrame` using the `.columns` property, and loop through each one with a `for` loop:

```python
for col in survey_data.columns:
    print(f"{survey_data.loc[:, col].dtype}: {col}")
```

```output
object: Timestamp
object: How old are you?
object: Industry
object: Functional area of job
object: Job title
object: Job title - additional context
int64: Annual salary (gross)
float64: Additional monetary compensation
object: Currency
object: Currency - other
object: Income - additional context
object: Country
object: State
object: City
object: Remote or on-site?
object: Years of experience, overall
object: Years of experience in field
object: Highest level of education completed
object: Gender
object: Race
```

(I've printed the dtypes before the column names because they're all about the same length, so it's easier to read.)

We can see that there two are columns with numeric data - `Annual salary (gross)` and `Additional monetary compensation`. But why is one of them `int64` and one of them `float64`?





- value_counts()
- count NaNs
- summary statistics using describe
- find range and outliers




[^1]: [Cleaning Big Data: Most Time-Consuming, Least Enjoyable Data Science Task, Survey Says. Forbes, 2016](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/)