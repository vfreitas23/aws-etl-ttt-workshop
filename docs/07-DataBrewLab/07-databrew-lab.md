<h1 id="toc_0" align="center">
DATA QUALITY & DATA PREPARATION
<br/>
WITH AWS GLUE DATABREW
</h1>

**AWS Glue DataBrew** is a visual data preparation tool that makes it easy for data analysts and data scientists to prepare data with an interactive, point-and-click visual interface without writing code. With Glue DataBrew, you can easily visualize, clean, and normalize terabytes, and even petabytes of data directly from your data lake, data warehouses, and databases, including Amazon S3, Amazon Redshift, Amazon Aurora, and Amazon RDS.

AWS Glue DataBrew is built for users who need to clean and normalize data for analytics and machine learning. Data analysts and data scientists are the primary users.

In this final lab, **Part 7 - Data Quality & Data Preparation with AWS Glue DataBrew -** You, as part of the fictitious **Data Analytics Team**, will work with it the AWS Glue Databrew to create, profile and prepare datasets while, at the same time, work, evolve and public Step Recipes using Glue DataBrew Recipes.



#### **1.** Creating Datasets & Profiling the Data (with Quality Rules). ***(switch to your right region if needed!)***

Go to the [AWS Glue DataBrew](https://console.aws.amazon.com/databrew/) console.


[CREATE A DATASET FROM THE SHIVS THIRD DEMO (INCOME BAND AND CUSTOMER TABLE]

- Create TopCustomer Dataset from Data Catalog  

- (Cataloged via crawler + step functions in the previous lab after removing duplicates with ML Transform job)  
	
[PROFILE THE CUSTOMER DATASET WITH DATA QUALITY RULES - 3-5 minutes for profile job to finish]

- Full dataset

- Output Settings: Navigate to -  s3://$BUCKET_NAME/etl-ttt-demo/output/databrew/jobprofiles/
[CREATE RULE SET]

- TopCustomer-Ruleset
[ADD RECOMMENDED QUALITY RULES + ANY OTHER I WANT]
Rule Set Name: TopCustomer-Ruleset
Associated Dataset: TopCustomers
recommended rules:

- duplicates? (The rule will pass if dataset has duplicate rows count <= 1 )

- missing values less than 2%


custom rules:

- The rule will pass if c_email_address has length >= 5 FOR greater than or equal to 100% of rows 


- Create Income Band from RDS (JDBC connection)
	- Table Name: income_band
	- 
[PROFILE INCOME_BAND DATASET WITH 1 QUALITY RULE]
  
Full dataset  

Ruleset: Income-Band-Ruleset
Rule Name: Income Check for Negative Values  
Rule Summary: The rule will pass if **ib\_lower\_bound**, **ib\_upper\_bound** has values >= 0 FOR greater than or equal to 100% of rows




#### **2.** Working with DataBrew Recipes & Projects


[CREATE A PROJECT FOR INCOME_BAND DATASET]

Project Name: TopCustomers-PerBand

-  show all the transformations available (quick overview)
Choose: Join
			- Choose Customer Dataset to join with A RIGHT JOIN
				-Join by Income_Band Sk and Birthday_day (income band goes from 1-20 only)
				-Remove ib_income_band_sk

Ask people to download from here:

[fix the command - that is an upload command!!]


~~~shell
mkdir -p /tmp/dsd/databrew-recipe/
aws s3 cp s3://ee-assets-prod-${AWS_REGION}/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/databrew/TopCustomers-PerBand-recipe.json  /tmp/dsd/databrew-recipe/TopCustomers-PerBand-recipe.json

aws s3 cp --recursive /tmp/dsd/databrew-recipe/ s3://$BUCKET_NAME/etl-ttt-demo/databrew-recipe/ 
~~~


- To add the steps:
	-  Go to Recipes and import the recipe received from data analytics team
	-  Show all the steps in the recipe and the JSON original recipe.
	-  Publish the recipe
	-  Go back to the Project:		
				- Import the Recipe into the project (Append!!)
				- Explain step validation (append/overwrite)  
				- Reorder the Rename steps (17,18 and 19)  
				- Create an Age field using DATE_DIFF only. (between date_of_birth and todays date )  
				- Drop the c\_birth_\_month, date\_of\_birth, c\_login and match\_id columns  
				- Save and Publish this as a new recipe.
				
- Run the Job and share the results.
