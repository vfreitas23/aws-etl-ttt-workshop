



[AT FIRST, REPLACE THE IAM ROLE IN THE INSTANCE FOR THE mod-XXXXX-AWSEC2InstanceProfile-XXXXXXX] - refresh the screen

----------[VARIABLES AND PATH SETTINGS - PRE STEPS]---------
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


[DISABLE AWS CREDENTIALS FROM CLOUD 9 CONFIG]

aws sts get-caller-identity
aws configure set region $AWS_REGION
aws configure get region
aws sts get-caller-identity --query Arn | grep AWSEC2ServiceRole--etl-ttt-demo -q && echo "IAM role valid" || echo "IAM role NOT valid"


ref_sg=$(aws ec2 describe-security-groups --query 'SecurityGroups[?contains(GroupName, `aws-cloud9-etl-ttt-demo`) == `true`].GroupId' --output text)
target_sg=$(aws ec2 describe-security-groups --query 'SecurityGroups[?contains(GroupName, `DefaultVPCSecurityGroup`) == `true`].GroupId' --output text)
aws ec2 authorize-security-group-ingress --group-id $target_sg --protocol -1 --source-group $ref_sg
echo "Source SG:" $ref_sg "has been added to Target SG:" $target_sg


[INSTALL BOTO3 FOR STREAMIN LAB FURTHER]

sudo pip3 install boto3


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

#---[NOT NEED THIS]
#---cd ~/environment/glue-workshop
#---aws s3 cp --recursive ~/environment/glue-workshop/code/ s3://$BUCKET_NAME/script/
--------------------------------------------------------------------
