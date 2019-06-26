# Hadoop

## 安装

https://blog.51cto.com/4837471/2140674

## Hadoop 2.x

Hadoop2.0以后的版本移除了原有的JobTracker和TaskTracker ,改由YARN平台的ResourceManager负责集群中所有资源的统一管理和分配, NodeManager管理Hadoop集群中单个计算节点。

YARN的设计减小了JobTracker的资源消耗, 减少了Hadoop1.0中发生单点故障的风险。我们还可以在YARN平台上运行Spark和Storm作业, 充分利用资源。

## Hadoop的组成

包括两个核心组成:

### HDFS:分布式文件系统,存储海量的数据

HDFS的特点

1、数据冗余,硬件容错

2、流式的数据访问

3、存储大文件

适合数据批量读写,吞吐量高

适合一次写入多次读取,顺序读写

不支持多用户并发写相同文件


适合

大规模数据

流式数据（写一次，读多次）

商用硬件（一般硬件）


不适合

低延时的数据访问

大量的小文件

频繁修改文件（基本就是写1次）



HDFS: 分布式文件存储

YARN: 分布式资源管理

MapReduce: 分布式计算

Others: 利用YARN的资源管理功能实现其他的数据处理方式

内部各个节点基本都是采用Master-Woker架构




HDFS是Hadoop分布式文件系统的简称,由若干台计算机组成,用于存放PB、TB数量级以上的文件, 每份文件可以有多个副本,所以HDFS是一个具有高冗余、高容错的文件系统。


### MapReduce :  并行处理框架,实现任务分解和调度

分而治之,一个大任务分成多个小的子任务( map )并行执行后,合并结果(reduce)

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/4.png)

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/5.png)

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/6.png)

JobTracker的角色

1. 作业调度

2. 分配任务、监控任务执行进度

3. 监控TaskTracker的状态

TaskTracker的角色

1. 执行任务

2. 汇报任务状态

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/7.png)

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/8.png)

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/9.png)


MapReduce的四个阶段

1.Split阶段

2.Map阶段 (需要编码)

3.Shuffle阶段

4.Reduce阶段 (需要编码)


![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/10.png)

理想的输入文件

由于NameNode内存有限,大量的小文件会给Hdfs带来性能上的问题。故Hdfs适合存放大文件,对于大量小文件,可以采用压缩、合并小文件的优化策略。例如,设置文件输入类型为CombineFileInputFormat格式



节点Map任务的个数

在实际情况下, map任务的个数是受多个条件的制约, 一般一个DataNode的map任务数量控制在10到100比较合适。

增加map个数,可增大`mapred.map.tasks` 

减少map个数,可增大`mapred.min.split.size`

如果要减少map个数,但有很多小文件,可将小文件合并成大文件,再使用准则2


本地优化  Combine

数据经过Map端输出后会进行网络混洗,经Shuffle后进入Reduce ,在大数据量的情况下可能会造成巨大的网络开销。故可以在本地先按照key先行一轮排序与合并,再进行网络混洗,这个过程就是Combine。

在多数情况下, Combine的逻辑和Reduce的逻辑一致的，即都是按照key合并数据,故可以认为Combine是对本地数据的Reduce操作。这里复用Reducer的逻辑,也可以自己实现Combiner类。

`job.setCombinerClass(MyReduce.class);` //设置Combine

`job.setReducerClass(MyReduce.class);` // 设置Reducer

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/11.png)


一个MapReduce作业中,以下三者的数量总是相等的:

1. Partitioner的数量

2. Reduce任务的数量

3. 最终输出文件(如: part-r-00000 )

在一个Reducer中,所有数据都会被按照key值升序排序，故如果part输出文件中包含key值,则这个文件一定是有序的。


Reduce任务数量

在大数据量的情况下,如果只设置1个reduce任务,那么在reduce阶段,整个集群只有该节点在运行reduce任务, 其他节点都将被闲置,效率十分低下。
故建议将reduce任务数量设置成一个较大的值(最大值是72 )。


一个节点上的Reduce任务数并不像Map任务数那样受多个因素制约。

1. 可以通过调节 参数`mapred.reduce.tasks` ;

2. 可以在代码中调用`job.setNumReduce Tasks(int n)`方法。



Mapper  Shuffle  Reducer

1. Combine本质 上是在Mapper缓冲区溢写文件的合并。

2 . Partition是在Reduce输入之前发生 ,相同的key值一定会进入同一个Partitioner , Reduce过程会按照key排序。

3. Partitioner、 Reducer、 输出文件三者数量是相等的。

4. 大数据量的情况下, Reducer数量不宜过少，可以通过

 `job.setNumReduceTasks(int n)`和`mapred.reduce.tasks`设置数量




## Hadoop可以用来做什么?

搭建大型数据仓库, PB级数据的存储、处理、分析、统计等业务

人物画像，精准营销，商品推荐

## 优点

高扩展 低成本 成熟生态圈


##  基本概念

### 块( Block )

### NameNode

是管理节点,存放文件元数据

1. 文件与数据块的映射表

2. 数据块与数据节点的映射表

### DataNode

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/2.png)

![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/3.png)







