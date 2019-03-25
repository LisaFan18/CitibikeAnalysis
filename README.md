# Citibike Analysis
Citibike data analysis and visualization is a course project. 

## Data Source
1. data url: https://s3.amazonaws.com/tripdata/index.html   
   The data is storaged on Amazon Simple Storage Severice (S3). It is offered by Amazon Web Services (AWS) that provides object storage through a web service interface. In a word, [Amazon S3](https://aws.amazon.com/s3/) is a kind of cloud storage. 

2. data dictionary https://www.citibikenyc.com/system-data 

## Download.r 
1. Tidyverse is an opinionated collection of R packages designed for data science. All packages share an underlying design philosophy, grammar, and data structures. [These packages](https://www.tidyverse.org/packages/) includes dplyr, readr, tidyr, ggplot2, purrr, stringr 

2. Package [purrr](https://purrr.tidyverse.org/) is  a Functional Programming Tools which include the map() and reduce() functions instead of using for loops. 
They are similar to the MapReduce functions in Spark. The purr [cheatsheet](https://github.com/rstudio/cheatsheets/blob/master/purrr.pdf) visualizes the process of map() and reduce(). 

3. Package [furr]() is to simplify the combination of purrr’s family of mapping functions and [future’s](https://cran.r-project.org/web/packages/future/vignettes/future-1-overview.html) parallel processing capabilities

## sqlite
We analyze 5 years citibike data from 2014.01 - 2018.12 which includes 60 csv files with over 8GB. We use [sqlite 3](https://www.sqlite.org/index.html) as the database engieen for our project for the following [reasons](https://www.sqlite.org/whentouse.html):  
* SQLite is a C-language library that implements a small, fast full-featured, SQL database engine. All group members are familiar with SQL.   
* The SQLite file format is stable, cross-platform. One of team members uses Ubuntu while others use Macbook.
* It's easier to install. In python, you don't need to install sqlite because the sqlite3 model is included in the standarded library. 
* The resulting database is a single file that can be easier to share between group members.
* SQLite can be faster than the filesystem for reading and writing content to disk. fileopen(), fileread() and filewrite()
* Compared to the enterprise client/server databases, it's easier to use. Take python as an example 
```python
import sqlite3
import pandas as pd

# connect to local sqlite database
path = '/Users/lisa/citibike/db'
conn = sqlite3.connect(path + '/db/citibike.db')

# generate standarded Column Names
df1801 = pd.read_csv(path+'/data/citibike201801.csv',nrows=5)
df1801 = df1801.rename(columns=lambda x: x.replace(" ", ""))
df1801.to_sql('AllData', conn, if_exists='append', index=False)

cur_schema=conn.cursor()
cur_schema.execute("PRAGMA table_info('AllData')")
rows_schema = cur_schema.fetchall()
for row in rows_schema:
    print(row)

cur_select = conn.cursor()
cur_select.execute("select * from AllData limit 10 offset 14000 - 10")
#cur_select.execute("select count(*) from AllData")
rows = cur_select.fetchall()
for row in rows:
     print(row)

cur_delete = conn.cursor()
cur_delete.execute("delete from AllData")

conn.commit()
conn.close()
```
