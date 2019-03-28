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
## R shiny
1. [Shiny cheatsheet](https://shiny.rstudio.com/images/shiny-cheatsheet.pdf) . 
    **ui.R**: nested R functions that assemble an HTML user interface. The input objects are used to collect values from users. It inculde textInput, passwordInput, radioInput, selectInput, dateRangeInput and so on.   
    **server.R**: a function with instructions on how to build and rebuild R objects displayed in the UI. The instructions include some render*() and *output() functions that work together to add R objects to the UI. 
    
2. [ShinyDashboard](https://rstudio.github.io/shinydashboard/index.html)
   Shinydashboard is built using [AdminLTE](https://github.com/ColorlibHQ/AdminLTE), which in turn uses [Bootstrap3](https://getbootstrap.com/).
   AdminLTE is a template and based on Bootstrap3 framework. Highly customizable and easy to use. Fits many screen resolutions from small mobile devices to large desktops.    
   Bootstrap3 is an open source toolkit for developing with HTML, CSS, and JS.  
   Icons are used liberally in shinydashboard. [FrontAwesome](https://fontawesome.com/icons?from=io) provides a list of icons. 
3. [Dashboard themes](https://github.com/nik01010/dashboardthemes)   
   install.packages("devtools")  
   devtools::install_github("nik01010/dashboardthemes") . 
   https://github.com/ropensci/git2r/issues/204   
4. [Leaflet](https://rstudio.github.io/leaflet/map_widget.html)

