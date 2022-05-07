<h1 id="toc_0" align="center">
DATA QUALITY & DATA PREPARATION
<br/>
WITH AWS GLUE DATABREW
</h1>

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<


#### **1.** Creating Datasets & Profiling the Data (with Quality Rules)


<<<<< write the right stuff

[CREATE A DATASET FROM THE SHIVS THIRD DEMO (INCOME BAND AND CUSTOMER TABLE]

---> Create TopCustomer Dataset from Data Catalog
	---> (Cataloged via crawler + step functions in the previous lab after removing duplicates with ML Transform job)
---> Create Income Band from RDS connection (delete income band from crawler - check template and lab instruction)

[PROFILE THE CUSTOMER DATASET WITH DATA QUALITY RULES - 3-5 minutes for profile job to finish]
--> Full dataset
--> Output Settings: Navigate to -> s3://$BUCKET_NAME/etl-ttt-demo/output/databrew/jobprofiles/
[CREATE RULE SET]
---> TopCustomer-Ruleset
[ADD RECOMMENDED QUALITY RULES + ANY OTHER I WANT]
Rule Set Name: TopCustomer-Ruleset
Associated Dataset: TopCustomers
recommended rules:
--> duplicates? (The rule will pass if dataset has duplicate rows count <= 1 )
--> missing values less than 2%
custom rules:
--> The rule will pass if c_email_address has length <= 5 FOR greater than or equal to 100% of rows 


[PROFILE INCOME_BAND DATASET WITH 1 QUALITY RULE]
Full dataset
Rule Name: Income Check for Negative Values
Rule Summary: The rule will pass if **ib_lower_bound**, **ib_upper_bound** has values >= 0 FOR greater than or equal to 100% of rows


#### **2.** Working with DataBrew Recipes & Projects

<<<<< write the right stuff

[CREATE A PROJECT FOR INCOME_BAND DATASET]
Project Name: TopCustomers-PerBand

---> show all the transformations available (quick overview)
Choose: Join
			-> Choose Customer Dataset to join with A RIGHT JOIN
				--> Join by Income_Band Sk and Birthday_day (income band goes from 1-20 only)
				--> Remove ib_income_band_sk

---> To add the steps:
	-> Go to Recipes and import the recipe received from data analytics team
	-> Show all the steps in the recipe and the JSON original recipe.
	-> Publish the recipe
	-> Go back to the Project:		
				---> Import the Recipe into the project
				---> Explain step validation (append/overwrite)
				---> Reorder the Rename steps (17,18 and 19)
				---> Create an Age field using DATE_DIFF only.
				---> Drop the c_birth_column, c_login and match_id
				---> Save and Publish this as a new recipe.
				
---> Run the Job and share the results.

<<<<< write the right stuff