#### ES 与传统关系型数据库的数据结构对比

| ES                                 | DB                             |
| ---------------------------------- | ------------------------------ |
| Index(ES 中的 document 集合)       | Table(关系型数据库中的表)      |
| Document(Index 中的一条数据)       | Row(表中的一条数据)            |
| Field(ES 中每个Index 中包含的字段) | Colume(表中的每一列)           |
| Mappling(ES Index结构定义)         | Schema(关系型数据库结构表定义) |
| DSL(查询语句)                      | SQL(查询语句)                  |

#### 查看 ES 集群的健康状态

```shell
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

#### Shard

shard 类型

- primary shard ： 主分片 （用于水平横向扩展 ES cluster），6.x默认为5，7.x 默认为1
- replica shard : 主分片副本（用于高可用）

#### Document

ES 中的每一条数据被定义为一个Document , document 了ES 中可以查询的最小数据单元。

Document 在 ES 中会被序列化为 JSON 格式进行存储，每一个 Document 都有唯一的 ID ，这个ID 可以自定义指定，也可以使用 ES 自动生成。

##### JSON document 示例

插入一条document

```shell
PUT twitter/_doc/1
{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
}
```

查询上述的 document 信息

```shell
GET twitter/_doc/1 // 查询 document 的具体信息
HEAD twitter/_doc/1 // 查看 document 是否存在
```

```json
{
  "_index": "twitter",
  "_type": "_doc",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": {
    "user": "kimchy",
    "post_date": "2009-11-15T14:12:12",
    "message": "trying out Elasticsearch"
  }
}
```

document 的 metadata

- _index : document 所属的索引名
- _type : document 所属的类型名，6.0之后只允许有 _doc 类型
- _id : document 的唯一 ID
- _source :  document 的原始 JSON 数据
- _version : document 的版本信息

document RESTful API

| name   | method | RESTful API                                    | description                                                  |
| ------ | ------ | ---------------------------------------------- | ------------------------------------------------------------ |
| index  | PUT    | <index_name>/<index_type>/<document_id> {JSON} | 插入操作：如果ID不存在，创建新的 document ; 如果ID 已存在，则先删除原 document ,再创建新的文档，同时version 增加1。不指定 ID ,则自动生成 ID |
| create | POST   | <index_name>/<index_type>/<document_id> {JSON} | 插入操作：如果ID存在，创建失败。不指定 ID ,则自动生成 ID     |
| read   | GET    | <index_name>/<index_type>/<document_id>        | 查询操作：根据document ID 进行查询                           |
| update | POST   | <index_name>/_update/<document_id>{JSON}       | 更新操作：document 必须已经存在，并且只对相应字段做增量修改  |
| delete | DELETE | <index_name>/_delete/<document_id>             | 删除操作：根据document ID 进行删除                           |

#### Index 

定义：index 是 一类 document 的集合

- index 体现了逻辑空间的概念，每个 index 都有自身的 mapping 定义（这相当于关系型数据库中的 表结构 schema 定义），用于定义包含在该 index 中的 document 的 Field 名 与 Field Type.
- Shard 体现了物理空间的概念：index 中的数据分散在 Shard 上

index 中的 mapping 和 settings 

- mappling ：定义document 字段名，字段的类型
- settings ：定义不同的数据在物理磁盘上的分布

##### JSON index 示例

```shell
GET twitter
```

```json
{
  "twitter": {
    "aliases": {},
    "mappings": {
      "_doc": {
        "properties": {
          "message": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "post_date": {
            "type": "date"
          },
          "user": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
      }
    },
    "settings": {
      "index": {
        "creation_date": "1563763215144",
        "number_of_shards": "5", // 主分片数
        "number_of_replicas": "1", // 每个主分片的副本数
        "uuid": "UNskVmJBRKqz16-lMibCVA",
        "version": {
          "created": "6020499"
        },
        "provided_name": "twitter"
      }
    }
  }
}
```

