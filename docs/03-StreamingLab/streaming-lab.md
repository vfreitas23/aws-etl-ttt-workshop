<h1 id="toc_0" align="center">
GLUE (STUDIO) STREAMING</h1>

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<

#### **1.** Understanding the Streaming Resources Provided (CloudFormation & Scripts)


<<<<< write the right stuff

[STREAMING DEMO PART]


[STREAM IN KINESIS IS PART OF THE TEMPLATE (ADD THE TABLE)  
-----> Use etl-ttt-demo-stream KINESIS DATA STREAM (ALREADY CREATED)

[DONT NEED TO BELOW TABLE FOR THE STREAMING WEB_PAGE - BUILT IT IN CLOUD FORMATION]

[TABLE SHOULD LOOK LIKE THE FOLLOWING]  
mysql -h ${mysqlendpoint} -u admin -pVitor123 -Dtpcds -e "DESCRIBE web_page"

| Field               | Type         | Null |
|---------------------|:------------:|:----:|
| wp_web_page_sk      | int (PK)     | NO   |
| wp_web_page_id      | char(16)     | NO   |
| wp_rec_start_date   | date         | YES  |
| wp_rec_end_date     | date         | YES  |
| wp_creation_date_sk | int          | YES  |
| wp_access_date_sk   | int          | YES  |
| wp_autogen_flag     | char(1)      | YES  |
| wp_customer_sk      | int          | YES  |
| wp_url              | varchar(100) | YES  |
| wp_type             | char(50)     | YES  |
| wp_char_count       | int          | YES  |
| wp_link_count       | int          | YES  |
| wp_image_count      | int          | YES  |
| wp_max_ad_count     | int          | YES  |

KINESIS INGESTION SCRIPT

~~~shell
cd ~/environment/ttt-demo/code???/  --CHECK THE PATH*
cat PutRecord_Kinesis.py
~~~

<!--
[NO NEED TO DO THIS STEP - KINESIS INGESTION SCRIPT IS IN THE ASSET BUCKET TOO]
cd ~//environment/ttt-demo//
cp ~/environment/glue-workshop/code/lab4/PutRecord_Kinesis.py .
sed -i 's+./data/lab2/sample.csv+/tmp/dsd/csv_tables/web_page.csv+g' PutRecord_Kinesis.py 
sed -i 's+glueworkshop+etl-ttt-demo-stream+g' PutRecord_Kinesis.py 
cat PutRecord_Kinesis.py
[TO SEE IF ABOVE SED WORKED - REPLACE FOR A NEW STREAMING NAME TOO AFTER FIXING THE CFN TEMPLATE ACCORDINGLY]
-->

<<<<<<


#### **2.** Validating Streaming Job Logic and Data (Glue Studio Dummy Job)


<<<<< write the right stuff

[CREATE A DUMMY GLUE STUDIO JOB TO TEST THE JOIN BETWEEN RDS CUSTOMER AND WEB_PAGE CSV FILE IN S3 - DUMMY DATA TO TEST THE PREVIEW IN GLE STUDIO]
---> give the job a name
---> set the job details (workers, role, retries, etc) and save it.

[SHOW THE CONNECTION IN GLUE AND SHOW CUSTOM CONNECTOR - SEE WHAT'S BETTER - MENTION MARKETPLACE CONNECTORS]

-> WebPage S3 source: s3://glueworkshop-039185979121-us-east-2/etl-ttt-demo/csv_tables/web_page.csv
-> Customer RDS source: glue-ttt-demo-db/rds_table_tpcds_customer

[ADD A JOIN TRANSFORM WITH BOTH SOURCES AS PARENTS AND USE BELOW QUERY TO JOIN RDS TO KINESIS STREAM - FROM SHIV'S DEMO]

~~~sql
select CONCAT(cust.c_first_name, ' ', cust.c_last_name) as full_name,
cust.c_email_address,
count(webpage.wp_char_count) as total_clicks
from cust
left join webpage
on cust.c_customer_sk = webpage.wp_customer_sk where 1=2 --remove it after building the output
group by 1,2
order by total_clicks desc
~~~

[PREVIEW THE QUERY WITH where 1=2 FIRST TO MAKE PREVIEWING FASTER AND AVOID ISSUES]
[ONCE PREVIEW IS COMPLETED - APPLY THE OUTPUT SCHEMA FROM DATAPREVIEW - SQL JOIN NODE!]
-> Appply Mapping: Do it after previewing data with Use Output Previewed button
---> full_name to c_full_name

-> Target: s3://etl-ttt-demo-XXXXXXXXXXX-XXXXXXXXX-1/etl-ttt-demo/output/gluestreaming/total_clicks/
-> Format: CSV !!!!

[!!! SAVE IT BUT DON'T RUN IT, ALL YOU NEED IS THE PREVIEW TO CONFIRM THE JOIN IS WORKING!!!]

<<<<< write the right stuff

#### **3.** Creating the Glue Streaming Job (Cloning Jobs!)

<<<<< write the right stuff


[CLONE THE DUMMY JOB AND REPLACE THE CSV WEB_PAGE SOURCE BY THE KINESIS WEB_PAGE STREAMING TABLE]
!!!! RUN THE JOB BUT DON'T SEND ANY STREAMING DATA YET!!!! (SETUP EVENT BRIDGE LAB FIRST !!!!!!


<<<<< write the right stuff











