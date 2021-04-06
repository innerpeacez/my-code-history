![elastic](../images/elastic-logo.svg)

#### 简介

Elasticsearch（ES）是一个基于Apache Lucene构建的开源、分布式、RESTful接口的全文搜索引擎。 Elasticsearch还是一个分布式文档数据库，其中每个字段均可被索引，而且每个字段的数据均可被搜索，ES能够横向扩展至数以百计的服务器存储以及处理PB级的数据。

#### 应用场景

- 数据分析引擎
- 搜索引擎
- 数据存储
- 日志存储

#### ES 中的重要概念

1. cluster 集群：一个集群就是由一个或多个节点组织在一起，它们共同持有你整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，这个名字默认就是“elasticsearch”。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群。

2. node 节点：一个节点是你集群中的一个服务器，作为集群的一部分，它存储你的数据，参与集群的索引和搜索功能

3. index 索引： 一个索引就是一个拥有几分相似特征的文档的集合

4. type 类型：类型是模拟mysql中的table概念，一个索引库下可以有不同类型的索引，比如商品索引，订单索引，其数据格式不同。不过这会导致索引库混乱，因此未来版本中会移除这个概念

5. document 文档： ES 中的每一条数据被定义为一个Document , document 是 ES 中可以查询的最小数据单元。Document 在 ES 中会被序列化为 JSON 格式进行存储，每一个 Document 都有唯一的 ID ，这个ID 可以自定义指定，也可以使用 ES 自动生成。

6. mapping 映射：映射(Mapping)相当于数据表的表结构。ElasticSearch中的映射（Mapping）用来定义一个文档，可以定义所包含的**字段**以及**字段的类型**、**分词器**及**属性**等等。类似于传统关系型数据中table的schema，用于定义一个索引（index）的某个类型（type）的数据的结构

   **mapping 支持的核心类型：**

   - string：字符串 string 类型包含 *text* 和 keyword
     - text：该类型被用来索引长文本，在创建索引前会将这些文本进行分词，转化为词的组合，建立索引；允许es来检索这些词，text类型不能用来排序和聚合
     - keyword：该类型不需要进行分词，可以被用来检索过滤、排序和聚合，keyword类型自读那只能用本身来进行检索（不可用text分词后的模糊检索）
   - long、integer、short、byte、double、float  ： 基础数值类型
   - date：日期类型
   - boolean： 布尔型
   - binary： 二进制类型

7. shards 分片：每个index会被拆分为多个shard，每个shard就会存放这个index的一部分数据，这此shard会散落在多台服务器上。有了shard就可以进行横向扩展，存储更多数据，让搜索和分析等操作分布到多台服务器上去执行，提升吞吐量和性能。shard又分为replica shard和primary shard，每个shard都是一个lucene index.

8. replicas 副本：每个服务器随时可能故障或宕机，此时shard就可以会丢失，因此可以为每一个shard创建多个replica副本。replica可以在shard故障时提供备用服务。保证数据不丢失或者丢失很少，多个replica还可以提升搜索操作的吞吐量和性能。

#### ES 与传统关系型数据库的数据结构对比

| ES                                 | DB                             |
| ---------------------------------- | ------------------------------ |
| Index(ES 中的 document 集合)       | Table(关系型数据库中的表)      |
| Document(Index 中的一条数据)       | Row(表中的一条数据)            |
| Field(ES 中每个Index 中包含的字段) | Colume(表中的每一列)           |
| Mappling(ES Index结构定义)         | Schema(关系型数据库结构表定义) |
| DSL(查询语句)                      | SQL(查询语句)                  |

#### ES 中的倒排索引





#### 配置

