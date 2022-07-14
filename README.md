# Skill Trend Identification

# Data Collection
Data collection for this project will be done using two methods: APIs and webscraping.
## Using APIs
Import necessary libraries.
```python
import requests
import pandas as pd
import json
from openpyxl import Workbook
```
The dataset used comes form the following source: https://www.kaggle.com/promptcloud/jobs-on-naukricom under a
**Public Domain license**. The original dataset is a csv, which has been converted into a json file. The API url is
stored in the following variable.
```python
api_url = "http://127.0.0.1:5000/data"
```
Using the API, I will define a function and execute a get request to obtain the number of job postings for a 
**specific technology**. I will store the results in an **excel spreadsheet**, named 'job-postings-by-language.xlsx'.

```python
# Defining a function
def get_number_of_jobs_T(technology):
    payload = {"Key Skills": technology}
    r = requests.get(api_url, params = payload)
    if r.ok:
        data = r.json()
    number_of_jobs = len(data)
    return technology, number_of_jobs
```
```python
# Technologies we would like to consider stored in a list.
languages = ["C", "C#", "C++", "Java", "JavaScript", "Python", "Scala", "Oracle", "SQL Server", "MySQL Server", "PostgreSQL", "MongoDB"]
```
```python
# Storing results in an excel file.
wb=Workbook()
ws=wb.active
ws.append(['Language', 'Number of Job Postings'])
for language in languages:
    ws.append(get_number_of_jobs_T(language))
wb.save("job-postings-by-language.xlsx")
```
I will also define a function to obtain the number of job positings for the following **US cities**:
- Los Angeles
- New York
- San Francisco
- Washington DC
- Seattle
- Austin
- Detroit

I will store the results in a separate excel spreadsheet, named 'job-positngs.xlsx'. 

```python
# Defining a function
def get_number_of_jobs_L(location):
    payload = {"Location": location}
    r = requests.get(api_url, params = payload)
    if r.ok:
        data = r.json()
    number_of_jobs = len(data)    
    return location, number_of_jobs
```
```python
locations = ["Los Angeles", "New York", "San Francisco", "Washington DC", "Seattle", "Austin", "Detroit"]
```
```python
# Storing results in an excel file.
wb=Workbook()
ws=wb.active
ws.append(['Location', 'Number of Job Postings'])
for location in locations:
    ws.append(get_number_of_jobs_L(location))
wb.save("job-postings.xlsx")
```
## Webscraping
I will extract data from the below website:
```python
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/labs/datasets/Programming_Languages.html"
```
Required libraries:
```python
from bs4 import BeautifulSoup
import requests
import pandas as pd
```
Download webpage at the url.
```python
data  = requests.get(url).text
```
Soup object
```python
soup = BeautifulSoup(data,"html5lib")
```
Scrape the ```Language Name``` and ```Anuual Average Salary```
```python
table = soup.find('table')
language = []
salary = []
for row in table.find_all('tr'):
        cols = row.find_all('td')
        language_name = cols[1].getText()
        annual_average_salary = cols[3].getText()
        language.append(language_name)
        salary.append(annual_average_salary)
df = pd.DataFrame(list(zip(language, salary)), columns = ['Language name', 'average_annual_salary'])
df = df.iloc[1: , :]
```
Save scrapped data into a file named popular-languages.csv
```python
df.to_csv('popular-languages.csv', index = False)
```

## Exploring Data

# Data Wrangling
Missing Values
Finding and Removing Duplicates
Normalizing Data

# Exploratory Data Analysis
Distribution
Outlier
Correlation

# Data Visualization
Visualizing Distribution of Data
Relationship
Composition
Comparison

# Dashboard

# Presentation
