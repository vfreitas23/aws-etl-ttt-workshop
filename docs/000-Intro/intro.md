<h1 id="toc_0" align="center">
Welcome to the ETL Migration & Modernization
<br/>Train-The-Trainer Workshop
</h1>

This workshop contains a set of XXX hands-on labs which complementes the Day One of the ETL Migration & Modernization - Train The Trainer Workshop.

Participants will build an end-to-end ETL Pipeline that will...[add details of the pipeline...]
  
After completing the labs, the participants will have a high level understand of AWS Glue core capabilities as well as they will be able to demonstrate most of the Glue core capabilities. 

<h2 id="toc_0" align="center">
Workshop Parts & Steps
</h2>

#### Part 0 - PRE STEPS
**1.** Setting up Cloud9 Environment Variables  
**2.** Switching Cloud9 Role Credentials  
**3.** Setting up Security Required Groups Inbound Rules  
**4.** Installing Required Libraries (Boto3)

#### Part 1 - TPCDS & RDS MySQL
**1.** Preparing & Generating TPCDS Dataset  
**2.** Populating the Amazon RDS-MySQL Database with TPCDS Dataset

#### Part 2 - GLUE DATABASE, TABLES, CONNECTIONS, CRAWLERS, CLASSIFIERS
**1.** Understanding the Glue Resources Provided (CloudFormation Resources)  
**2.** Running the MySQL-RDS-Crawler  
**3.** Creating a New Crawler (and a Classifier for it!)

#### Part 3 - GLUE (STUDIO) STREAMING
**1.** Understanding the Streaming Resources Provided (CloudFormation & Scripts)  
**2.** Validating Streaming Job Logic and Data (Glue Studio Dummy Job)  
**3.** Creating the Glue Streaming Job (Cloning Jobs!)


#### Part 4 - ORCHESTRATION & DATA ANALYSIS
**1.** Understanding the Orchestration Flow  
**2.** Creating Glue Workflow and Glue Event Based Trigger (via CLI)  
**3.** Creating Event Bridge Rule and Target (via CLI)  
**4.** Starting Orchestration & Exploring and Analyzing Table's Data Cataloged in Glue Data Catalog  
**6.** Creating Views on Top of Cataloged Tables

#### Part 5 - DATA QUALITY & PREPARATION WITH AWS GLUE DATABREW
**1.** Creating Datasets & Profiling the Data (with Quality Rules)  
**2.** Building & Managing DataBrew Recipes  
**3.** Operationalizing a DataBrew Project into a Glue Job.
