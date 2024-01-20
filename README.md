# Benchmark Cloud Database Perf Insight with sysbench


This exercise will leverage the multi-threaded benchmark too, [sysbench](https://github.com/akopytov/sysbench), to evaluate managed database on all major cloud provides, including GCP, AWS, AlibabaCloud, Huawei Cloud and Tencent Cloud. 

By conducting a more systematic performance comparison of various cloud database vendors, the goal is to serve as a reference for developers and enterprises when selecting cloud databases. 

The initial stage will use sysbench *oltp_read_write* benchmark for PostgreSQL.


## 1. Environment

### Hardware resource
A compute engine(such as AWS's EC2 and GCP's VM) and a cloud database(such as AWS's RDS-PG and GCP's Cloud SQL-PG) will be allocated within **the same region, and same available-zone** to minimize the network issue.

The exercise chooses the **entry/mid-level enterprise** Both compute engine and database, so that the resource would be power enough for an enterprise workshop, but also not too high so anyone can reproduce the exercise with minimal cost or free credit from the cloud provider. With that, the compute engine will cost $100~200/month, the Database will cost $500~800/month before any discount, and the spec of each resource will be listed in the detail reports. 


## 2. Sysbench

* Version: sysbench 1.0.xx 

### Step 1:  create Database

```
$ psql -h $dbip -p $dbport  -U $dbuser -W -c "CREATE DATABASE t_sysbench";
```

### Step 2: prepare Data: 10 tables, each with 10M rows

```
sysbench --db-driver=pgsql --pgsql-host="$dbip" --pgsql-port="$dbport" --pgsql-user="$dbuser" --pgsql-password="$pw"  --pgsql-db=t_sysbench --table_size=10000000 --tables=10 --events=0 --time=600  --threads=$i --percentile=95 --report-interval=60 oltp_read_write prepare
```

### Step 3: benchmark run

Seven 10-minutes run of oltp_read_write with with different # of concurrent threads, with the focus on TPS, QPS, P95 latency and the CPU usage of the cloud database.

| Name             | Value |
| :---------------- | :------: | 
| scenario        |  oltp\_read\_write   | 
| concurrent threads        |  2,4,8,16,32,64,128   |
| latency           |   P95   |  
| cycle           |   600s   |  
| tables | 10   | 
| table size | 10,000,000   | 


```
#!/bin/bash

# Pls modify accordingly
ofile="sysBench_result"
dbip="1.0.0.0"
dbport="5432"
dbuser="postgres"
pw="notGoingToTellyou"

for ((i=2; i<=128; i=i*2)) 
do 
	echo "Thread = $i"
	echo "######----- $(date) -----######" &>> "$ofile"
	echo "Thread = $i" &>> "$ofile"
	sysbench --db-driver=pgsql --pgsql-host="$dbip" --pgsql-port="$dbport" --pgsql-user="$dbuser" --pgsql-password="$pw"  --pgsql-db=t_sysbench --table_size=10000000 --tables=10 --events=0 --time=600  --threads=$i --percentile=95 --report-interval=60 oltp_read_write run &>> "$ofile"
done
```

## 3. Other notes

### Cost

This exercise will also consider cost of the resource as one of the key factors, and will use the pay-as-you-go model for comparison cross cloud providers. However, some providers carry discounted price. For example, 
At the time of testing, Alibaba Cloud offers discount at 78% for its ECS, so that the list price is 114.69/month but the real cost for this exercise is $89.46. 

It is debatable whether to use cost with or without discount for later comparison. On one hand, Alibaba always carries some kind of discount both for pay-as-you-go(in this case) and subscription by month or years, so discounted price is indeed normal price; on another hand, there is no guarantee of the term of the discounted price for pay-as-you-go. Because this exercise is to provide a guideline for performance and business planning, the full price is used to avoid un-planned cost spike. 

Also because some of the price is listed in Renminbi (CNY), a conversion rate of 7.21(CNY:USD = ￥7.21 : $1) is used so the cost will be presented in US dollar . 