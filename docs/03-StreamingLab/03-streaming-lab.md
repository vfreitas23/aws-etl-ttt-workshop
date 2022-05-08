<h1 id="toc_0" align="center">
GLUE (STUDIO) STREAMING</h1>

The Part 3 - Glue (Studio) Streaming of the ETL Train the Trainer workshop is where you are going to use AWS Glue Studio Graphical Interface for the first time.

You are going to use Glue Studio do build 2 jobs here. The first job is a *dummy* Glue Streaming Job that will validate the JOIN between the **customer** RDS table and a **web_page** CSV data that you uploaded earlier to S3. 

Note: This CSV data is basically a representation of the actual **web-page-streaming-table** (with the different of that one being in JSON format) that you will use in real Glue Streaming Job

Apart from validate the JOIN between datasets, this *dummy* job will also validate the SQL code used in the SQL Transform node of this lab. If validation succeeds, you will then be able to Preview the Data before outputting it or, in this case, before building the real Glue Streaming Job.

#### **1.** Understanding the Streaming Resources Provided (CloudFormation & Scripts)


For this lab to work, the CloudFormation template provided few resources for you already.

**Kinesis Data Stream**

A Kinesis Data Stream named **etl-ttt-demo-stream** has been added already which will receive the stream of data from a script you will run later on this lab. Click here <add link> to explore the Kinesis Data Stream if you want.

![KINESIS STREAM](images/studio-kinises-data-stream.png)

**Cataloged Streaming Table**

As you already saw it, a table named **etl-ttt-demo-stream** has been created for you with a proper schema definition and all the necessary settings. You can see the details of this table in the AWS Glue Console <link>.

Here's a snapshot of that table's schema.

![WEP-PAGE SCHEMA](images/studio-web-page-table-schema.png)

**Kinesis Data Ingestion Script (python)**

As mentioned, you will be running the following script in order to simulate streaming data being added to your Kinesis Data Stream **etl-ttt-demo-stream**


![INGESTION SCRIPIT](images/studio-kinises-ingestion-script.png)

To download the script (and to take a loot at it), run the following commands:

~~~shell
cd ~/environment/ttt-demo/
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/PutRecord_Kinesis.py .
cat PutRecord_Kinesis.py
~~~

</br>
#### **2.** Validating Streaming Job Logic and Data (Glue Studio Dummy Job)


Let's start building the **dummy** job to validate our Glue Streaming Job logic.

From the AWS Glue Console, click on AWS Glue Studio under the ETL section in the far left menu.

<p align="center">
 <img src=images/studio-job-menu.png>
</p>

This will take you to the AWS Glue Studio Console where you will be authoring the jobs graphically. For that, click on Jobs on the far left menu, select **Visual with a blank** canvas option and click on **Create**.

![DUMMY JOB](images/studio-create-dummy-job.png)

You will be presented with a blank canva. The first thing you must do there is to rename you job. Just click where it says **"Untitled job"**
, type **dummy-streaming-job** and click out the Job name box.

![BLANK CANVAS](images/studio-blank-canvas.png)

Now, the next thing you must do, is to set your jobs detail. Click on the tab **Job details** tab and set the following configurations:

- Under **IAM role** select **AWSGlueServiceRole-etl-ttt-demo**
- Check the **Automatically scale the number of workers** option (GA regions only, otherwise set **number of workers** to **4**)
- Under **Job bookmark** select **Disable**
- Set **Number of retries** to **0**

Note: In production environments, you want to enable the bookmark and set retries to bigger than 0.

You don't need to change any other settings here, but you should take some time to explore what settings are available in this tab. When you are done exploring, click **Save** on the upper right to save the changed settings.

Click the **Visual** tab again to go back to visual editor. You should see 3 dropdown buttons: **Source**, **Transform**, and **Target**. Let's start creating the following 2 sources:

**Web Page Source**

For the **Web Page CSV** source, click on the **Source** dropdown icon and choose **Amazon S3** in dropdown list. Click on the node that has been automatically added to the canvas to highlight it. Make the following changes to it:

- Click **Node properties** tab
 - Set **Name** to **Web Page S3**
- Click **Data source properties - S3** tab
 - Under **S3 source type** select **S3 location**
 - Set **S3 URL** to: *s3://${BUCKET\_NAME}/etl-ttt-demo/csv\_tables/web\_page.csv* - (use the **Browse S3** bucket to navigate.)
 - Uncheck the **Recursive** option
 - Click **Infer schema** button at the bottom
 - Click on **Output schema** tab to see schema infered, then click **Save**
    

**Customer RDS Source**

For the **Customer RDS** source, click on the **Source** dropdown icon and choose **AWS Glue Data Catalog** in the dropdown list.

