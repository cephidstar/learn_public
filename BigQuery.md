# Introduction to Google Cloud BigQuery

## Introduction

BigQuery is Google Cloud's enterprise data warehouse, built upon a serverless architecture. BigQuery data is stored in table/column format and is accessed and   managed through standard SQL, which can be executed through BigQuery's API client libraries, built for several common languages.

From an observability perspective, BigQuery is particularly suited for storing and analyzing observability data, such as event data produced by performance monitoring applications. BigQuery is highly scalable and it includes powerful features such as a high bandwidth data analysis engine, supported by machine learning. BigQuery also supports the monitoring of its own performance, by computing and storing metrics around the operations executed upon the data it houses. 

In this course, you'll become familiar with BigQuery through hands-on practice, by executing SQL queries in both the BigQuery SQL Workspace and through the BigQuery Cloud Shell.
 
At the end of this course, you'll have had some practice with BigQuery and you'll be equipped with a sandbox to continue experimenting on your own, using the many resources that Google Cloud and other practitioners provide.

## Connecting to the BigQuery Sandbox

The BigQuery Workspace is available to anyone with a Google Account at no cost. However Google requires you to have set up a payment method on your account, for at minimum, verification purposes.

If you already have a Google account with a payment method defined, the BigQuery setup will consist of just a few steps. Otherwise, they'll be just a few more steps for entering and verifying your information.

