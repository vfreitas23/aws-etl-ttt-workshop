<h1 id="toc_0" align="center">
RUNNING GLUE DATABREW JOBS
</h1>

DataBrew takes on the job of transforming your data by running the instructions that you set up when you made a recipe. The process of running these instructions is called a job. A job can put your data recipes into action according to a preset schedule. But you aren't confined to a schedule. You can also run jobs on demand. If you want to profile some data, you don't need a recipe. In that case, you can just set up a profile job to create a data profile.


#### **6.** Turning projects and recipes into Glue DataBrew jobs
		

Create a new DataBrew job.

    Click JOBS on the left.

    Click Create Job button.

Create job

    Under Job details set the Job name to covid-testing-recipe-job.

    Under Job type select Create a recipe job.

Set job type

    Under Job input select Run on Project. In Select a project select covid-testing from the list.

Set run type

    Under Job output settings set S3 location to s3://${BUCKET_NAME}/output/lab5/csv/ replacing ${BUCKET_NAME} with your own S3 bucket name.

Job output setting

    Under Permission select AWSGlueDataBrewServiceRole-glueworkshop in Role name.

Job role name

    Click Create and run job.

    Once the job has been created, click the JOB icon on the left. Under the Recipe jobs tab you will see a new job with name covid-testing-job in the Running state. Wait for the job to finish.

Run job

    Once the job finishes, click on the job name. You should see one succeeded job run under Job run history tab. Click the Data linage tab to see the data linage graph for the job.

Data linage

    You should see a new folder in S3 under s3://${BUCKET_NAME}/output/lab5/csv/ which contains the output of the job. Use the command below to copy the output files locally in Cloud9 and explore the output in Cloud9.

1
aws s3 cp s3://${BUCKET_NAME}/output/lab5/ ~/environment/glue-workshop/output/lab5 --recursive

View output file