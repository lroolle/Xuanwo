title:: Data Layers: I have a dream

-
- 期望
	- ```sql
	  -- Create Stage
	  create stage test
	        type = S3
	        credential = 'hmac:ak:sk'
	        endpoint = 'https://s3.awsamazon.com'
	        bucket = 'bucket_name'
	        region = 'us-west-1'
	        root = '/path/to/data';
	  ```
	- ```sql
	  -- Copy data from stage into table
	  copy into test_csv from '@test/path/to/data'
	        format CSV
	        record_delimitor = ',';
	        
	  copy into 
	  ```
- 现状
	- Table Trait
	- Engine
		- Fuse (file based)
		- Github
		- Memory
	- Stream Source
		- CSV
		- Parquet
		- Value
- 用户故事
	- 将 X 存储上的 Y 数据导入到 Databend
	- X = s3/gcs/oss
	- Y = avro/parquet/csv/orc
- 想法
	- Features
		- Query Pushdown
			- S3 Selcet
			- Big Query?
	- Data Connector
		- format: [[Avro]], [[Parquet]], [[CSV]], httpd/nginx log, mysql dump files
		- storage: fs/[[nfs]]/nas/[[s3]]/[[gcs]]/oss/[[ipfs]] -> DAL
- 规划
	- DAL2
		- Init String
	- Cloud Native Storage Format
	- ```sql
	  copy into default.test from '@stage' format parquet;
	  ```
	-
- 现在的进展
	- DAL2 refactor
		- implement s3 support
- 参考资料
	- [Prestodb Connectors](https://prestodb.io/docs/current/connector.html)
	- [[Apache Flink/Connectors]]
	- [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro.html)
-