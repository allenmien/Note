# ElasticSearch
## 基础
### 搜索
文档中的每个字段都将被索引并且可以被查询 。
搜索（search） 可以做到：
- 在类似于 gender 或者 age 这样的字段 上使用结构化查询，join_date 这样的字段上使用排序，就像SQL的结构化查询一样。
- 全文检索，找出所有匹配关键字的文档并按照相关性（relevance） 排序后返回结果。
- 以上二者兼而有之。
#### 空搜索

```
GET /_search
```

```
{
   "hits" : {
      "total" :       14,
      "hits" : [
        {
          "_index":   "us",
          "_type":    "tweet",
          "_id":      "7",
          "_score":   1,
          "_source": {
             "date":    "2014-09-17",
             "name":    "John Smith",
             "tweet":   "The Query DSL is really powerful and flexible",
             "user_id": 2
          }
       },
        ... 9 RESULTS REMOVED ...
      ],
      "max_score" :   1
   },
   "took" :           4,
   "_shards" : {
      "failed" :      0,
      "successful" :  10,
      "total" :       10
   },
   "timed_out" :      false
}
```
##### hits
包含 total 字段来表示匹配到的文档总数，并且一个 hits 数组包含所查询结果的前十个文档
##### _score
衡量了文档与查询的匹配程度，返回的文档是按照 _score 降序排列的。
##### took
took 值告诉我们执行整个搜索请求耗费了多少毫秒。
##### timeout 
timed_out 值告诉我们查询是否超时。
如果低响应时间比完成结果更重要，你可以指定 timeout 为 10 或者 10ms（10毫秒），或者 1s（1秒）：

```
GET /_search?timeout=10ms
```
> 应当注意的是 timeout 不是停止执行查询，它仅仅是告知正在协调的节点返回到目前为止收集的结果并且关闭连接。在后台，其他的分片可能仍在执行查询即使是结果已经被发送了。
使用超时是因为 SLA(服务等级协议)对你是很重要的，而不是因为想去中止长时间运行的查询。

#### 多索引，多类型
/索引/类型/_search

```
/_search
在所有的索引中搜索所有的类型
/gb/_search
在 gb 索引中搜索所有的类型
/gb,us/_search
在 gb 和 us 索引中搜索所有的文档
/g*,u*/_search
在任何以 g 或者 u 开头的索引中搜索所有的类型
/gb/user/_search
在 gb 索引中搜索 user 类型
/gb,us/user,tweet/_search
在 gb 和 us 索引中搜索 user 和 tweet 类型
/_all/user,tweet/_search
在所有的索引中搜索 user 和 tweet 类型
```
#### 分页
##### from 和 size 参数
size：显示应该返回的结果数量，默认是 10

from：显示应该跳过的初始结果数量，默认是 0
##### 栗子
如果每页展示 5 条结果，可以用下面方式请求得到 1 到 3 页的结果：

```
GET /_search?size=5
GET /_search?size=5&from=5
GET /_search?size=5&from=10
```
> 请记住一个请求经常跨越多个分片，每个分片都产生自己的排序结果，这些结果需要进行集中排序以保证整体顺序是正确的

#### 轻量搜索
##### 两种形式的 搜索 API
###### “轻量的” 查询字符串
- "+" 前缀表示必须与查询条件匹配
- "-" 前缀表示一定不与查询条件匹配
-  没有 "+" 或者 "-" 的所有其他条件都是可选的
-  ":" 包含

**例子：**

```
GET /_all/tweet/_search?q=tweet:elasticsearch
```

tweet 类型中 tweet 字段包含 elasticsearch 单词的所有文档

```
GET /_all/tweet/_search?q=+name:john +tweet:mary
```

name 字段中包含 john 并且在 tweet 字段中包含 mary 的文档

```
GET /_search?q=%2Bname%3Ajohn+%2Btweet%3Amary
```
字符串参数URL编码
###### _all

```
GET /_search?q=mary
```
当索引一个文档的时候，Elasticsearch取出所有字段的值拼接成一个大的字符串，作为 _all 字段进行索引。

