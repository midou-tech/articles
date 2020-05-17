> 少点代码，多点头发

本文已经收录至我的GitHub,欢迎大家踊跃star 和 issues。

https://github.com/midou-tech/articles

从今天开始准备给大家带来全新的一系列文章，Elasticsearch系列

新系列肯定会有很多疑惑，先为大家答疑解惑，下面是今天要讲的问题

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gevalbk9kkj30yc0u0tcr.jpg)

### 为什么写Elasticsearch系列文章？

之前在文章中也陆陆续续的提到过，龙叔是做搜索引擎的。搜索引擎技术属于商业技术，大家耳熟能详的百度搜索，Google搜索，这可都是因为把握核心搜索技术，从而诞生了商业帝国。

每个互联网大厂都想去分一杯搜索的羹，360搜索、神马、头条、搜狗搜索等等，由此可见搜索技术的商业作用和机密性了。

> 搜索把握用户的入口

蘑菇街的搜索引擎是一款使用C++开发、完全自研、没有开源的搜索引擎，没有开源就是不能随便写出来的。

但是现在不一样了

第一、我离职了，离开了意味着不在持有那些商业机密了，就算不讲出来我也没啥心理负担(但还是不能讲的，离职协议写的很清楚，不能**泄露公司商业机密**)。

第二、去新的公司还是在搜索领域，他们用Es  Elasticsearch是一个开源搜索，开源的东西可以随便说，但还是不能说公司的**商业数据**。

自己一直在搜索领域做，输出搜索相关的文章，第一个可以让自己更好的学习和总结，第二个可以让粉丝们了解到搜索这个神秘的技术，增加大家自身的核心竞争力。

后面会说到，Elasticsearch是搜索引擎，但不简单只能使用在搜索领域，他可以作用的场景非常多。

### Elasticsearch是什么？

Elasticsearch 是一个分布式的开源**搜索**和**分析**引擎，适用于所有类型的数据，包括文本、数字、地理空间、结构化和非结构化数据。

Elasticsearch 在 Apache Lucene 的基础上开发而成，Elasticsearch 以其简单的 REST 风格 API、分布式特性、速度和可扩展性而闻名，是 Elastic Stack 的核心组件。

Elastic Stack 是适用于数据采集、充实、存储、分析和可视化的一组开源工具。人们通常将 Elastic Stack 称为 ELK Stack（代指 Elasticsearch、Logstash 和 Kibana），目前 Elastic Stack 包括一系列丰富的轻量型数据采集代理，这些代理统称为 Beats，可用来向 Elasticsearch 发送数据。

Elasticsearch 的实现原理主要分为以下几个步骤，首先用户将数据提交到Elasticsearch 数据中心，再通过分词控制器去将对应的数据分词，将其权重和分词结果一并存入数据，当用户搜索数据时候，再根据权重将结果排名，打分，再将返回结果呈现给用户。

是什么差不多搞清楚了，再说说ES都哪些成熟的应用以及在哪些领域使用。



### Elasticsearch在哪些领域使用？

- 应用程序搜索
- 网站搜索
- 企业搜索
- 日志处理和分析
- 基础设施指标和容器监测
- 应用程序性能监测
- 地理空间数据分析和可视化
- 安全分析
- 业务分析



### Elasticsearch有哪些特点？

**Elasticsearch 很快。** 由于 Elasticsearch 是在 Lucene 基础上构建而成的，所以在全文本搜索方面表现十分出色。Elasticsearch 同时还是一个近实时的搜索平台，这意味着从文档索引操作到文档变为可搜索状态之间的延时很短，一般只有一秒。因此，Elasticsearch 非常适用于对时间有严苛要求的用例，例如安全分析和基础设施监测。

**Elasticsearch 具有分布式的本质特征。** Elasticsearch 中存储的文档分布在不同的容器中，这些容器称为*分片*，可以进行复制以提供数据冗余副本，以防发生硬件故障。Elasticsearch 的分布式特性使得它可以扩展至数百台（甚至数千台）服务器，并处理 PB 量级的数据。

**Elasticsearch 包含一系列广泛的功能。** 除了速度、可扩展性和弹性等优势以外，Elasticsearch 还有大量强大的内置功能（例如数据汇总和索引生命周期管理），可以方便用户更加高效地存储和搜索数据。

**Elastic Stack 简化了数据采集、可视化和报告过程。** 通过与 Beats 和 Logstash 进行集成，用户能够在向 Elasticsearch 中索引数据之前轻松地处理数据。同时，Kibana 不仅可针对 Elasticsearch 数据提供实时可视化，同时还提供 UI 以便用户快速访问应用程序性能监测 (APM)、日志和基础设施指标等数据。



### 学习Elasticsearch能提高哪些竞争力？

看到Elasticsearch在这么多的领域在使用，特点也这么明显。看到这里估计都不用在说什么核心竞争力，你已经意识到了。

Elastic 于 2018 年 6 月 29 日正式推出 Elastic Certified Engineer 认证考试，认证通过可以获得官方颁发的证书和徽章，title就是 **Elastic认证工程师** 

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gevbch7lomj30u80u0zrz.jpg)

具体认证的细节和含金量，没有具体研究过，但是可以很明显的感受到官方出了这样一个认证，表明社会需要大量这样的人才，而这方面人才的培养和考核指标还欠缺。

**有没有必要一定要考这个认证？**

个人觉得，和英语四六级一样，通过了再说没用。

如果你是学生，可以考虑去考一个认证，因为你很难有业务场景驱使你去做这方面的成长，认证一定是有难度的，一个一个的困难会驱使你成长，最终这个认证也会成为招聘时一个非常大的亮点。

**这个认证会有哪些帮助？**

- 对于快速的构建知识体系帮助。

- 对于全面的熟悉官方文档帮助。

- 对于实战解决线上问题帮助。（遇到了相关技术问题基本上不需要再求助于社区，80%以上的问题自己基本就能解决。）

- 对于增强信心、克服英文恐惧帮助。



### Elasticsearch 支持哪些编程语言？

- Java
- JavaScript (Node.js)
- Go
- .NET (C#)
- PHP
- Perl
- Python
- Ruby



### 哪里可以找到有关 Elasticsearch 的更多信息？

- [Elasticsearch GitHub 存储库：https://github.com/elastic](https://github.com/elastic)
- [Elasticsearch 官方文档：https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Elasticsearch中文社区：https://elasticsearch.cn](https://elasticsearch.cn/)



我是龙叔，一个分享互联网技术和心路历程的star。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gev88f8kfvj30p00dw0tn.jpg)