- Click **Node properties** tab
 - Change Name to **Customer RDS**
- Click **Data source properties - Data Catalog** tab
 - Under **Database** select **glue\_ttt\_demo\_db**
 - Under **Table** select **rds\_crawled\_tpcds\_customer**
 - Click on **Output schema** tab to see schema, then click **Save**

![SOURCES ADDED](images/studio-canvas-sources-added.png)

<!-- [SHOW THE CONNECTION IN GLUE AND SHOW CUSTOM CONNECTOR - SEE WHAT'S BETTER - MENTION MARKETPLACE CONNECTORS] -->


To join both datasets, click on the **Web Page S3** node first to highlight it, then click on **Transform** dropdown icon.

Note: You will notice there are pre-build transformations and custom transformations. Glue Studio is designed to be used by developers who could write custom **Apache Spark, Glue and SQL code**, but it also provides pre-build common transformations. For this lab you wil lbe using a custom SQL Transform to write your own SQL Join code.

In the list of transforms that appears, scroll to the bottom and choose **SQL**. A new **SQL** node will be linked to the **Web Page S3** node. Click on this new **SQL** node to highligt it and do the following:

- Click **Node properties** tab
 - Under **Node parents**, select **Customer RDS** by clicking the checkbox next to it
- Click **Transform tab**
	- Set Input sources **Web Page S3** with **Spark SQL aliases** value to **webpage**
	- Set Input sources **Customer RDS** with **Spark SQL aliases** value to **cust**
	- Copy the following code to SQL query and click **Save**

~~~sql
select CONCAT(cust.c_first_name, ' ', cust.c_last_name) as full_name,
cust.c_email_address,
count(webpage.wp_char_count) as total_clicks
from cust
left join webpage
on cust.c_customer_sk = webpage.wp_customer_sk where 1=2 --remove the where clause after preview!!
group by 1,2
order by total_clicks desc
~~~

- Click on **Data Preview** tab
 - Click on the **Start data preview session** button there.
 - In the pop-up window that will open, choose the IAM role **AWSGlueServiceRole-etl-ttt-demo**
  - Wait for **Data Preview** to finish (it takes about 5 minutes to complete)

Note: Preview the query with **where 1=2** first to make previewing faster and to avoid issues. If issues happen, close this job and clone it into a new one (clone steps is following).

- Once preview is completed, remove the **where 1=2** clause.
- Click on the **Data Preview** tab again to see the data there.

Note that that only relevant columns were brought from the SQL query.

- Click on **Output schema** tab to see schema,

Note that you have 2 outputs: **Output 1** and **Output 2**.

- Click on the **Use datapreview schema** button that you see right there in the **Output schema** tab.

Note that the schema now matches only the relevant columns of the query. Click **Save**.


![PREVIEW OUTPUT](images/studio-preview-output.png)



Now, add an **Apply Mapping** transform by clickinng on the **SQL** node first to highlight it, then click on **Transform** dropdown icon and choose **Apply Mapping** from the list.

A new **Apply Mapping** node will be linked to the **SQL** node. Click on this new **Apply Mapping** node to highligt it and do the following:

- Click on **Transform** tab
 - Change Source key **full_name** to **c\_full\_name**
 - Click on **Output schema** tab to see the change and **Save**.
 
![PREVIEW OUTPUT](images/studio-apply-mapping.png)

 
Finally, add a **Target** to the job. To do this, first click on the **Apply Mapping** node to highlight it, then click on **Target** dropdown, choose **Amazon S3** from the list and a new **Amazon S3** node will be linked to the **Apply Mapping** node. 

Click on this new **Amazon S3** node to highligt it and do the following:


- Click on **Data target properties - S3** tab
 - Set **Format** to **CSV**
 - Set **S3 Target Location** to *s3://$BUCKET\_NAME/etl-ttt-demo/output/gluestreaming/total\_clicks/* 

TIP: Switch back quickly to your Cloud9 enviroment and use the following command to build the entire path you need for the **S3 Target Location** above.

~~~shell
echo "s3://$BUCKET_NAME/etl-ttt-demo/output/gluestreaming/total_clicks/"
~~~


<h4 id="toc_0" align="center"> !!! You can now save this job for the last time but DO NOT RUN IT!!!! </h4>


#### **3.** Creating the Glue Streaming Job (Cloning Jobs!)

<<<<< write the right stuff




[CLONE THE DUMMY JOB AND REPLACE THE CSV WEB_PAGE SOURCE BY THE KINESIS WEB_PAGE STREAMING TABLE]  
!!!! RUN THE JOB BUT DON'T SEND ANY STREAMING DATA YET!!!!  
(SETUP EVENT BRIDGE LAB FIRST !!!!!!


<<<<< write the right stuff