部署之前，先要了解 ES 相关的配置文件，这样再部署的时候才能控制的得心应手.ES 支持通过 [RESTful 接口](https://www.elastic.co/guide/en/elasticsearch/reference/7.11/cluster-update-settings.html)动态配置集群信息，同时也支持 elasticsearch.yml 文件静态配置

##### ES 有三种配置文件

- elasticsearch.yml 用于配置 Elasticsearch
- jvm.options 用于配置 Elasticsearch JVM 设置的 jvm.options
- log4j2.properties 用于配置 Elasticsearch 日志记录

##### ES 重要配置

| 配置                 | 描述                                                         | 配置位置          |
| -------------------- | ------------------------------------------------------------ | ----------------- |
| path.data            | 数据存储路径，支持配置多个路径                               | elasticsearch.yml |
| path.logs            | 日志存放路径                                                 | elasticsearch.yml |
| cluster.name         | 集群名称配置                                                 | elasticsearch.yml |
| node.name            | 节点名称配置                                                 | elasticsearch.yml |
| network.host         | 网络配置，默认127.0.0.1 或者 [::1]                           | elasticsearch.yml |
| discovery.seed_hosts | 节点间相互发现，从而选出主节点                               | elasticsearch.yml |
| plugin.mandatory     | 强制安装对应插件，如果节点未安装好，节点将不可用             | elasticsearch.yml |
| -Xmx                 | Xmx 最大堆，如：-Xmx4g                                       | jvm.options       |
| -Xms                 | Xms 最小堆，如：-Xms4g                                       | jvm.options       |
| -XX:HeapDumpPath     | 默认内存不足异常中的堆转储到默认数据目录 /var/lib/elasticsearch | jvm.options       |
| -XX:ErrorFile        | 默认情况下JVM致命错误日志设置保存在目录 /var/log/elasticsearch | jvm.options       |

#### 部署

Es 提供了多种部署方式，这里主要介绍本地部署，docker 部署，kubernetes部署

##### 本地部署（部署到 Linux 或 MacOS 系统）

###### 执行一下命令下载并部署 elasticsearch

```shell
version=7.11.1
paltform=linux # linux or darwin
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${version}-${paltform}-x86_64.tar.gz
tar -xvzf elasticsearch-${version}-${paltform}-x86_64.tar.gz
cd elasticsearch-${version}/ 
./bin/elasticsearch
```

###### 检查 elasticsearch 是否启动成功

```http
curl -X GET localhost:9200
{
  "name" : "innerpeacezdeMacBook-Pro.local",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "YdvQaLX5TCuQuU6-m3Q0Nw",
  "version" : {
    "number" : "7.11.1",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "ff17057114c2199c9c1bbecc727003a907c0db7a",
    "build_date" : "2021-02-15T13:44:09.394032Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

###### es 下载文件目录结构及描述

| 目录    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| home    | ES home 文件夹或者 $ES_HOME                                  |
| bin     | 二进制脚本，包括用于启动节点的elasticsearch和用于安装插件的elasticsearch-plugin |
| conf    | 配置文件，包括elasticsearch.yml                              |
| data    | 节点上分配的每个索引/分片的数据文件的位置。可以支持多个位置。 |
| logs    | 日志文件的位置。                                             |
| plugins | 插件文件位置。每个插件将包含在一个子目录中。                 |
| repo    | 共享的文件系统存储库位置。可以配置多个位置。可以将文件系统存储库放置在此处指定的任何目录的任何子目录中。 |

##### Docker 部署（单机）

###### 拉取镜像

```shell
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.11.1
```

###### 部署镜像

```sh
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.11.1
```

##### Docker 部署（集群）

###### 创建 docker-compose.yml文件

```yml
version: '2.2'
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.11.1
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic
  es02:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.11.1
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data02:/usr/share/elasticsearch/data
    networks:
      - elastic
  es03:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.11.1
    container_name: es03
    environment:
      - node.name=es03
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es02
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data03:/usr/share/elasticsearch/data
    networks:
      - elastic

volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

networks:
  elastic:
    driver: bridge
```

###### 部署集群

```sh
docker-compose up
```

###### 检查是否正常启动

```http
curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"
```

#### kuberneter部署（Helm 3）[ES Helm Chart官方库](https://github.com/elastic/helm-charts/tree/master/elasticsearch)

##### 安装 helm 3 cli (略)

安装好后需要配置 helm cli 可以访问 kubernetes 集群

##### 添加 elasticsearch helm 仓库

```shell
helm repo add elastic https://helm.elastic.co
```

##### 部署

简单的执行 一下命令后，等待 es 在 kubernetes 中部署完成即可使用

```shell
helm install elasticsearch elastic/elasticsearch
```

#### kuberneter部署（Operator）

##### 下载 ES operator 资源文件

```shell
kubectl apply -f https://download.elastic.co/downloads/eck/1.4.0/all-in-one.yaml
```

##### 快速部署一个单机版的ES

```
cat <<EOF | kubectl apply -f -
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: quickstart
spec:
  version: 7.11.1
  nodeSets:
  - name: default
    count: 1
    config:
      node.store.allow_mmap: false
EOF
```

##### 检查是否部署成功

```sh
kubectl get elasticsearch
```

```sh
kubectl get pods --selector='elasticsearch.k8s.elastic.co/cluster-name=quickstart'
```

##### 查看 ES 日志

```sh
kubectl logs -f quickstart-es-default-0
```

##### 检查ES 是否启动成功

```sh
PASSWORD=$(kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
```

```sh
kubectl port-forward service/quickstart-es-http 9200
```

```sh
curl -u "elastic:$PASSWORD" -k "https://localhost:9200"
```

#### ES 插件安装

##### 官方核心插件安装

```shell
# 安装单个插件
bin/elasticsearch-plugin install [plugin_id]

# 同时安装多个
bin/elasticsearch-plugin install [plugin_id] [plugin_id] ... [plugin_id]
```

##### 自定义URL下载插件

```shell
bin/elasticsearch-plugin install [url]
```

##### 查看当前安装的插件

```shell
bin/elasticsearch-plugin list
```

ES 同时也支持了通过 RESTful API查看已安装的插件

```http
GET /_nodes/plugins 
```

##### 卸载插件

```shell
bin/elasticsearch-plugin remove [plugin_id]
```

##### 插件更新

ES 插件是与 ES 版本绑定的，索引在升级 ES 需要重新安装对应版本的插件

```shell
bin/elasticsearch-plugin remove [plugin_id]
bin/elasticsearch-plugin install [plugin_id]
```

#### ES RESTful API

| name   | method | RESTful API                                    | description                                                  |
| ------ | ------ | ---------------------------------------------- | ------------------------------------------------------------ |
| index  | PUT    | <index_name>/<index_type>/<document_id> {JSON} | 插入操作：如果ID不存在，创建新的 document ; 如果ID 已存在，则先删除原 document ,再创建新的文档，同时version 增加1。不指定 ID ,则自动生成 ID |
| create | POST   | <index_name>/<index_type>/<document_id> {JSON} | 插入操作：如果ID存在，创建失败。不指定 ID ,则自动生成 ID     |
| read   | GET    | <index_name>/<index_type>/<document_id>        | 查询操作：根据document ID 进行查询                           |
| update | POST   | <index_name>/_update/<document_id>{JSON}       | 更新操作：document 必须已经存在，并且只对相应字段做增量修改  |
| delete | DELETE | <index_name>/_delete/<document_id>             | 删除操作：根据document ID 进行删除                           |

##### 文档元数据详情

- _index : document 所属的索引名
- _type : document 所属的类型名，6.0之后只允许有 _doc 类型
- _id : document 的唯一 ID
- _source :  document 的原始 JSON 数据
- _version : document 的版本信息

#### ES 集群管理

##### 查看 ES 集群的健康状态

```http
GET _cluster/health
```

```json
{
  "cluster_name": "efk-cluster",
  "status": "green", // 状态
  "timed_out": false,
  "number_of_nodes": 3, // 节点数
  "number_of_data_nodes": 3, // 数据节点数
  "active_primary_shards": 126, // 主分片数
  "active_shards": 252,
  "relocating_shards": 0,
  "initializing_shards": 0,
  "unassigned_shards": 0,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks": 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 100
}
```

集群的三种状态

- green : 主分片和副本分片都正常分配
- yellow : 主分片正常，存在副本分片不正常
- red : 存在主分片未分配

##### 查看集群节点信息

```http
GET _cat/nodes?pretty
```

##### 查看集群中的分片数

```http
GET _cat/shards?pretty
```

##### 查看集群中的索引

```http
GET _cat/indices?pretty
```

#### ES 内存优化

