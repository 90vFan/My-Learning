ElasticSearch
---------------------

数据库

数据分析平台

#### [Document APIs](<https://www.elastic.co/guide/en/elasticsearch/reference/7.1/docs.html>)

| RDBMS                   | Elastic Search        |
| ----------------------- | --------------------- |
| Database                | index                 |
| Table                   | ~~Type~~              |
| Row                     | Document              |
| Column                  | Field                 |
| Schema                  | Mapping               |
| Index                   | Everything is indexed |
| SQL                     | DSL                 |
| SELECT * FROM table ... | GET *Index*/_doc/*doc_name* {filter}|
| UPDATE table SELECT ... | PUT *Index*/_doc/*doc_name* {data}   |

RMDS（关系型数据库）: 事务性 / Join

ElasticSearch: 相关性（Score）/　高性能全文检索



Index 索引，一类文档的集合

每个索引都有自己的 Mapping 定义，定义文档字段类型（string, number, list, object）

7.0 以前，一个 Index 可以配置多个 Types

6.0 开始，Type 已经被废除

7.0 开始，一个索引只能创建一个 Type - "_doc"

[Removal of types](<https://www.elastic.co/guide/en/elasticsearch/reference/7.0/removal-of-types.html>)

Mapping Type 由 Lucence field 决定，不同 Type 中的相同文档字段会共享同一个 Mapping.

加入我需要删除 A Type 中的 user_name 字段而保留 B Type 中的 user_name 字段，这就会引起歧义。同时也会影响 Lucence 压缩文档的性能

替代解决办法

直接添加**Type**字段

```json
PUT twitter
{
  "mappings": {
    "_doc": {
      "properties": {
        "type": { "type": "keyword" }, 
        "name": { "type": "text" },
        "user_name": { "type": "keyword" },
        "email": { "type": "keyword" },
        "content": { "type": "text" },
        "tweeted_at": { "type": "date" }
      }
    }
  }
}

PUT twitter/_doc/user-kimchy
{
  "type": "user", 
  "name": "Shay Banon",
  "user_name": "kimchy",
  "email": "shay@kimchy.com"
}

PUT twitter/_doc/tweet-1
{
  "type": "tweet", 
  "user_name": "kimchy",
  "tweeted_at": "2017-10-24T09:00:00Z",
  "content": "Types are going away"
}

GET twitter/_search
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "user_name": "kimchy"
        }
      },
      "filter": {
        "match": {
          "type": "tweet" 
        }
      }
    }
  }
}
```

### Bulk API

支持在一次 API 调用中，对不同的索引进行操作



## 集群

**节点**: 一个 ElasticSearch 实例

Master-eligible nodes

Master Node

集群状态：Cluster Status

Data Node 数据节点

Coordinating Node，处理客户端请求，集群每个节点都起到 Coordinating Node 的作用，应对高并发

**分片**：一个 Lucence 实例，解决数据水平扩展问题，解决数据高可用问题（读取的吞吐，IO）



### 倒排索引

正排索引：目录，文档ID到文档内容

倒排索引：索引页，单词到文档ID的索引

<https://zh.wikipedia.org/wiki/%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95>

