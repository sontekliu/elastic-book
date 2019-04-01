# ElasticSearch 安装及基本概念

### 1. 安装 ElasticSearch

#### 1.1 下载并解压

```shell
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.0.tar.gz
$ tar -zxvf elasticsearch-6.7.0
$ ls -l
total 896
drwxr-xr-x  45 sontek  staff    1440 Mar 21 23:34 bin
drwxr-xr-x   9 sontek  staff     288 Mar 21 23:34 config
drwxr-xr-x  42 sontek  staff    1344 Mar 21 23:34 lib
drwxr-xr-x   2 sontek  staff      64 Mar 21 23:34 logs
drwxr-xr-x  31 sontek  staff     992 Mar 21 23:34 modules
drwxr-xr-x   2 sontek  staff      64 Mar 21 23:34 plugins
-rw-r--r--   1 sontek  staff   13675 Mar 21 23:29 LICENSE.txt
-rw-r--r--   1 sontek  staff  427502 Mar 21 23:34 NOTICE.txt
-rw-r--r--   1 sontek  staff    8519 Mar 21 23:29 README.textile
```

#### 1.2 启动 ES

```shell
$ ./bin/elasticsearch
```

如果一切正常，那么 ES 就会在默认的 9200 端口运行，此时打开浏览器可请求该端口。或者使用 curl 命令也可以，如想让系统以守护进程启动，添加 `-d` 参数即可。 `$ ./bin/elasticsearch -d`

```shell
$ curl localhost:9200
{
  "name" : "ctHNbGD",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "cjtHUz12RViYzSua9a0uIw",
  "version" : {
    "number" : "6.7.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "8453f77",
    "build_date" : "2019-03-21T15:32:29.844721Z",
    "build_snapshot" : false,
    "lucene_version" : "7.7.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

在上面的代码中，ES 返回一个 JSON 对象，其中包含当前节点、集群、版本等信息。

#### 1.3 远程访问 ES  

默认情况下，ES 只允许本机进行访问，如需要远程访问，需修改 ES 配置文件。将 ES 安装目录下的 `config/elasticsearch.yml` 文件中的 `network.host` 注释去掉，并将其值改成`0.0.0.0`，然后重新启动即可。

```shell
network.host: 0.0.0.0
```

### 2. ES 基本概念

#### 2.1 Cluster 和 Node
ElasticSearch 是一个分布式搜索引擎，可以使用多台服务器进行协调工作，每台服务器可以运行单个或者多个 ElasticSearch 实例，单个 ElasticSearch 实例被称为一个节点（Node），具有相同 `cluster_name` 的一组或者多个节点构成集群（cluster）。
#### 2.2 Index
索引是具有相同结构的文档（Document）集合，Elastic 会索引所有字段，经过处理之后写入一个反向索引（Inverted Index），查找数据时，直接查找该索引，所以，Elastic 数据管理的顶层单位就叫做 Index（索引），类似于传统关系型数据库中的一个库，每个 Index 的名字必须是小写。

#### 2.3 Document
Index 里面的单条纪录称为 Document（文档），多条 Document 构成了一个 Index。每个 Document 对象都是一个 JSON 对象，存储了零个或多个字段，每个 Document 都有一个 Type 和一个 ID。类似于关系型数据库中的一条纪录。Document 示例如下：

```JSON
{
  "name": "张三",
  "age": 23,
  "address": "北京石景山"
}
```

#### 2.4 Type
每个索引里都可以有一个或多个 Type，Type 是 Index 中的一个逻辑数据分类，一个 Type 下的 Document，都有相同的field，比如博客系统，有一个Index，可以定义用户数据 Type，博客数据Type，评论数据 Type。Type 就类似于关系型数据库中的表（table）。

#### 2.5 ElasticSearch VS DataBase

Elasticsearch |	数据库
--------------|-------------------------
Document		|	行
Type			|	表
Index			|	库



































