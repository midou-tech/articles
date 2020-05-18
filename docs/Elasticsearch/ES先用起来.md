> 少点代码，多点头发

本文已经收录至我的GitHub,欢迎大家踊跃star 和 issues。

https://github.com/midou-tech/articles

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

可能有人根本不知道什么是索引，so 就来了，先学点基本概念。