```
{
    "tweet":    "However did I manage before Elasticsearch?",
    "date":     "2014-09-14",
    "name":     "Mary Jones",
    "user_id":  1
}
```
这就好似增加了一个名叫 _all 的额外字段：

```
"However did I manage before Elasticsearch? 2014-09-14 Mary Jones 1"
```
###### 更复杂的查询
**栗子：**
- name 字段中包含 mary 或者 john
- date 值大于 2014-09-10
- _all_ 字段包含 aggregations 或者 geo

```
+name:(mary john) +date:>2014-09-10 +(aggregations geo)
```
### 映射和分析

```
GET /索引/_mapping/类型
```

```
GET /feed_sit/_mapping/feed_sit
```

```
{
   "gb": {
      "mappings": {
         "tweet": {
            "properties": {
               "date": {
                  "type": "date",
                  "format": "strict_date_optional_time||epoch_millis"
               },
               "name": {
                  "type": "string"
               },
               "tweet": {
                  "type": "string"
               },
               "user_id": {
                  "type": "long"
               }
            }
         }
      }
   }
}
```
#### 精确值和全文
### 请求体查询
#### 空查询

```
POST /index/type1,type2/_search
{}
```
#### from 和 size 参数


```
POST /_search
{
  "from": 30,
  "size": 10
}
```
#### 查询表达式

##### 查询语句的结构

```
POST /_search
{
    "query": {
        "match_all": {}
    }
}
```

```
GET /_search
{
    "query": {
        "match": {
            "tweet": "elasticsearch"
        }
    }
}
```
##### 合并查询语句
一条复合语句可以合并任何其它查询语句，包括复合语句。这就意味着，复合语句之间可以互相嵌套，可以表达非常复杂的逻辑。


```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "info_title": "区块链"
        }
      },
      "must_not": {
        "match": {
          "name": "mary"
        }
      },
      "should": {
        "match": {
          "tweet": "full text"
        }
      },
      "filter": {
        "range": {
          "age": {
            "gt": 30
          }
        }
      }
    }
  }
}
```
嵌套

```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "email": "business opportunity"
        }
      },
      "should": [
        {
          "match": {
            "starred": true
          }
        },
        {
          "bool": {
            "must": {
              "match": {
                "folder": "inbox"
              }
            },
            "must_not": {
              "match": {
                "spam": true
              }
            }
          }
        }
      ],
      "minimum_should_match": 1
    }
  }
}
```
#### 查询与过滤
> 从 Elasticsearch2.0开始，过滤（filters）已经从技术上被排除了，同时所有的查询（queries）拥有变成不评分查询的能力。

> 我们用 "filter" 这个词表示不评分、只过滤情况下的查询。你可以把 "filter" 、 "filtering query" 和 "non-scoring query" 这几个词视为相同的。

> 过滤（filtering）的目标是减少那些需要通过评分查询（scoring queries）进行检查的文档。 

#### 最重要的查询
##### match_all查询
match_all 查询简单的匹配所有文档。在没有指定查询方式时，它是默认的查询：

```
{ "match_all": {}}
```
##### match查询

```
{ "match": { "tweet": "About Search" }}
```
> 对于精确值的查询，你可能需要使用 filter 语句来取代query，因为 filter 将会被缓存

###### multi_match查询
在 content_keywords、title_keywords字段中，查询“区块链”关键字的信息
```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "multi_match": {
      "query": "区块链",
      "fields": ["content_keywords","title_keywords"]
    }
  }
}
```

###### range查询

```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "range": {
      "info_push_time": {
        "gte": "2018-03-05 16:51:04",
        "lt": "2018-03-08 16:51:04"
      }
    }
  }
}
```
###### term查询
term 查询被用于精确值匹配，这些精确值可能是数字、时间、布尔或者那些 not_analyzed 的字符串。

```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "term": {
      "info_push_time": {
        "value": "2018-03-05 20:36:01"
      }
    }
  }
}
```
###### terms查询
允许你指定多值进行匹配。如果这个字段包含了指定值中的任何一个值，那么这个文档满足条件。

