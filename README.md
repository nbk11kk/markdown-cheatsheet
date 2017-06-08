1. Provide a script in a `scripts/` directory to load the CSV data into database table(s) of your own design.

Assumption: I assumed the database as oracle and created all scripts based on oracle syntax

Usually we can directly load the csv files into data base directly using import option in Toad or SQL Developer tool. However that is not ideal solution only for very small data sets (under 10,000 rows).
I created SQL Loader scripts that read any flat files and load the data into database. This process can handle very large number of rows.
All script files are added in scripts folder. Files we use for this process are

Data folder
Sample_salesperson.ctl
Sample_campaigns.ctl
Load_data.cmd

If we run load_data.cmd file, it will call sample_salesperson.ctl and sample_campaigns.ctl files and load data from data folder into data base tables. 

Up on running the load_data.cmd file, log files for each ctl file will get auto generated which has summary information about the data load. 
If there are any errors, a bad data file gets auto generated data with the rejected records. If there is any filter in ctl script, a discard file gets auto generated with filtered rows.



2. Provide SQL scripts in a `scripts/` directory to achieve the following:

Sql queries file is added to scripts folder (All_Queries.sql)
Assumptions are added as comments before each sql query in All_Queries.sql



3. (BONUS ARCHITECTURE QUESTION)   - Imagine this problem scaled up to a large business working with terabytes to petabytes of information.  Would your solution be different?  What kinds of performance issues would you consider?  What overall architectural concerns would you have?  How might you structure a team around this kind of work?  

As mentioned above SQL loader utility should work for millions of rows without any problem but it could be a problem to load terabytes of data and even if we can load terabytes of data, retrieving the data from data base and scalability will be other challenge. Even if something goes wrong with the server while loading there is a high risk of down time.

If we adopt using AWS, we can load csv files into S3 bucket and create a lambda function to refine the data in csv file and then create a kinesis streams to load data into redshift data base which can handle the large amounts of data very well. All this process can be done incrementally. We can set up a cloud watch alarm if there are any errors in the loading process to send the notifications. Since redshift being the columnar data base, data retrieval is very quick .Redshift has also auto scalability option which is very simple to configure.


Dev-Ops engineer will set up the AWS cluster and create a process to dump the csv files into s3 bucket. Lambda function can be created using c# code or any other scripting language by a software/data engineer and then software/data engineer will load data into redshift data base using kinesis streams.






