- [[Flink]] 把 Connectors 分为三类
-
- DataSet Connectors
	- [[Avro]]
	- [[Hadoop]]
	- [[Azure Tables]]
- DataStream Connectors
	- 形如 [[Kafka]], [[RabbitMQ]] 这样的消息队列
- Table API Connectors
	- 支持 Table alike API 的服务
	- [[Flink]] 有一个 File Systems 抽象，能够从文件系统来消费和存储数据
		- 目前支持 [[s3]]，[[azblob]]，[[gcs]] 等对象存储服务
		- 支持 [[hdfs]] 以及相关的兼容服务
-
- 参考资料
	- [Flink Connectors](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/deployment/filesystems/overview/)