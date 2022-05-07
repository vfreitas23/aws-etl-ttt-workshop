
<h1 id="toc_0" align="center">
AWS GLUE COMPONENTS
<br/>
(Databases, Tables, Connections, Crawlers And Classifiers)
</H1>

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<



#### **1.** Understanding the Glue Resources Provided (CloudFormation Resources)


<<<<< write the right stuff

----------[SETTING UP GLUE DATABASE, ALL CRAWLERS AND CLASSIFIER FOR SUBSEQUENT LABS]---------

<<<<<<


#### **2.** Testing and running pre-created Glue Resources (Glue Connection & Glue Crawler for MySQL RDS)

<<<<< write the right stuff

---> TEST MYSQL CONNECTION IN GLUE CONSOLE  
---> RUN THE ALREADY CREATED MYSQL-RDS-CRAWLER [3-5 MINUTES] - (THIS CRAWLER AND DATABASE IS ALREADY CREATED)  
-------> 2 TABLES GETS CREATED  

<<<<< write the right stuff

#### **3.** Creating new Glue Resources (New Crawler & Classifier)

<<<<< write the right stuff

[WHILE ABOVE CRAWLER IS RUNNING, DO THE FOLLOWING TWO PIECES FOR THE STREAMING CRAWL (FOLLOWING LAB)]

---> [CREATE A CLASSIFIER]
CSV  
Comma (,)  
Double-quote (")  
Has headings  
c_full_name,c_email_address,total_clicks  

---->[CREATE A CRAWLER TO READ FROM THE PATH OF THE STREAMING JOB - USE A CUSTOM CSV CLASSIFIER FOR THAT!!!]

--> Name: crawl_streammed_data  
--> Classifier: CSV  
--> Include path: [In Cloud9] -

~~~cli
echo "s3://${BUCKET_NAME}/etl-ttt-demo/output/gluestreaming/total_clicks/"
~~~
  
--> Sample Size: 1  
--> Exclude Pattern: **00001  
--> Database: glue-ttt-demo-db  
--> Grouping Behavior: Create a single schema for each S3 path [check]  
!!!!DO NOT RUN THIS CRAWLER YET!!!!

<<<<< write the right stuff
