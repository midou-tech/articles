> 少点代码，多点头发

本文已经收录至我的GitHub,欢迎大家踊跃star 和 issues。

https://github.com/midou-tech/articles

看文章有两点需要注意：

- 本公号讲解的Elasticsearch是基于7.7.0版本，你们在阅读一些相关书籍和博客注意版本，不同版本很多概念会有出入。

- 文章写作过程中会经常将Elasticsearch简写为Es，阅读过程中需要注意。

一般学习一个新的技术或者产品，第一步就是用起来。什么设计理论，框架源码，都别和我谈，先**run**起来。这也是在公司看别人项目的绝招。

用起来，有一个很明显的点，是你能感受到他，不然天天看理论知识，看源码会让你觉得你好像懂了，但又心里没底，最终会导致你走火入魔。

今天龙叔的主题就是 **学Es，先run起来**，用起来之后在去探索内部更多问题和原理。

![Elasticsearch](https://tva1.sinaimg.cn/large/007S8ZIlly1gex0aitcu7j30zc0iignh.jpg)



### Elasticsearch安装

Elasticsearch 的底层是开源库 [Lucene](https://lucene.apache.org/)。但是，你没法直接用 Lucene，必须自己写代码去调用它的接口。Elasticsearch 是 Lucene 的封装，提供了 REST API 的操作接口，开箱即用。

Elasticsearch 需要 Java 8 环境。如果你的机器还没安装 Java，可以在网上找个教程安装，注意要保证环境变量`JAVA_HOME`正确设置。

安装完 Java，就可以跟着 [官方文档：https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html) 安装 Elasticsearch。我这里就直接下载压缩包比较简单。

```sh
#mac和linux上安装教程一样的
# 下载
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.7.0-darwin-x86_64.tar.gz
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.7.0-darwin-x86_64.tar.gz.sha512
$ shasum -a 512 -c elasticsearch-7.7.0-darwin-x86_64.tar.gz.sha512 
#解压
$ tar -xzf elasticsearch-7.7.0-darwin-x86_64.tar.gz
$ cd elasticsearch-7.7.0/
```

接着，进入解压后的目录，运行下面的命令，启动 Elasticsearch。

```sh
$ ./bin/elasticsearch
```

如果一切正常，那可能是run起来了，Es默认打开9200端口。测试下是否启动成功，用 curl 工具测试（这个工具后面会写一篇文章介绍，还有上面用的wget），也可以在浏览器访问。

```sh
$ curl localhost:9200 #测试命令
{
  "name" : "MacBook-Pro.local",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "Z1NxCjE4T6CgTjZmpAVe_A",
  "version" : {
    "number" : "7.7.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "81a1e9eda8e6183f5237786246f6dced26a10eaf",
    "build_date" : "2020-05-12T02:01:37.602180Z",
    "build_snapshot" : false,
    "lucene_version" : "8.5.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

请求9200端口，Elastic 返回一个 JSON 对象，包含当前节点、集群、版本等信息。

收到这样一个JSON对象，说明启动成功。

安装整体没什么压力，java环境装好，基本就是开箱即用。程序员最喜欢使用这样的中间件，开箱即用，从不管箱子里面是啥。

### 基本概念

本来run起来就准备说搞点数据进去，在和Es进行交互起来，但是正在准备写数据进索引的时候，发现不对劲。

可能有人根本不知道什么是索引？什么Document。于是 就来了，先普及下基本概念。



####  节点（Node） 与集群（ Cluster）

Elastic 本质上是一个分布式数据库，允许多台服务器协同工作，每台服务器可以运行多个 Elastic 实例。

单个 Elastic 实例称为一个节点（node）。一组节点构成一个集群（cluster）。

#### 索引（Index）

Elastic 会索引所有字段，经过处理后写入一个反向索引（Inverted Index），也经常称之为倒排索引。查找数据的时候，直接查找该索引。

Elastic 数据管理的顶层单位就叫做 Index（索引）。它是单个数据库的同义词。每个 Index （即数据库）的名字必须是小写。

#### 文档（Document）

Index 里面单条的记录称为 Document（文档）。许多条 Document 构成了一个 Index。



### 写点数据进Es

基本概念已经有了，知道查找是通过倒排索引进行的，所以数据肯定是存放在索引里面的。

我们现在要写数据进Es，其实就是把数据写到Es的索引（index）中，前面已经把Es启动起来了，并没有创建索引。

今天写数据就不写代码了，利用ES的一些封装很好的接口，直接命令行操作，后期在用代码写数据进Es。

先创建一个index ，使用curl 工具在命令行操作，这是一个put请求。

```sh
$curl -X PUT 'localhost:9200/user'
```

查看索引是否以及创建成功

```sh
$ curl -X GET 'http://localhost:9200/_cat/indices?v'
```

这个get请求可以查看当前节点的所有索引

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gf3jqekoayj319i030jrt.jpg)

妥妥的已经创建成功

顺便说下，删除一个索引的命令，DELETE参数表示删除

```sh
$curl -X DELETE 'localhost:9200/user'
```

到这里索引已经创建好了，可以写点数据进去了。使用接口 /index/_doc/id  ，/索引名/\_doc/doc\_id

```sh
$ curl -X PUT -H 'Content-Type: application/json' 'localhost:9200/user/_doc/1' -d '
{
  "name": "龙跃十二",
  "title": "工程师",
  "desc": "一个分享互联网技术和心路历程的star"
}'
```

查看当前索引下的所有数据

```sh
$ curl 'localhost:9200/user/_search?pretty=true
```

到这里基本我们已经可以写数据到指定索引了，生产场景不会这么写数据的，都是用代码写海量数据进ES的，这就几条数据也没什么搜索性能可谈的。

我之前工作中日志数据都是TB级别的写到Es中，当遇到这种数据量的搜索时才会感受到搜索引擎的魅力，才会意识到Es的重要性。

这里主要是练手和跑通流程，所以造了一些数据到Es中



### 和ES进行交互

其实写数据进Es已经是一种交互了，在讲一些其他的交互接口

目前讲的交互方式主要是通过原生的请求的方式，还没有上升到界面操作，后期在学习的过程中会展现出来。

#### 查询交互

使用 GET 方法，直接请求`/Index/_search`，就会返回所有记录。

```sh
$ curl 'localhost:9200/user/_search?pretty=true'
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "user",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "龙跃十二",
          "title" : "工程师",
          "desc" : "一个分享互联网技术和心路历程的star"
        }
      },
      {
        "_index" : "user",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "name" : "三y",
          "title" : "工程师",
          "desc" : "只有光头才能变得更强"
        }
      },
      {
        "_index" : "user",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "name" : "敖丙",
          "title" : "工程师",
          "desc" : "一个互联网苟且偷生的工具人"
        }
      }
    ]
  }
}

