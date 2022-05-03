<h1 id="toc_0" align="center">
Working with TPC-DS Data & RDS MySQL Database
</h1>

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<



#### **1.** Preparing & Generating TPCDS Dataset  


<<<<< write the right stuff

[TPCDS DATA CONFIGURATION DEMO PART]


tpcds - This data is used for a decision support benchmark. When you load this data the schema tpcds is updated with sample data. For more information about the tpcds data, see TPC-DS

<<<<<<

~~~shell
mkdir -p ~/environment/ttt-demo
cd ~//environment/ttt-demo//
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/16d7423c-23d3-4185-8ab4-5b002ec51153-tpc-ds-tool.zip tpc-ds-tool.zip
~~~

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<

~~~shell
unzip tpc-ds-tool.zip 
cd DSGen-software-code-3.2.0rc1/tools/
make
head -200 tpcds.sql 
~~~

[WAIT FOR ABOUT 3 MIN TO COMPLETE]

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<

~~~shell
mkdir -p /tmp/dsd/csv_tables/
./dsdgen -scale 1 -dir /tmp/dsd -parallel 2 -child 1
cd /tmp/dsd/
ls -lrt
~~~

Testing RDS MySQL Database Connection - (OPTIONAL)

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<

~~~shell
sudo yum -y install telnet
mysqlendpoint=$(aws cloudformation describe-stacks --query 'Stacks[*].Outputs[?OutputKey==`MySQLEndpoint`].OutputValue | [0] | [0]' --output text)
telnet $mysqlendpoint 3306
~~~

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<


#### **2.** Populating the Amazon RDS-MySQL Database with TPCDS Dataset  

<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<

~~~shell
tpcds_script_path=~//environment/ttt-demo//DSGen-software-code-3.2.0rc1/tools/tpcds.sql 
mysql -h ${mysqlendpoint} -u etluser -petltttdemopwd -Dtpcds < $tpcds_script_path
mysql -h ${mysqlendpoint} -u etluser -petltttdemopwd -Dtpcds -e "show tables"
~~~

[BELOW SCRIPT IS STORED IN THE EVENT ENGINE MODULE ASSET BUCKET]

~~~shell
mysqlendpoint=$1
DIR=/tmp/dsd
ls $DIR/*.dat | while read file; do
pipe=$file.pipe
mkfifo $pipe
table=`basename $file .dat | sed -e 's/_[0-9]_[0-9]//'`
echo $file $table
LANG=C && sed -e 's_^|_\\N|_g' -e 's_||_|\\N|_g' -e 's_||_|\\N|_g' $file > $pipe & \
mysql -h ${mysqlendpoint} -u etluser -petltttdemopwd --local-infile -Dtpcds -e \
"load data local infile '$pipe' replace into table $table character set latin1 fields terminated by '|'"
rm -f $pipe
done
~~~


<<<<< write the right stuff

The following commands will blah blah lah blah

<<<<<<

~~~shell
aws s3 cp s3://ee-assets-prod-us-east-1/modules/31e125cc66e9400c9244049b3b243c38/v1/downloads/etl-ttt-workshop/load_TPC-DS_MySQL.sh .
. load_TPC-DS_MySQL.sh $mysqlendpoint
~~~

#### **3.** Unloading Tables (in CSV) from RDS MySQL Database and Uploading to S3


<<<<< write the right stuff
[EXTRACT TABLES FOR THE LABS INTO CSV FORMAT AND UPLOADING IT INTO S3]

The following commands will blah blah lah blah

<<<<<<

~~~shell
mysql -h ${mysqlendpoint} -u etluser -petltttdemopwd -Dtpcds --batch -e "select * from web_page" | sed 's/\t/","/g;s/^/"/;s/$/"/;s/\n//g' > /tmp/dsd/csv_tables/web_page.csv
mysql -h ${mysqlendpoint} -u etluser -petltttdemopwd -Dtpcds --batch -e "select * from customer" | sed 's/\t/","/g;s/^/"/;s/$/"/;s/\n//g' > /tmp/dsd/csv_tables/customer.csv
mysql -h ${mysqlendpoint} -u etluser -petltttdemopwd -Dtpcds --batch -e "select * from income_band" | sed 's/\t/","/g;s/^/"/;s/$/"/;s/\n//g' > /tmp/dsd/csv_tables/income_band.csv
aws s3 cp --recursive /tmp/dsd/csv_tables/ s3://$BUCKET_NAME/etl-ttt-demo/csv_tables/
~~~









