<h1 id="toc_0" align="center">
WORKFLOW ORCHESTRATION WITH
<br/>AWS STEP FUNCTIONS
</h1>

<<<<< write the right stuff

[NEED TO CREATE A FOLDER NAMED (Cloud9): .../etl-ttt-demo/data_analytics_team_folder/

<!--
MAKE SURE THE TRAIL HAS THE FOLLOWING PATH CONFIGURED:
	- Bucket name: etl-ttt-demo-171714944327-ca-central-1  
	- Prefix:	/etl-ttt-demo/data_analytics_team_folder/
-->

[SIMULATE A FILE BEING SHARED FROM THE ANALYTICS TEAM]

[THIS FILE WILL TRIGGER THE STEP FUNCTION WORKFLOW WHICH CONSEQUENTLY WILL RUN THE FINDMATCHES ML VALIDATION JOB TO CLEAN UP THE DUPLICAIONS. AFTER THAT, STEP FUNCTION WORKFLOW WILL START THE CRAWLER TO CATALOG THE VALIDATED DATA WHICH WILL THEN BE SERVED AS THE SOURCE DATASET FOR THE ANALYTICS TEAM TO USE IN THE DATABREW LAB]

[NEED TO CREATE A CRAWLER TO CRAWL THE GENERATED FILE - STEP FUNCTION WILL TRIGER IT - ADD THIS CRAWLER TO THE CFN TOO]


<<<<< write the right stuff


#### **1.** Creating the Step Function Workflow


<<<<< write the right stuff

Write down the steps to create the workflow...

First is a Glue Job:

StateName: FindMatch Job 
JobName: ml-lab-notebook-job
Wait for task to complete - optional  
Next State: Add a next state
Comment: "To remove duplicates in the dataset using FIndMatches ML Transform"

Then the Glue Crawler:

StateName: Crawl Dedup Data 
JobName: ml-lab-notebook-job
Wait for task to complete - optional  
Next State: Add a next state
Comment: "To crawl the data once the FindMatches job conclude the dedup higiene process"

STEP FUNCTION WORKFLOW - JSON GENERATED CODE SNIPPET

~~~json
{
  "Comment": "A description of my state machine",
  "StartAt": "Find Match Job",
  "States": {
    "Find Match Job": {
      "Type": "Task",
      "Resource": "arn:aws:states:::glue:startJobRun.sync",
      "Parameters": {
        "JobName": "ml-lab-notebook-job"
      },
      "Comment": "To remove duplicates in the dataset using FIndMatches ML Transform",
      "Next": "Crawl Dedup Data"
    },
    "Crawl Dedup Data": {
      "Type": "Task",
      "End": true,
      "Parameters": {
        "Name": "ml_final_crawler"
      },
      "Resource": "arn:aws:states:::aws-sdk:glue:startCrawler",
      "Comment": "To crawl the data after FindMatches dedup higiene process"
    }
  }
}
~~~

State Machine Name: findmatches-ML-dedup-workflow
Existing Role: AWSStepFunctionRole-etl-ttt-demo



<<<<< write the right stuff


#### **2.** Creating an EventBridge Rule & Target for the Step Function Workflow (via CLI)


<<<<< write the right stuff


--- first the event rule  ----


~~~cli
aws events put-rule \
    --name "find-matches-rule" \
    --event-pattern "{ \
                        \"source\": [\"aws.s3\"], \
                        \"detail-type\": [\"AWS API Call via CloudTrail\"], \
                        \"detail\": { \
                            \"eventSource\": [\"s3.amazonaws.com\"], \
                            \"eventName\": [\"PutObject\"], \
                            \"requestParameters\": { \
                                \"bucketName\": [\"${BUCKET_NAME}\"], \
                                \"key\": [{\"prefix\": \"etl-ttt-demo/data_analytics_team_folder/\"}]
                            } \
                        } \
                    }"
~~~

--- then the event rule target  ----

~~~cli
aws events put-targets \
    --rule find-matches-rule \
    --targets "Id"="stepfunction-workflow","Arn"="arn:aws:states:${AWS_REGION}:${AWS_ACCOUNT_ID}:stateMachine:findmatches-ML-dedup-workflow","RoleArn"="arn:aws:iam::${AWS_ACCOUNT_ID}:role/AWSEventBridgeInvokeRole-etl-ttt-demo" \
    --region ${AWS_REGION}
~~~

[SIMMULATE THE FILE BEING UPLOADED BY A DIFFERENT TEAM TO CENTRAL BUCKET FOR FURTHER PROCESSING]

aws s3 cp /tmp/dsd/csv_tables/ml-lab/ml-customer-full/full-topcustomer.csv s3://$BUCKET_NAME/etl-ttt-demo/data_analytics_team_folder/





<<<<< write the right stuff
