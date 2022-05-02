



----------[SETTING UP GLUE DATABASE, ALL CRAWLERS AND CLASSIFIER FOR SUBSEQUENT LABS]---------

---> TEST MYSQL CONNECTION IN GLUE CONSOLE
---> RUN THE ALREADY CREATED MYSQL-RDS-CRAWLER [3-5 MINUTES] - (THIS CRAWLER AND DATABASE IS ALREADY CREATED)
-------> 2 TABLES GETS CREATED

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
--> Include path: s3://etl-ttt-demo-${BUCKET_NAME}-${ACCOUNT_REGION}/etl-ttt-demo/output/gluestreaming/total_clicks/
--> Sample Size: 1
--> Exclude Pattern: **00001
--> Database: glue-ttt-demo-db
--> Grouping Behavior: Create a single schema for each S3 path [check]
!!!!DO NOT RUN THIS CRAWLER YET!!!!
