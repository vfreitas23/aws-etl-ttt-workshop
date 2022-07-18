<h1 id="toc_0" align="center">
ETL MIGRATION & MODERNIZATION
<br/>TRAIN-THE-TRAINER WORKSHOP
</h1>

This workshop contains a set of seven hands-on labs which complementes the Day One of the ETL Migration & Modernization - Train The Trainer Workshop.

Participants will build an end-to-end ETL Pipeline that will...[add details of the pipeline...]
  
After completing the labs, the participants will have a high level understand of AWS Glue core capabilities as well as they will be able to demonstrate most of the Glue core capabilities. 

<h2 id="toc_0" align="center">
WORKSHOP PARTS & STEPS
</h2>

#### Part 0 - PRE STEPS
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Setting up Cloud9 Environment Variables  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Switching Cloud9 Role Credentials  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.** Setting up Security Required Groups Inbound Rules  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**4.** Installing Required Libraries (Boto3)

#### Part 1 - TPCDS & RDS MySQL
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Preparing & Generating TPCDS Dataset  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Populating the Amazon RDS-MySQL Database with TPCDS Dataset  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.** Unloading Tables (in CSV) from RDS MySQL Database and Uploading to S3


#### Part 2 - AWS GLUE DISCOVERY COMPONENTS (Databases, Tables, Connections, Crawlers and Classifiers)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Understanding the Glue Resources Provided (CloudFormation Resources)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Testing and running pre-created Glue Resources (Connection & MySQL-RDS-Crawler)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.** Creating new Glue Resources (Crawler & Classifier)

#### Part 3 - GLUE (STUDIO) STREAMING
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Understanding the Streaming Resources Provided (CloudFormation & Scripts)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Validating Streaming Job Logic and Data (Glue Studio Dummy Job)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.** Creating the Glue Streaming Job (Cloning Jobs!)

#### Part 4 - ORCHESTRATION & DATA ANALYSIS
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Understanding the Orchestration Flow  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Creating Glue Workflow and Glue Event Based Trigger (via CLI)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.** Creating Event Bridge Rule and Target (via CLI)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**4.** Triggering Orchestration & Following The Flow  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**5.** Exploring and Analyzing Table's Data Cataloged in Glue Data Catalog

#### Part 5 - MACHINE LEARNING WITH GLUE & GLUE STUDIO NOTEBOOKS
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**0.** (Pre Steps) - Understading & Setting up the Resources for the ML Lab  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Creating and Training the Glue FindMatches ML Transform  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Testing FindMatches Transform with Glue Studio Notebook  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.** Deploying & Running a FindMatches Glue Job 

#### Part 6 - WORKFLOW ORCHESTRATION WITH AWS STEP FUNCTIONS
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Creating the Step Function Workflow  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Creating an EventBridge Rule & Target for the Step Function Workflow (via CLI) 

#### Part 7 - DATA QUALITY & PREPARATION WITH AWS GLUE DATABREW
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.** Creating Datasets & Profiling the Data (with Quality Rules)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.** Working with DataBrew Recipes & Projects  