To get started, go to the [BigQuery main site](https://cloud.google.com/bigquery). If you're already signed up to use BigQuery, then the link above will take you to the right place and you can skip the rest of this section. 

Click either of the **getting started** buttons you'll find on the welcome/initialization screen.

![](/assets/images/sign_up_1b.png)

This will initiate the sign up steps you'll need to complete. When you've finished, you'll be presented the Google Cloud Console shown below. Scroll to find a link to the BigQuery product and select it.

![](/assets/images/sign_up_2.png)

When you've arrived at the BigQuery Workspace shown below, you're ready to access data!

![](/assets/images/BigQuery_Console_bd.png)

### Lab: Execute SQL Commands in the BigQuery Workspace

**Step 1.** You should be logged into the BigQuery Workspace, otherwise open it [here](https://console.cloud.google.com/bigquery).

Within the workspace, BigQuery gives you access to the 'Google Trends' public dataset, which includes a wide range of statistical data that you can practice accessing with SQL. 

![](/assets/images/Lab1_image_1.png)

**Step 2.** Within the initial Workspace banner shown above, Click VIEW DATASET. If prompted again in another pop-up window, click View Dataset again. 

The **bigquery-public-data** dataset appears in the Explorer pane of the console, on the left, just below your personal dataset, which will be issued a randomly generated name such as 'beaming-storm-379618'.

![](/assets/images/BQ_Explorer_bd.png)

**Step 3.** Expand the bigquery-public-data dataset in the Explorer pane to reveal the sample table groups that you can practice upon.
Each dataset contains one or more tables. Scroll down to and expand the **census_bureau_usa** table group. 
 
**Step 4.** Within the census_bureau_usa table group, click the **population_by_zip_2010** table to open it in the workspace to the right.  Here you'll see the list of fields that comprise the table.  Take a moment to review the field names and definitions. Let's now run some queries on it.  

**Step 5.** Just above the fields list, open the ![](/assets/images/Query_dropdown.png) menu and select the **In a new tab** option.  The workspace opens with a simple, incomplete SQL SELECT statement. The statement is shown in error because you need to supply to the clause, the fields to retrieve. 
 
Select 'all fields' by entering an asterisk * in the space between the SELECT and FROM clauses, making no other changes to the statement. It should look as shown below:

~~~
SELECT * FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` LIMIT 1000 
~~~

**Step 6.** Click the RUN command just above the query pane. The query results should appear in the a panel below the statement. The first 1000 records in the **population_by_zip_2010** table.

![](/assets/images/query_results.png)
 
**Step 7.** Modify the statement as shown below, and re-run it to retrieve the population for a specific zip code, perhaps your hometown zipcode. You can copy/paste
and overwrite statements into the query pane.

~~~
SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = '12054'
~~~

**Step 8.** Just for fun, query the **samples.shakespeare** table, which contains the text of the entire works of Shakespeare. The following query counts the number of distinct phrases in Shakespeare's works, where the supplied string is found.

~~~
SELECT word, SUM(word_count) AS count FROM `bigquery-public-data`.samples.shakespeare WHERE word LIKE '%love%' GROUP BY word 
~~~

## Accessing BigQuery Data Outside the Sandbox

BigQuery is certainly more than just an interactive workspace where you can query, import, analyze, and shape data with SQL. BigQuery serves as a data repository for other cloud services and enterprise applications, regardless of where they are deployed.  
  
BigQuery provides APIs for use in Go, Java, Node.js, PHP, Python, C#, and Ruby code. Accessing and managing BigQuery data through code, boosts the flexibility and power of SQL queries by introducing parameters, variables, logic, functions, or any other programming constructs (e.g. loops) you require.

Thankfully, Google Cloud provides tools that make it easy to connect to BigQuery externally, and to experiment with coded SQL. In the next lab, you'll experiment with the **Google Cloud Shell**, the **bq command-line tool**, and the **CloudShell Editor**. 

### Lab: Access BigQuery Data Through Google Cloud Shell

**Step 1.** Access the [Google Cloud Shell](https://console.cloud.google.com/bigquery?cloudshell=true).

Wait a few moments as Cloud Shell provisions a Compute Engine virtual machine running a Debian-based Linux operating system for your use. The Cloud Shell terminal should open in a panel at the bottom of the screen:

![](/assets/images/Cloudshell_Terminal.png)
 
**Step 2.** But instead of creating, editing and running scripts solely through this bash shell command line, let's make this exercise even easier by using the Google Cloudshell Editor. 
 
Just above the terminal, to the right,  Click ![](/assets/images/open_editor.png) and give it a moment to appear. If prompted, open the 'Home Workspace', and again, if prompted 'Activate' the shell.

**Step 3.** Now add the terminal back into the workspace by clicking **Open Terminal** above the editor.

![](/assets/images/open_terminal.png)

You should see the Explorer panel on the left, with a folder/file directory under your Google account name. If you mouse-over the top of this directory, you should see icons for creating new files and folders. 

![](/assets/images/new_file.png)

**Step 4.** Create a new script file and name it **myfirstbqscript.sh**.

**Step 5.** Copy the following code into the script (in two lines). The **bq query** command executes the SQL statement that follows it.  

~~~~
bq query --use_legacy_sql=false \ 
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~

**Alert! 
Make sure the escape character at the end of the first line is recognized. If it is colored red as shown below ...

![](/assets/images/colored_red.png)

... delete it and retype it starting from preceding characters, until it is recognized. Otherwise your script will not run properly.

**Step 6.** On the command line in the bottom panel, run your script with **./myfirstbqscript.sh**  If prompted, click to authorize use of the bq command. 
 
**Was there a permissions error when you ran your script?** With no other permissions settings entered, each newly created script file you attempt run will require you to open up the permissions. Correct the issue by executing **chmod +x myfirstbqscript.sh** and run the script again. 

You should get a result similar to the one below. 

![](/assets/images/bq_sql_output_bd.png)


## Converting SQL Queries to Parameterized Queries 

When executing SQL from code, it's impractical to have to edit an SQL statement each time you want to produce slightly different results. 
**Parameterizing SQL queries** is central to the technique of writing dynamic SQL, which builds SQL statements dynamically at runtime. Dynamic SQL is commonly used in SQL stored procedures.  

Recall the zipcode example from the previous lab:

~~~~
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~

If you could substitute the literal "12054" in the command with a parameter, you could supply the value on the command line, each time you ran your script. 
 
### Lab: Convert your SQL query into a parameterized query.

#### Part I - Modify myfirstbqscript.sh to receive the value for the zip code from the command line. ####

Running your script on the command line with a parameter would look like this:

**$ ./myfirstbqscript.sh 12054**

Within the script, the parameter value is placed in the variable **$1**. If you use multiple parameters on
the command line, they would be assigned to the variables $1, $2, $3 etc. within the script. 
 
**Step 1.** Add the following line of code above the **bq query** command in **./myfirstbqscript.sh**, to recieve the zip code value in a well-named variable.

**zipcode=$1**

**Step 2.** Replace **WHERE zipcode = "12054"'** in the SQL statement, with the following parameterized token.

**WHERE zipcode = "'$zipcode'"'** 

**Step 3.** Execute your script as /myfirstbqscript.sh ##### where ##### is a zipcode of your choice.

#### Part II ####
 
Just for fun, create another script named **shakespeare.sh**, to run the same SQL statement we ran in the BigQuery Workspace: 

~~~
SELECT word, SUM(word_count) AS count FROM `bigquery-public-data`.samples.shakespeare WHERE word LIKE '%love%' GROUP BY word 
~~~

Converting the **LIKE '%love%'** clause to use a parameter is a little trickier and not well documented. 

Here you need to use **LIKE "%'$search_string'%"**

Give it a try, and enjoy your rapid fire Shakespeare data mining!

For more information on **Quoting mechanisms in Unix/Linux**, check out the **Resources** section at the end of this course.

## Other Things to Try

#### Challenge #1 ####

Write a script-driven BigQuery query that accepts 2 or 3 command line parameters that will become values inserted into a parameterized SQL statement. 

However, your script must execute without error, when some or none of the parameters are provided. Take some time to consider the various SQL statement default behaviors you require for each possible combination of supplied parameters.

#### Challenge #2 ####

BigQuery is particularly suited for storing and analyzing observability data. 

In this repository's **/assets** folder, you'll find a CSV file named **eventlog.csv**.
Use BigQuery's **ADD DATA** feature (shown below) to upload this file to create a new data table. Once uploaded, it will
appear in your default dataset that was created for you when you created your BigQuery account.

![](/assets/images/add_data.png)

The **eventlog.csv** file contains sample event data, similar perhaps to event data produced by performance monitoring applications. 

After uploading the file, consider which types of queries you might write in order to analyze this event data. Also consider how parameterization
of your SQL could lead to a standard set of API's or some other programmed assets, that could be used by other applications to analyze this data and/or present the results in dashboards.

## Summary

Congratulations on completing this course. You now have working knowledge of how to use BigQuery for your application requirements.

We hope you plan to continue using the BigQuery Workspace and Google Cloud Shell to experiment with SQL. Note that can also access BigQuery through your favorite programming languages, using the BigQuery APIs. 

If you signed up with Google BigQuery for the free account access, make sure to monitor your usage and account status as you continue experimenting in the BigQuery Workspace, Google Cloud Shell.

## Resources

### Google Cloud and BigQuery Resources ###

[Google Cloud documentation](https://cloud.google.com/docs)

[BigQuery documentation](https://cloud.google.com/bigquery/docs)

### Other Resources ###

[W3 Schools SQL Documentation](https://www.w3schools.com/sql/sql_quickref.asp)

#### Quoting mechanisms in Unix/Linux ####
[gnu.org](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)
[Educative](https://www.educative.io/answers/what-are-quoting-mechanisms-in-unix-linux)
[Tutorialspoint](https://www.tutorialspoint.com/unix/unix-quoting-mechanisms.htm)
