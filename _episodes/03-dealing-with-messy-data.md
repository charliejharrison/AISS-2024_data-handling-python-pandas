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

We've covered the basic concepts of storing and accessing data in Pandas, on a nice neat dataset. But real-world datasets are often - usually - nearly always? - quite messy; so much so that data scientists spend the majority of their time organising and cleaning data [^1]. We'll download a dataset and spend some time exploring it, tidying it up, and analysing it, learning some more `pandas` functions along the way. 

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

Let's get some information about the data set.

```python
survey_data.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 17164 entries, 0 to 17163
Data columns (total 20 columns):
 #   Column                                Non-Null Count  Dtype  
---  ------                                --------------  -----  
 0   Timestamp                             17164 non-null  object 
 1   How old are you?                      17164 non-null  object 
 2   Industry                              17118 non-null  object 
 3   Functional area of job                17053 non-null  object 
 4   Job title                             17164 non-null  object 
 5   Job title - additional context        3910 non-null   object 
 6   Annual salary (gross)                 17164 non-null  int64  
 7   Additional monetary compensation      13147 non-null  float64
 8   Currency                              17164 non-null  object 
 9   Currency - other                      90 non-null     object 
 10  Income - additional context           1695 non-null   object 
 11  Country                               17164 non-null  object 
 12  State                                 14179 non-null  object 
 13  City                                  17096 non-null  object 
 14  Remote or on-site?                    17100 non-null  object 
 15  Years of experience, overall          17164 non-null  object 
 16  Years of experience in field          17164 non-null  object 
 17  Highest level of education completed  17115 non-null  object 
 18  Gender                                17081 non-null  object 
 19  Race                                  17095 non-null  object 
dtypes: float64(1), int64(1), object(18)
memory usage: 2.6+ MB
```

We can see that most of the columns have dtype `object`. We'll come back to these. 

Notive that there two are columns with numeric data - `Annual salary (gross)` and `Additional monetary compensation`. But why is one of them `int64` and one of them `float64`? In the original survey form, the validation on the "Additional monetary compensation" question should have prevented any entries that weren't whole numbers. 

Let's try to find which values in that columne are not integers. 

We could try to check the type of each element directly, but this won't work, because Pandas has created the whole column as float64. Instead we have to look at the values and find those which aren't whole numbers. We can use the "modulo" operation to get the decimal portion of a real number, using the percent `%` operator. 

Let's use that to build a mask:

```python
non_int_mask = survey_data["Additional monetary compensation"] % 1 != 0
```

Then we can apply the mask to our dataframe and look at the values that are causing the problem:

```python
non_int_values = survey_data[non_int_mask].loc[:, "Additional monetary compensation"]

print(f"Shape: {non_int_values.shape}")
print(non_int_values.head(5))
```

```output
Shape: (4017,)
3       NaN
7       NaN
8       NaN
10      NaN
13      NaN
         ..
17147   NaN
17148   NaN
17152   NaN
17153   NaN
17161   NaN
Name: Additional monetary compensation, Length: 4017, dtype: float64
```

We can see that instead of decimal numbers, we're dealing with `NaN` - missing values. What's happened here? The original CSV contains empty cells, so when pandas read in those elements it uses NaN as a placeholder. 

- find and count NaNs
- value_counts() to look at categorial statistics
- summary statistics using describe
- find range and outliers
- find distributions for each category


[^1]: [Cleaning Big Data: Most Time-Consuming, Least Enjoyable Data Science Task, Survey Says. Forbes, 2016](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/)