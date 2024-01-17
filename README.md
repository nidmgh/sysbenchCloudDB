# Benchmark Cloud Database Perf Insight with sysbench


This exercise will leverage the multi-threaded benchmark too, [sysbench](https://github.com/akopytov/sysbench), to evaluate managed database on all major cloud provides, including GCP, AWS, AlibabaCloud, Huawei Cloud and Tencent Cloud. 

By conducting a more systematic performance comparison of various cloud database vendors, the goal is to serve as a reference for developers and enterprises when selecting cloud databases. 

The initial stage will use sysbench *oltp_read_write* benchmark for PostgreSQL.


## Environment

### Hardware resource
A compute engine(such as AWS's EC2 and GCP's VM) and a cloud database(such as AWS's RDS-PG and GCP's Cloud SQL-PG) will be allocated within **the same region, and same available-zone** to minimize the network issue.

The exercise chooses the **entry/mid-level enterprise** Both compute engine and database, so that the resource would be power enough for an enterprise workshop, but also not too high so anyone can reproduce the exercise with minimal cost or free credit from the cloud provider. With that, the compute engine will cost $100~200/month, the Database will cost $500~800/month before any discount, and the spec of each resource will be listed in the detail reports. 


## sysbench

* Version: sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

### Step 1:  create Database

```
$ psql -h $dbip -p $dbport  -U postgres -W -c "CREATE DATABASE t_sysbench";
```
### Step 2: prepare Data: 10 tables, each with 10M rows

```
sysbench --db-driver=pgsql --pgsql-host="$dbip" --pgsql-port="$dbport" --pgsql-user="$dbuser" --pgsql-password="$pw"  --pgsql-db=t_sysbench --table_size=10000000 --tables=10 --events=0 --time=600  --threads=$i --percentile=95 --report-interval=60 oltp_read_write prepare
```

### Step 3: benchmark run

| Name             | Value |
| :---------------- | :------: | 
| scenario        |  oltp\_read\_write   | 
| concurrent threads        |  2,4,8,16,32,64,128   |
| cycle           |   600s   | 
| break    |  20s   | 
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
	sleep 20s
done
```

