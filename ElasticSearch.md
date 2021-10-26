---
title: ES学习笔记
date: 2020-12-22 09:40:33
tags:
---

## 下载安装

### 下载（docker）

```
docker pull elasticsearch
```

### 配置

```
mkdir -p /mydata/elasticsearch/config
mkdir -p /mydata/elasticsearch/data
echo "http.host: 0.0.0.0" >/mydata/elasticsearch/config/elasticsearch.yml
chmod -R 777 /mydata/elasticsearch/
```

<!--more-->

### 启动



```
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e  "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m -Xmx512m" \
-v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /mydata/elasticsearch/data:/usr/share/elasticsearch/data \
-v  /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.6.2 
```

### 设置开机自启

```
docker update elasticsearch --restart=always
```

## 概念

![A{V60~K}ZEBX}6QLXDJB9FO](https://gitee.com/vergeee/static-repo/raw/master//img/20201222203731.png)

- 索引相当于数据库

- 类型相当于数据表(慢慢会被弃用)

- 文档相当于数据库的数据行
  - 字段：文档中的kv对
  - 词：表示文本中的一个单词
  - 标记：表示在字段中出现的词

### 倒排索引

## IK分词器

github地址：https://github.com/medcl/elasticsearch-analysis-ik

分词器版本和es版本对应

解压后放入ex的plugins目录下

#### ik_smart

最少切分

```json
GET _analyze
{
  "analyzer": "ik_smart",
  "text": "好好生活"
}
```

结果

```json
{
  "tokens" : [
    {
      "token" : "好好",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "生活",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 1
    }
  ]
}
```

#### ik_max_word

穷尽可能

```json
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "好好生活"
}
```



```json
{
  "tokens" : [
    {
      "token" : "好好",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "好生",
      "start_offset" : 1,
      "end_offset" : 3,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "生活",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 2
    }
  ]
}

```

### 增加词库



## Rest风格api

- GET: 获取
- POST: 更新
- PUT:创建
- DELETE:删除

### 创建索引（PUT）

发送PUT请求到

`http://localhost:9200/<index>/<type>/<id>`

例：

```json
PUT http://192.168.56.10:9200/movies/movie/1
{
    "title": "The Godfather",
    "director": "Francis Ford Coppola",
    "year": 1972
}
```

返回结果

```json
{
    "_index": "movies",
    "_type": "movie",
    "_id": "1",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```

### 更新索引

使用完全相同的索引类型id，再次发送PUT请求,带上新的JSON对象

```json
PUT http://192.168.56.10:9200/movies/movie/1
{
    "title": "The Godfather",
    "director": "Francis Ford Coppola",
    "year": 1972,
    "genres": ["Crime", "Drama"]
}
```

返回结果

```json
{
    "_index": "movies",
    "_type": "movie",
    "_id": "1",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 4,
    "_primary_term": 1
}
```

_version加一

reslult为update

### 通过ID获取文档/索引

发送GET到`http://localhost:9200/movies/movie/1`

返回结果

```json
{
    "_index": "movies",
    "_type": "movie",
    "_id": "1",
    "_version": 6,
    "_seq_no": 5,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "title": "The Godfather",
        "director": "Francis Ford Coppola",
        "year": 1972,
        "genres": [
            "Crime",
            "Drama"
        ]
    }
}
```

### 删除文档

发送DELETE到`http://localhost:9200/movies/movie/1`

返回结果

```json
{
    "_index": "movies",
    "_type": "movie",
    "_id": "1",
    "_version": 7,
    "result": "deleted",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 6,
    "_primary_term": 1
}
```

### 核心功能：搜索

http://localhost:9200/_search 

http://localhost:9200/movies/_search - 在电影索引中搜索所有类型

http://localhost:9200/movies/movie/_search - 在电影索引中显式搜索电影类型的文档。

#### 搜索请求正文和DSL

```json
{
    "query": {
        //Query DSL here
    }
}
```

例

```json
GET /_search
{
  "query": { 1
    "bool": { 2
      "must": [
        { "match": { "title":   "Search"        }},
        { "match": { "content": "Elasticsearch" }}
      ],
      "filter": [ 3
        { "term":  { "status": "published" }},
        { "range": { "publish_date": { "gte": "2015-01-01" }}}
      ]
    }
  }
}
```

1. query包含查询上下文
2. 两个match被用来给每个匹配到的文档评分
3. filter会过滤掉不符合条件的

#### 复合查询

符合查询包括

1. bool query：默认
2. boosting query：返回文档匹配到positive，同时减少匹配到negative的文档的分数
3. constant_score query: 所有匹配的文档将被给予相同分数
4. dis_max query
5. function_score query

##### Boolean query

| **Occur** | 表头                           |
| --------- | ------------------------------ |
| must      | 文档中必须包含，匹配到会有加分 |
| filter    | 文档中必须包含，但不会加分     |
| should    | 文档中应该出现                 |
| must_not  | 文档中不能出现，分数将被忽略   |

```json
POST _search
{
  "query": {
    "bool" : {
      "must" : {
        "term" : { "user.id" : "kimchy" }
      },
      "filter": {
        "term" : { "tags" : "production" }
      },
      "must_not" : {
        "range" : {
          "age" : { "gte" : 10, "lte" : 20 }
        }
      },
      "should" : [
        { "term" : { "tags" : "env1" } },
        { "term" : { "tags" : "deployed" } }
      ],
      "minimum_should_match" : 1,
      "boost" : 1.0
    }
  }
}
```

## 整合Spring Boot