title:: Windows Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency
type:: [[Paper]]
conference:: [[SOSP '11]]
doi:: [10.1145/2043556.2043571](https://dl.acm.org/doi/10.1145/2043556.2043571)

- https://sigops.org/s/conferences/sosp/2011/current/2011-Cascais/printable/11-calder.pdf
-
- [[Azure Blobs]]，[[Azure Disks]], [[Azure Files]] 等存储服务都是基于同一套架构搭建的
- 除了传统的存储服务之外，这套架构还支持了
	- [[Azure Queues]]
	- [[Azure Tables]]
-
- 先进，非常先进。
- ---
- 关键特性
	- 强一致
	- 全局可共享的命名空间
	- 灾难恢复
	- 多租户和存储成本
		- WAS 对存储成本的理解是比用户在自有硬件上承载相同的负载要更低
- Global Partitioned Namespace
	- WAS 采用全球统一的命名规则
		- `http(s)://AccountName.<service>.core.windows.net/PartitionName/ObjectName`
		- 当然了，大清自有国情，全球统一不包括大清局域网
-
- ---
- 无用但有趣的一些小发现
	- WAS 很容易手滑打成 AWS (
		- 后来 WAS 把前面的 windows 去掉了，只说 Azure Storage
		- 肯定跟这个没关系 (