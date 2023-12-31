# start
- 7.x 跟8.x 版本差异许多所以我分成两个yml，执行如下
    - .env是给8.x版本用的
    ```bash
    docker-compose -f docker-compose_7.x.yml up -d
    docker-compose -f docker-compose_8.x.yml up -d
    ```
- 如果是8.x版本
    - es: https://localhost:9200
    - kibana: http://localhost:5601
# Ref
- [Install Elasticsearch with Docker](https://www.elastic.co/guide/en/elasticsearch/reference/7.5/docker.html)
- [This is the official Go client for Elasticsearch.](https://www.elastic.co/guide/en/elasticsearch/client/go-api/current/overview.html)
- [我把ElasticSearch上雲了 系列](https://ithelp.ithome.com.tw/users/20130639/ironman/3747)
- [ElasticSearch 全文字搜尋引擎 — 基本概念介紹](https://medium.com/happy-friday/elasticsearch-%E5%85%A8%E6%96%87%E5%AD%97%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%E4%BB%8B%E7%B4%B9-f38a0cab9717)
- [笔记六：通过 Analyzer 进行分词](https://learnku.com/articles/35136)
- [要搞懂 Elasticsearch Match Query，看这篇就够了](https://segmentfault.com/a/1190000017110948)
- [[Elasticsearch] 基本概念 & 搜尋入門](https://godleon.github.io/blog/Elasticsearch/Elasticsearch-getting-started/)
- [Getting started with the Elastic Stack and Docker-Compose](https://www.elastic.co/blog/getting-started-with-the-elastic-stack-and-docker-compose)
- [Docker-Compose 建立 Elasticsearch 與 Kibana 服務](https://kevintsengtw.blogspot.com/2018/07/docker-compose-elasticsearch-kibana.html)
- [Elasticsearch：使用 Docker compose 来一键部署 Elastic Stack 8.x](https://juejin.cn/post/7082735047824015397)
- [docker-compose快速部署elasticsearch-8.x集群+kibana](https://blog.csdn.net/boling_cavalry/article/details/125232858)
- [如何在 Kibana 8 與 Elasticsearch 8 上看到資料](https://blog.yowko.com/discover-elasticsearch8-kibana8/)

# services
- GUI: http://localhost:8080
- Kibana: http://localhost:5601
- Elasticsearch: http://localhost:9200
# Tip
- Index: 代表文件資料庫，相當於 RDBMS 中的 Database。
- Type: 代表文件類型，相當於 RDBMS 中的 Table。
- Document: 代表文件資料，ES 使用 JSON 格式來儲存文件，相當於 RDBMS 中的 Row。
- Field: 代表文件中的欄位，也就是 JSON 格式中的 Key，相當於 RDMS 中的 Column
    - keyword: 代表該 field 為 exact value，例如 id, email，可用來排序以及 wildcard 搜尋
    - text: 代表該 field 為 full-text。
    - numeric: 代表該 field 為數字，可用於 aggregate 以及 range 搜尋。
- 如果要支援中文分词，得另外安装扩展
    ```bash
    docker-compose exec es01 bash
    bin/elasticsearch-plugin install analysis-icu

    # 另外es02, es03也要
    ```

# Kibana
- 如果要作一个log viewer的话，index的mapping结构大概如下，重点是timestamp这个，这会在kibana预设作为时间读取的判断，其余的栏位其实可以参考kibana架设起来后，可以尝试汇入example data，看官方mapping怎用的
    ```json
    {
        "settings": {
            "index": {
                "number_of_shards": 3,
                "number_of_replicas": 2
            }
        },
        "mappings": {
            "properties": {
                "@timestamp": {
                    "type": "alias",
                    "path": "timestamp"
                },
                "message": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "url": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "response": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "request": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "timestamp": {
                    "type": "date"
                }
            }
        }
    }
    ```
- 上述步骤完成后，kibana还需要创建"index pattern"，有点像是一个群组，指定要读取哪些ES的index，通常"index pattern"会取名类似"log*"，然后要作log的es的index，可以用每天下去分类，index名称就会变成"log-2023-08-22"这样