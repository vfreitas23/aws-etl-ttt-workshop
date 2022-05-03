



<h1 id="toc_0" align="center">
Preparing the Cloud9 Enviroment
</h1>


We will use AWS Cloud9 to run shell commands, edit and run Python scripts for the labs. Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It combines the rich code editing features of an IDE such as code completion, hinting, and step-through debugging, with access to a full Linux server for running and storing code.

Go to the AWS Cloud9 console in your environment and you should see a Cloud9 with name glueworkshop. Click Open IDE button to enter Cloud9 IDE environment.

<LINK> Cloud 9 console - In Cloud9, close the welcome tab then click on the menu bar Window and then New Terminal. This will create a new tab and load a command-line terminal.

You will use this terminal window throughout this pre-step lab to execute the AWS CLI commands and scripts.

new Cloud9 terminal


During the workshop environment setup, a S3 bucket is created for storing lab files and CloudTrail logs. Copy following command to Cloud9 terminal window to get your lab S3 bucket name to ${BUCKET_NAME}. Please save the ${BUCKET_NAME} value, you will be using it through out the workshop.

[detail more what we are doing below]


#### **1.** Setting up Cloud9 Environment Variables  


[AT FIRST, REPLACE THE IAM ROLE IN THE INSTANCE FOR THE mod-XXXXX-AWSEC2InstanceProfile-XXXXXXX] - refresh the screen

~~~shell
 AWS_ACCOUNT_ID=`aws sts get-caller-identity --query Account --output text`
 AWS_REGION=`aws configure get region`
 BUCKET_NAME=etl-ttt-demo-${AWS_ACCOUNT_ID}-${AWS_REGION}
 mysqlendpoint=$(aws cloudformation describe-stacks --query 'Stacks[*].Outputs[?OutputKey==`MySQLEndpoint`].OutputValue | [0] | [0]' --output text)
 echo "export BUCKET_NAME=\"${BUCKET_NAME}\"" >> /home/ec2-user/.bashrc
 echo "export AWS_REGION=\"${AWS_REGION}\"" >> /home/ec2-user/.bashrc
 echo "export AWS_ACCOUNT_ID=\"${AWS_ACCOUNT_ID}\"" >> /home/ec2-user/.bashrc
 echo ${BUCKET_NAME}
 echo ${AWS_REGION}
 echo ${AWS_ACCOUNT_ID}
 echo $mysqlendpoint  
~~~ 


<<<<< write the right stuff

The following commands will download and unzip the lab files to your Cloud9 environment, download and zip the 3rd party Python library, upload lab files to your S3 bucket, then copy the COVID-19 dataset to your S3 bucket.
We will use data from a public COVID-19 dataset curated by AWS. We will show details about how to access and exchange data on AWS platform Access COVID-19 Dataset. If you are interested in learning more about this COVID-19 dataset, read this AWS blog
for more information. The 3rd party Python library is from github
and you could find details about the library there.

<<<<<<


#### **2.** Switching Cloud9 Role Credentials  

[DISABLE AWS CREDENTIALS FROM CLOUD 9 CONFIG]

~~~cli
> aws sts get-caller-identity
> aws configure set region $AWS_REGION
> aws configure get region
> aws sts get-caller-identity --query Arn | grep AWSEC2ServiceRole--etl-ttt-demo -q && echo "IAM role valid" || echo "IAM role NOT valid"
~~~


#### **3.** Setting up Security Required Groups Inbound Rules  

~~~shell
ref_sg=$(aws ec2 describe-security-groups --query 'SecurityGroups[?contains(GroupName, `aws-cloud9-etl-ttt-demo`) == `true`].GroupId' --output text)
target_sg=$(aws ec2 describe-security-groups --query 'SecurityGroups[?contains(GroupName, `DefaultVPCSecurityGroup`) == `true`].GroupId' --output text)
aws ec2 authorize-security-group-ingress --group-id $target_sg --protocol -1 --source-group $ref_sg
echo "Source SG:" $ref_sg "has been added to Target SG:" $target_sg
~~~
 
#### **4.** Installing Required Libraries (Boto3)  

[INSTALL BOTO3 FOR STREAMIN LAB FURTHER]

~~~shell
cli sudo pip3 install boto3
~~~

<!-- 

-----------[NOT NEED TO DO THE FOLLOWING THINGS - GET ONLY NECESSARY FILE - MOVE IT TO MY OWN ASSETS BUCKET]
cd ~/environment
aws s3 cp s3://ee-assets-prod-us-east-1/modules/aa287fde7dd448ffac85ed7824e5c1f0/v7/download/glue-workshop.zip glue-workshop.zip
unzip glue-workshop.zip
mkdir ~/environment/glue-workshop/library
mkdir ~/environment/glue-workshop/output
mkdir -p /tmp/dsd/csv_tables/

[NOT NEED THIS...]
git clone https://github.com/jefftune/pycountry-convert.git
cd ~/environment/pycountry-convert
zip -r pycountry_convert.zip pycountry_convert/
mv ~/environment/pycountry-convert/pycountry_convert.zip ~/environment/glue-workshop/library/

---[NOT NEED THIS]
---cd ~/environment/glue-workshop
---aws s3 cp --recursive ~/environment/glue-workshop/code/ s3://$BUCKET_NAME/script/
--------------------------------------------------------------------

-->


Go to the AWS S3 console and explore the bucket ${BUCKET_NAME}. [AND OTHER SETTINGS YOU DID!!!] You should see the following prefixes in the bucket. Take some time to explore the content in the bucket.

You are finished setting up the workshop environment and can move on to Lab 01 now.
