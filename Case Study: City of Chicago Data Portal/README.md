# Demographic Analysis of Chicago

We will be working on a real world dataset provided by the Chicago Data Portal wherein we will be assuming the position of Data Analyst for a non-profit organization that strives to improve educational outcomes for children and youth in the City of Chicago. We will be analysing the census, crime, and school data for a given neighborhood or district and further identify the causes that impact the enrollment, safety, health, environment ratings of schools.

In this assignment, we will download the datasets provided, load them into a database, write and execute SQL queries to answer the problems provided.

Using this Python notebook we will:

- Understand three Chicago datasets
- Load the three datasets into three tables in a Db2 database
- Execute SQL queries to answer assignment questions

# Understand the datasets

To complete the assignment problems in this notebook you will be using three datasets that are available on the city of Chicago's Data Portal:

Socioeconomic Indicators in Chicago
Chicago Public Schools
Chicago Crime Data

1. Socioeconomic Indicators in Chicago
This dataset contains a selection of six socioeconomic indicators of public health significance and a “hardship index,” for each Chicago community area, for the years 2008 – 2012.

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at: https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2

2. Chicago Public Schools
This dataset shows all school level performance data used to create CPS School Report Cards for the 2011-2012 school year. This dataset is provided by the city of Chicago's Data Portal.

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at: https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t

3. Chicago Crime Data
This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days.

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at: https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2

# How to work with IBM Db2 Database console?

To analyze the data using SQL, it first needs to be stored in the database.

While it is easier to read the dataset into a Pandas dataframe and then PERSIST it into the database, it results in mapping to default datatypes which may not be optimal for SQL querying. For example a long textual field may map to a CLOB instead of a VARCHAR.

Therefore, we will manually load the table using the database IBM Db2 console LOAD tool.

## Connecting to IBM Db2 Database console

First, load the SQL extension and establish a connection with the database

>> !pip install sqlalchemy==1.3.9
>> !pip install ibm_db_sa

Load SQL extension

>> %load_ext sql

Enter the connection string for Db2 on Cloud database instance. The format looks something like as the following; 

>> %sql ibm_db_sa://my-username:my-password@my-hostname:my-port/my-db-name