```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "terms": {
      "FIELD": [
        "VALUE1",
        "VALUE2"
      ]
    }
  }
}
```
###### exists 查询和 missing 查询
用来查找有无该字段的文档

```
POST /feed_sit/feed_sit/_search?pretty=true
{
  "query": {
    "exists":{
      "field":"info_title"
    }
  }
}
```

#### 组合多查询
需要在多个字段上查询多种多样的文本，并且根据一系列的标准来过滤。

可以用 bool 查询来实现需求。这种查询将多查询组合在一起，成为用户自己想要的布尔查询。

相关性得分是如何组合的。每一个子查询都独自地计算文档的相关性得分。一旦他们的得分被计算出来，bool查询就将这些得分进行合并并且返回一个代表整个布尔操作的得分。

参数：
> must
文档 必须 匹配这些条件才能被包含进来。

> must_not
文档 必须不 匹配这些条件才能被包含进来。

> should
如果满足这些语句中的任意语句，将增加 _score ，否则，无任何影响。它们主要用于修正每个文档的相关性得分。

> filter
必须 匹配，但它以不评分、过滤模式来进行。这些语句对评分没有贡献，只是根据过滤标准来排除或包含文档。

**栗子：**

下面的查询用于查找：

title 字段匹配 how to make millions

并且tag 字段不被标识为 spam 的文档

那些被标识为 starred或在2014之后的文档，将比另外那些文档拥有更高的排名

如果 两者都满足，那么它排名将更高：

```
{
    "bool": {
        "must":     { "match": { "title": "how to make millions" }},
        "must_not": { "match": { "tag":   "spam" }},
        "should": [
            { "match": { "tag": "starred" }},
            { "range": { "date": { "gte": "2014-01-01" }}}
        ]
    }
}
```
> 如果没有 must 语句，那么至少需要能够匹配其中的一条 should 语句。但，如果存在至少一条must语句，则对should语句的匹配没有要求。

##### 过滤器

```
{
    "bool": {
        "must":     { "match": { "title": "how to make millions" }},
        "must_not": { "match": { "tag":   "spam" }},
        "should": [
            { "match": { "tag": "starred" }}
        ],
        "filter": {
          "range": { "date": { "gte": "2014-01-01" }} 
        }
    }
}
```
如果你需要通过多个不同的标准来过滤你的文档，bool查询本身也可以被用做不评分的查询。

```
{
    "bool": {
        "must":     { "match": { "title": "how to make millions" }},
        "must_not": { "match": { "tag":   "spam" }},
        "should": [
            { "match": { "tag": "starred" }}
        ],
        "filter": {
          "bool": { 
              "must": [
                  { "range": { "date": { "gte": "2014-01-01" }}},
                  { "range": { "price": { "lte": 29.99 }}}
              ],
              "must_not": [
                  { "term": { "category": "ebooks" }}
              ]
          }
        }
    }
}
```
##### constant_score查询
它被经常用于你只需要执行一个filter而没有其它查询（例如，评分查询）的情况下。

可以使用它来取代只有filter语句的bool查询。在性能上是完全相同的，但对于提高查询简洁性和清晰度有很大帮助。

```
{
    "constant_score":   {
        "filter": {
            "term": { "category": "ebooks" } 
        }
    }
}
```
#### 验证查询
### 排序和相关性
#### 排序
#### 按照字段的值进行排序

```
GET /_search
{
    "query" : {
        "bool" : {
            "filter" : { "term" : { "user_id" : 1 }}
        }
    },
    "sort": { "date": { "order": "desc" }}
}
```
#### 多级排序
匹配的结果首先按照日期排序，然后按照相关性排序。

结果首先按第一个条件排序，仅当结果集的第一个sort值完全相同时才会按照第二个条件进行排序，以此类推。

```
GET /_search
{
    "query" : {
        "bool" : {
            "must":   { "match": { "tweet": "manage text search" }},
            "filter" : { "term" : { "user_id" : 2 }}
        }
    },
    "sort": [
        { "date":   { "order": "desc" }},
        { "_score": { "order": "desc" }}
    ]
}
```
