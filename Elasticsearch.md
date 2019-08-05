# Elasticsearch

## 电商平台的分布式搜索引擎

1、实现方式

1) Lucene+ 自己封装

2) Solr

3) ElasticSearch


## 为什么要选择Elasticsearch

Elasticsearch优点

1、Elasticsearch是分布式的。不需要其他组件，分发是实时的，被叫做”Push replication”。

2、Elasticsearch 完全支持Apache Lucene的接近实时的搜索。

3、处理多租户(multitenancy) 不需要特殊配置，而Solr则需 要更多的高级设置。

4、默认就支持GE0 Hash算法。


缺点

1、只有一名开发者(当前Elasticsearch GitHub组织已经不只如此，已经有了相当活跃的维护者)

2、还不够自动(不适合当前新的Index Warmup API (热索引) )


Apache Solr

优点

1、Solr有一个更大、更成熟的用户、开发和贡献者社区。

2、支持添加多种格式的索引，如: HTML、PDF、微软Office系列软件格式以及JSON、XML、CSV等纯文本格式。3、Solr比较成熟、稳定。

4、不考虑建索引的同时进行搜索，速度更快。

缺点

1、建立索引时，搜索效率下降，实时索引搜索效率不高。


## Elasticsearch与Solr的比较总结

1、二者安装都很简单;

2、Solr 利用Zookeeper进行分布式管理，而Elasticsearch自身带有分布式协调管理功能;  I

3、Solr支持更多格式的数据，而Elasticsearch仅支持json文件格式;

4、Solr 官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能多有第三方插件提供(有些第三方插件是收费的);

5、Solr在传统的搜索应用中表现好于Elasticsearch,但在处理实时搜索应用时效率明显低于Elasticsearch。

6、Solr是传统搜索应用的有力解决方案，但Elasticsearch更适用于新兴的实时搜索应用。


## Elasticsearch工作原理

1、Lucene 是一个JAVA搜索类库，它本身并不是一个完整的解决方案，需要额外的开发工作。

2、Document文档存储、文本搜索。

3、Index索引，聚合检索。

4、Analyzer分 词器，如IKAnalyzer、 word分词、Ansj、 Stanford等5、大数据搜索引擎解决方案原理

6、NoSQL的兴起( Redis、MongoDB、 Memecache )


![](https://github.com/gaoyuanyuan2/big-data/blob/master/img/12.png)