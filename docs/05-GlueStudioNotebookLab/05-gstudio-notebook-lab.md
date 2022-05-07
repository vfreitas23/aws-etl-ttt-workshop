<h1 id="toc_0" align="center">
MACHINE LEARNING WITH GLUE
<br/> & <br/>GLUE STUDIO NOTEBOOK
</h1>

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<


#### **0.** (Pre Steps) - Understading & Setting up the Resources for ML Lab

<<<<< write the right stuff

[MACHINE LEARNING TRANSFORM LAB]  

DESCRIPTION: CSV with duplicated customers but with 

~~~shell
mkdir -p /tmp/dsd/csv_tables/ml-lab/ml-customer-sampling /tmp/dsd/csv_tables/ml-lab/ml-customer-full /tmp/dsd/csv_tables/ml-lab/ml-labeling-file
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/ml-customer/sample_topcustomer.csv /tmp/dsd/csv_tables/ml-lab/ml-customer-sampling/sample_topcustomer.csv
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/ml-customer/full-topcustomer.csv /tmp/dsd/csv_tables/ml-lab/ml-customer-full/full-topcustomer.csv
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/ml-customer/topcuustomer-labeling-file.csv /tmp/dsd/csv_tables/ml-lab/ml-labeling-file/topcuustomer-labeling-file.csv

aws s3 cp --recursive /tmp/dsd/csv_tables/ml-lab s3://$BUCKET_NAME/etl-ttt-demo/csv_tables/ml-lab/
~~~

<<<<< write the right stuff


#### **1.** Creating and Training the Glue FindMatches ML Transform

- Run the ML- Crawler (add this one to the CFN template)


- Use the sample file to generate labeling file (in S3)
- Download the generated labeling file and show how to add the labels to similar records (Just show 2 or 3 records to demonstrate)
- Use the already-filled labeling file to train the ML FindMatches transform (get it from S3)

[Don't need to wait anything to complete, just show it. Then use the pre-created ones]


#### **2.** Testing FindMatches Transform with Glue Studio Notebook

<<<<< write the right stuff


Once the FindMatches transform is trained, Open Glue studio and create a new Glue Studio Notebook.
	- Name: ml-lab-notebook-job
	- IAM Role: Glue Service Role - etl-ttt-demo

Write the following Spark Code into 4 differente cells

Cell 1 [replace the existing Cell 1]:

~~~python
%glue_version 2.0
%idle_timeout 60

import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
from awsglueml.transforms import FindMatches
  
sc = SparkContext.getOrCreate()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
~~~

Cell 2:

~~~python
datasource0 =      glueContext.create_dynamic_frame.from_catalog(database="glue-ttt-demo-db", table_name="ml_dedup_full_topcustomer_csv",  transformation_ctx = "datasource0")      
                                                        
resolvechoice1 = ResolveChoice.apply(frame = datasource0, choice = "MATCH_CATALOG", database = "glue-ttt-demo-db",  table_name = "ml_dedup_full_topcustomer_csv", transformation_ctx = "resolvechoice1")

resolvechoice1.printSchema()
~~~


Cell 3:

~~~python
findmatches2 = FindMatches.apply(frame = resolvechoice1, transformId = "tfm-733f49ce1a5f98c4e2b8a9604ae4427368b08b07", transformation_ctx = "findmatches2")

resolvechoice3 = ResolveChoice.apply(frame = findmatches2, choice = "make_struct", transformation_ctx = "resolvechoice3")

dropnullfield4 = DropNullFields.apply(frame = resolvechoice3, transformation_ctx = "dropnullfield4")
df1 = dropnullfield4.toDF()

print("Duplicate count of matching IDs : "+ str(df1.count()))

~~~

Cell 4:

~~~python

custidlist = ["AAAAAAAANDCFBAAA","BAAAAAAAJLKGBAAA","AAAAAAAAJLKGBAAA","AAAAAAAAPLNDAAAA","AAAAAAAANDCFBAAA","BAAAAAAAPLNDAAAA","BAAAAAAANDCFBAAA","AAAAAAAANKFAAAAA","BAAAAAAANKFAAAAA","AAAAAAAADFHGAAAA","BAAAAAAADFHGAAAA"]
cols = ['c_customer_id','c_current_addr_sk','c_salutation','c_first_name','c_last_name','c_birth_day','c_birth_month','c_birth_year','c_birth_country','c_login','c_email_address','match_id']

print("Duplicate count of matching IDs : "+ str(df1.count()))

df1.filter(df1.c_customer_id.isin(custidlist) ).select(*cols).orderBy("match_id").show(truncate=False)

~~~

Cell 5:

~~~python

df2 = df1.drop_duplicates(subset=['match_id'])
print("Distinct count of matching IDs : "+ str(df2.count()))

df2.filter(df2.c_customer_id.isin(custidlist) ).select(*cols).orderBy("match_id").show(truncate=False)

~~~


---> Wait for the last cell to finish and output the results.

<<<<< write the right stuff



#### **3.** Validating and Deploying the FindMatches Glue Job 

<<<<< write the right stuff

---> validate the results:
	--> Reference this blog: Reference This Blog: http://blog.zenof.ai/machine-learning-based-fuzzy-matching-using-aws-glue-ml-transforms/

"However, the Find matches transform adds another column named match_id to identify matching records in the output. Rows with the same match_id are considered matching records."


Once done explaining the Matching IDs logic, add a new cell to the notebook to sink the very final dataframe to S3 but DON'T RUN this cell!!!

[DON'T RUN THIS CELL] Cell 6:

~~~python
new_df  = df2.select(*cols).orderBy("match_id")

new_df.coalesce(1).write.option("header","true").mode("overwrite").csv("s3://etl-ttt-demo-589541849248-us-east-2/etl-ttt-demo/output/ml-lab/topcustomers-dedup/")
~~~

[DON'T RUN THIS CELL]

<!--
S3 Target Location: s3://etl-ttt-demo-589541849248-us-east-2/etl-ttt-demo/output/ml-lab/topcustomers-dedup/ 
!-->

JUST SAVE THE JOB TO DEPLOY IT PROPERLY AND MOVE TO THE NEXT LAB]

<<<<< write the right stuff