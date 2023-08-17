# Ref
- [Install Elasticsearch with Docker](https://www.elastic.co/guide/en/elasticsearch/reference/7.5/docker.html)
- [This is the official Go client for Elasticsearch.](https://www.elastic.co/guide/en/elasticsearch/client/go-api/current/overview.html)
- [我把ElasticSearch上雲了 系列](https://ithelp.ithome.com.tw/users/20130639/ironman/3747)
- [ElasticSearch 全文字搜尋引擎 — 基本概念介紹](https://medium.com/happy-friday/elasticsearch-%E5%85%A8%E6%96%87%E5%AD%97%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%E4%BB%8B%E7%B4%B9-f38a0cab9717)
- [笔记六：通过 Analyzer 进行分词](https://learnku.com/articles/35136)

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