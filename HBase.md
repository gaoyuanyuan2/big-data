# HBase

## 1.HBase 的角色

### 1.1、 HM aster

功能:

1)监控RegionServer

2)处理RegionServer故障转移

3)处理元数据的变更

4)处理region的分配或移除

5)在空闲时间进行数据的负载均衡

6)通过Zookeeper发布自己的位置给客户端

### 1.2、RegionServer

功能:

1)负责存储HBase的实际数据

2)处理分配给它的Region

3)刷新缓存到HDFS

4)维护HLog

5)执行压缩