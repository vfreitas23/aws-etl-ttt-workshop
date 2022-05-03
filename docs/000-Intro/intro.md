<h1 id="toc_0" align="center">
ETL MIGRATION & MODERNIZATION
<br/>TRAIN-THE-TRAINER WORKSHOP
</h1>

This workshop contains a set of XXX hands-on labs which complementes the Day One of the ETL Migration & Modernization - Train The Trainer Workshop.

Participants will build an end-to-end ETL Pipeline that will...[add details of the pipeline...]
  
After completing the labs, the participants will have a high level understand of AWS Glue core capabilities as well as they will be able to demonstrate most of the Glue core capabilities. 

<h2 id="toc_0" align="center">
WORKSHOP PARTS & STEPS
</h2>

#### Part 0 - PRE STEPS
**1.** Setting up Cloud9 Environment Variables  
**2.** Switching Cloud9 Role Credentials  
**3.** Setting up Security Required Groups Inbound Rules  
**4.** Installing Required Libraries (Boto3)

#### Part 1 - TPCDS & RDS MySQL
**1.** Preparing & Generating TPCDS Dataset  
**2.** Populating the Amazon RDS-MySQL Database with TPCDS Dataset  
**3.** Unloading Tables (in CSV) from RDS MySQL Database and Uploading to S3


#### Part 2 - AWS GLUE COMPONENTS (Databases, Tables, Connections, Crawlers and Classifiers)
**1.** Understanding the Glue Resources Provided (CloudFormation Resources)  
**2.** Testing and running pre-created Glue Resources (Connection & MySQL-RDS-Crawler)
**3.** Creating new Glue Resources (Crawler & Classifier)

#### Part 3 - GLUE (STUDIO) STREAMING
**1.** Understanding the Streaming Resources Provided (CloudFormation & Scripts)  
**2.** Validating Streaming Job Logic and Data (Glue Studio Dummy Job)  
**3.** Creating the Glue Streaming Job (Cloning Jobs!)


#### Part 4 - ORCHESTRATION & DATA ANALYSIS
**1.** Understanding the Orchestration Flow  
**2.** Creating Glue Workflow and Glue Event Based Trigger (via CLI)  
**3.** Creating Event Bridge Rule and Target (via CLI)  
**4.** Triggering Orchestration & Following The Flow  
**5.** Exploring and Analyzing Table's Data Cataloged in Glue Data Catalog

#### Part 5 - DATA QUALITY & PREPARATION WITH AWS GLUE DATABREW
**1.** Creating Datasets & Profiling the Data (with Quality Rules)  
**2.** Building & Managing DataBrew Recipes  
**3.** Operationalizing a DataBrew Project into a Glue Job.
