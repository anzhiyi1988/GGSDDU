https://www.elastic.co/guide/en/elasticsearch/client/java-rest/7.2/java-rest-high-search.html

# 引入maven

```java
// 新建的时候，如果没参数，则查找所有的索引。
SearchRequest searchRequest = new SearchRequest(); 

// 搜索的 所有条件 由此类设置
SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder(); 
// 设置一个match_all的查询
searchSourceBuilder.query(QueryBuilders.matchAllQuery()); 

// 把设置好的条件加入 search中
searchRequest.source(searchSourceBuilder); 
```

