## Benchmark Cloud Database (DBench) 云数据库性价比排行榜 FAQ 


### What is the goal of Benchmark Cloud Database (DBench)， 什么是云数据库性价比排行榜?

Benchmark Cloud Database provides objective benchmarks and subjective information to developers, cloud customers, and other users to make informed decisions in choosing cloud databases on popular cloud platforms globaly.

NineData, as a company providing data management service on multi-cloud, regularly receives inquiry from customers about which cloud provider to host their mission critical databases. On one hand, the industrial standardized RDS satisfies roughly 80~90% of such customers; another hand, there are so many variations, from hardware, physical location, Cloud providers, price promotion/adjustments, that confusing everyone. Therefore, NineData decides to setup the framework, and provide the report from the cost-effectiveness perspective.

The DBench serves the cloud database price-performance information, just like the "google flight" for users to compare the air tickets from point A to point B.


### Who can benefit from DBench?

A user chooses cloud managed database on the right host. 


### What methodology DBench use?

The DBench server leverages IaC (Terraform, REST API, etc.) for cloud resource provision with simple architecture of one VM and one enterprise-level DBMS on multiple cloud(currently, 5) and multiple geo-location(currently, 10+). 

A industrial standard benchmark is performed with difference level of concurrency, and its productivity information, such as throughput and latency is collected.

Supported by performance metrics, the report provides two key scores: the sweat spot of productivity, measured by throughput and latency, scaled by concurrency of each RDS each region; the price/cost ratio comparison across RDS of different cloud providers and regions.

The benchmark(s) will be run regularly to capture the latest information, and to demonstrate the historical trend.


### What assumptions DBench take?

* Database systems have matured. For example, the free/open sourced RDBMS, MySQL and PostgreSQL can satisfy majority of small-middle tier customers 
* In cloud-era, OS and DBMS become standard parts. RDS(Relational Database Services) of each provider is built on top of the same code base of open source projects. For Database architecture perspective, the differentiators are minimal. This statement does not apply to proprietary DBMS.
* Customers of cloud database expect to the default setting as optimized one. If fine-tuning is required, the customers would choose to setup the database system themselves instead of paying a premium for an otherwise free database.  
	
### Which benchmarks DBench use?	
	
In the initial stage, the DBench will focus  on OLTP workload: Sysbench (https://en.wikipedia.org/wiki/Sysbench). 

TPC-C (https://tpc.org/） and  customized benchmark  are under development)

### Which cloud providers Benchmark Cloud Database (DBench) test?

Currently, we support the following providers: AWS, Alibaba, GCP, Huawei, Tencent.

	
### Which database configurations does DBench choose? 

**Default** configuration with the following "ideas". 

The project is currently in primary stage. It evaluates two open source Database: PostgreSQL and MySQL(initial released on March 20, 2024， Spring equinox 春分) with target configuration to satisfy small-med enterprise customers, typically (not dedicated) RDS instance with 4vCPU16GB or 8vCPUC32GB. 

As the assumptions discussed above, no tuning is performed except for matching hardware resource (CPU, memory, IO, network) so that no obvious bottleneck. For example, AWS charges IOPS separately, which may inadvertently lead inadequate IO resource.  

Another constrain is the cost, which is almost among the top consideration of CIOs. The annual cost of the RDS will be kept within $8K ~ $15K. 


### How to understand the report?


The report shows output(in term of TPS, transaction per second), ROI (TPS/month-cost) under full productivity situation according the latency and resource usage. In layman's term, within a fix (range) of budget, how much horsepower from a RDS instance one can purchase on each Cloud.


### Can I repeat the benchmark analysis, are you open source? 

YES. Although DBench does not cover all variants, the methodology, documents and most importantly, the scripts with instruction can be found at GitHub with Apache 2.0.

To develop a customized benchmark to march a particular workload, please contact us at @9z.cloud.  



### What's the limitation of the current stage of the DBench project? 

First of all, the initial release(as of March, 2024) is rudimentary, please take it with a grain of salt, and performance tuning is an Art. Due to the virtually unlimited variables, DBench strives to provide a reasonable choice of cloud database from price-performance perspective, but not necessarily the perfect choice.

Currently, DBench is limited to `sysbench`, with a simple testcase OLTP scenario. Please treat it as an indicator instead of Comprehensive performance analysis that mimic real world scenario. 

The resource provision script can be used as a base-line for your reference.


### Database is a sophisticated system, why DBench does not fine-tune the system before running the analysis.

The purpose of cloud and managed-Database (such as RDS) is to offload the borden of database maintenance, operation and tuning to Cloud provider. Therefore, customer can focus the skillset of their IT team to their core business. If customers have to spend their valuable resource to figure out how to best use a RDS， they'd better build a self-managed database instead of paying 40%~300% for an open source(and free) database system. 

Hence, It is up to the cloud providers to setup a RDS  "ready out of the box". A customer who purchase a car from dealership will not tuning a car before drive. 




### How to request that DBench supports additional database, a new hosting provider, a new region or configuration?

Please email to ....@9z.cloud
