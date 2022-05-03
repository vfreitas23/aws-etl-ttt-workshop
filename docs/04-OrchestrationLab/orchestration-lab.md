<h1 id="toc_0" align="center">
ORCHESTRATION & DATA ANALYSIS</h1>

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<


#### **1.** Understanding the Orchestration Flow


<<<<< write the right stuff


EVENT BRIDGE TRIGGERS A WORKFLOW AS SOON AS STREAMING DATA LANDS IN S3 - WORKFLOW CRAWLS THE DATA

--> MAKE SURE THE TRAIL HAS THE FOLLOWING PATH CONFIGURED:

Bucket name: etl-ttt-demo-171714944327-ca-central-1  
Prefix:	/etl-ttt-demo/output/gluestreaming/total_clicks/


<<<<< write the right stuff

#### **2.** Creating Glue Workflow and Glue Event Based Trigger (via CLI)


<<<<< write the right stuff


--- create the workflow first  ----

~~~cli
aws glue create-workflow --name etl-ttt-event-driven-workflow
~~~


--- then the trigger  ----

~~~cli
aws glue create-trigger \
    --workflow-name etl-ttt-event-driven-workflow \
    --type EVENT \
    --name s3-object-trigger \
    --actions CrawlerName=crawl_streammed_data \
	--event-batching-condition "{\"BatchSize\": 2,
	\"BatchWindow\": 900}"
~~~
	
<<<<< write the right stuff



#### **3.** Creating Event Bridge Rule and Target (via CLI)


<<<<< write the right stuff


--- next the event rule  ----


~~~cli
aws events put-rule \
    --name "total-clicks-rule" \
    --event-pattern "{ \
                        \"source\": [\"aws.s3\"], \
                        \"detail-type\": [\"AWS API Call via CloudTrail\"], \
                        \"detail\": { \
                            \"eventSource\": [\"s3.amazonaws.com\"], \
                            \"eventName\": [\"PutObject\"], \
                            \"requestParameters\": { \
                                \"bucketName\": [\"${BUCKET_NAME}\"], \
                                \"key\": [{\"prefix\": \"etl-ttt-demo/output/gluestreaming/total_clicks/\"}]
                            } \
                        } \
                    }"
~~~

--- finally the event rule target  ----


~~~cli
aws events put-targets \
    --rule total-clicks-rule \
    --targets "Id"="glueworkflow-totalclicks","Arn"="arn:aws:glue:${AWS_REGION}:${AWS_ACCOUNT_ID}:workflow/etl-ttt-event-driven-workflow","RoleArn"="arn:aws:iam::${AWS_ACCOUNT_ID}:role/AWSEventBridgeInvokeRole-etl-ttt-demo" \
    --region ${AWS_REGION}
~~~

<<<<< write the right stuff


#### **4.** Triggering Orchestration & Following The Flow


<<<<< write the right stuff


[GET THE SCRIPT FROM BUCKET]

~~~shell
cd ~//environment/ttt-demo//
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop  
PutRecord_Kinesis.py .
~~~

~~~shell
cat PutRecord_Kinesis.py
~~~

[SCRIPT SHOULD LOOK LIKE THIS]

~~~python
import csv
import json
import boto3
import time
import string
import random

def generate(stream_name, kinesis_client):
    with open("/tmp/dsd/csv_tables/web_page.csv", encoding='utf-8') as csvf:
        csvReader = csv.DictReader(csvf)
        for rows in csvReader:
            partitionaKey = ''.join(random.choices(string.ascii_uppercase + string.digits, k = 20))
            jsonMsg = json.dumps(rows)
            kinesis_client.put_record(StreamName = stream_name,
                                      Data = jsonMsg,
                                      PartitionKey = partitionaKey)
            print(jsonMsg)
            time.sleep(0.2)

if __name__ == '__main__':
    generate('etl-ttt-demo-stream', boto3.client('kinesis'))
[end of script]

[ONCE EVENT BRIDGE IS FULLY SETUP WITH TARGET AND ALL...]
[START STREAMING DATA BY RUNNIG THE ABOVE SCRIPT WITH THE FOLLOWING COMMAND IN CLOUD9]
[WAIT UP TO 1 OR 2 MINUTES]
~~~


[START THE SCRIPT]

~~~shell
python PutRecord_Kinesis.py 
~~~

[OBSERVE THE WORKFLOW GETTING TRIGGER AS THE FIRST FILES ARRIVE IN S3 FROM THEM STREAMING JOB]

---OBSERVE THE FAILURES DUE TO MULTIPLE FILES ---

[OBSERVE THAT WORKFLOW TRIGGERS THE CRAWLER WHICH CREATES THE TABLE (total_clicks) FOR FOLLOWING ANALYSIS LAB]

[DELETE THE WORKFLOW OR EDIT THE TRIGGER TO 100 SO IT STOP RUNNING AT EVERY BATCH OF STREAMING DATA]


<<<<< write the right stuff

#### **5.** Exploring and Analyzing Table's Data Cataloged in Glue Data Catalog

<<<<< write the right stuff

[ADD DEFAULT BUCKET FOR ATHENA: /athena-output/]

[RUN THE FOLLOWING QUERY]

~~~sql
SELECT * FROM "AwsDataCatalog"."glue-ttt-demo-db"."total_clicks" order by 3 desc limit 10;
~~~

[Creating Views on Top of Cataloged Tables]
[CREATE A VIEW ON TOP OF THE CREATED TABLE TO BROWSE THE TOP 5 CUSTOMERS CLICKING ON THE WEBSITE]

~~~sql
CREATE OR REPLACE VIEW "tpc_customer_inter" AS 
SELECT
  "c_full_name"
, "c_email_address"
, "sum"(CAST("total_clicks" AS integer)) total_clicks
FROM
  "total_clicks" 
GROUP BY 1, 2
ORDER BY 3 DESC
~~~

[QUERY THE VIEW WITH BELOW QUERY]

~~~sql
SELECT * FROM "glue-ttt-demo-db"."tpc_customer_inter" order by 3 desc limit 10;
~~~

[ADD MORE DATA TO THE STREAM AND KEEP CHECKING WITH ABOVE QUERY] - EVERY 30-60 SECONDS


<<<<< write the right stuff


