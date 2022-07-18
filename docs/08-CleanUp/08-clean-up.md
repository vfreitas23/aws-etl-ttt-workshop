<h1 id="toc_0" align="center">
CLEAN UP
</h1>


#### NOTE: UNDER CONSTRUCTION - MAKE SURE


If you are at an **AWS hosted event** and used **Event Engine** for your labs, you can skip the following clean up steps.

If you are using your **own AWS account** for the labs, we strongly recommend you to perform the **clean up steps** as soon as you finish the labs to **avoid extra charges**.

Execute the commands below in **Cloud9 terminal** to delete the **AWS resources created** during labs.

> If you choose to use names that are different than the default names provided in the lab instructions, please modify the names to match your own resources in the commands below.

**1\.** Delete the **DataBrew project**, **jobs**, and **dataset**.

~~~cli
aws databrew delete-job --name <your jobs names>
aws databrew delete-job --name <your profile jobs names>
aws databrew delete-project --name <your projects names>
aws databrew delete-dataset --name <your datasets names>
~~~

**2\.** Find the **recipe versions** in the **recipe store** and delete the versions list by below cli command.

~~~cli
aws databrew list-recipe-versions --name <you recipes names>
~~~

> Replace the recipe version number with what you find from command above.

~~~cli
aws databrew batch-delete-recipe-version --name <your repices names> --recipe-version "1.0"
~~~
**3\.** Delete all the **Glue jobs, Crawlers and Databases** created during the labs.

~~~cli
aws glue delete-job --job-name dummy-streaming-job
aws glue delete-job --job-name glue-streaming-job 
aws glue delete-job --job-name ml-notebook-job

aws glue delete-crawler --name <crawler names>

aws glue delete-database --name <database names>

~~~

**5\.** Delete Triggers and Workflows

~~~cli
aws glue delete-workflow --name <your workflow name>
aws glue delete-trigger --name <your trigger name>

aws events list-targets-by-rule --rule <your rule name>
aws events remove-targets --rule <your rule name> --ids "<your target name 1>"
aws events remove-targets --rule <your rule name> --ids "<your target name 2>"

aws events delete-rule --name <your rule name>

aws stepfunctions delete-state-machine --state-machine-arn arn:aws:states:${AWS_REGION}:${AWS_ACCOUNT_ID}:stateMachine:<your state machine name>
~~~
-->

**6\.** Delete the CloudTrail and the Workshop bucket.

~~~cli
aws s3 rm s3://${BUCKET_NAME} --recursive
~~~

**7\.** Delete the lab Cloudformation stacks.

~~~cli
aws cloudformation delete-stack --stack-name etl-ttt-demo
~~~

**8\.** Close the browser tab of Cloud9 IDE.

Now you have cleaned up all the resources created during the workshop.
