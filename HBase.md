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

6)负责处理 Region 分片

### 组件：

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/1.jpg)

1) Write-Ahead logs

HBase 的修改记录，当对 HBase 读写数据的时候，数据不是直接写进磁盘，它会在内存中
保留一段时间（时间以及数据量阈值可以设定）。但把数据保存在内存中可能有更高的概率
引起数据丢失，为了解决这个问题，数据会先写在一个叫做 Write-Ahead logfile 的文件中，
然后再写入内存中。所以在系统出现故障的时候，数据可以通过这个日志文件重建。

2) HFile

这是在磁盘上保存原始数据的实际的物理文件，是实际的存储文件。

3) Store

HFile 存储在 Store 中，一个 Store 对应 HBase 表中的一个列族。

4) MemStore

顾名思义，就是内存存储，位于内存中，用来保存当前的数据操作，所以当数据保存在 WAL
中之后， RegsionServer 会在内存中存储键值对。

5) Region

Hbase 表的分片，HBase 表会根据 RowKey 值被切分成不同的 region 存储在 RegionServer 中，
在一个 RegionServer 中可以有多个不同的 region。