```

上面代码中，返回结果的 `took`字段表示该操作的耗时（单位为毫秒），`timed_out`字段表示是否超时，`hits`字段表示命中的记录，里面子字段的含义如下。

- total：返回记录数，本例是2条。
- max_scor：最高的匹配程度，本例是1.0。
- hits：返回的记录组成的数组。

返回的记录中，每条记录都有一个`_score`字段，表示匹配的程序，默认是按照这个字段降序排列。

Es的查询语法还有很多，后面在结合实战项目的时候会讲解其他语法，你也可以看下官网语法介绍 [官网查询语法](https://www.elastic.co/guide/en/elasticsearch/reference/7.7/query-dsl.html)。



#### 数据操作交互

新增一条doc记录的语法示例如下，可以不用指定doc_id的，Es会默认有一个doc_id。

```sh
$ curl -X PUT -H 'Content-Type: application/json' 'localhost:9200/user/_doc/2' -d '
{
  "name": "敖丙",
  "title": "工程师",
  "desc": "一个互联网苟且偷生的工具人"
}'
```



删除一条doc记录的语法是 ` /Index/_doc/doc_id `

```sh
$ curl -X DELETE  'localhost:9200/user/_doc/1'
```



更新一条记录的语法示例

```sh
$ curl -X PUT -H 'Content-Type: application/json' 'localhost:9200/user/_doc/2' -d '
{
  "name": "三太子敖丙",
  "title": "工程师",
  "desc": "一个互联网苟且偷生的工具人"
}'
```



### 总结一下

本篇文章，我们把Es从官网下载下来，可以run起来，可以写数据进去，可以查询，学习了一些简单的交互语法。

当然Es的魅力不在于此，Es的魅力之一在于可以对**海量数据**进行**高效**的检索。

下篇文章出一个关于Es的写作大纲，方便大家在看的过程中有一个整理的轮廓。

Es整个知识点我也是边学边写，有什么不对的地方，还希望大佬们尽管指出来。



![龙跃十二](https://tva1.sinaimg.cn/large/00831rSTly1gdjy2023y3j30p00dw0tn.jpg)