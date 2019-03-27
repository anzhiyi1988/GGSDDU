 

GET /_cat/health?v

 

GET _cat/nodes?v

 

GET _cat/indices?v

 

GET _alias

 

GET _template

 

 

GET _stats?level=cluster

 

DELETE _template/template_anzhy

 

DELETE anzhy_index_1

DELETE anzhy_index_2

DELETE anzhy_index_3

DELETE anzhy_index_4

DELETE anzhy_rollover-000001

DELETE anzhy_rollover-000002

 

POST _flush

 

GET _cat/recovery?v

 

GET _cat/recovery?v&h=i,s,t,ty,st,f,fp,b,bp

 

GET _cat/recovery?v&h=i,s,t,ty,st,shost,thost,f,fp,b,bp

 

GET _stats

GET anzhy_index_2/_stats/_all?fields=C_AJBH

 

GET anzhy_index_2/_stats/indexing?types=t_dsr_all

 

GET anzhy_index_2/_stats/fielddata?fielddata_fields=C_AJBH,C_DSRBH

 

GET anzhy_index_2/_stats/fielddata?fields=C_AJBH,C_DSRBH

 

GET anzhy_index_2/_recovery?human

 

GET anzhy_index_2/_segments

 

GET anzhy_index_2/_shard_stores

GET anzhy_rollover/_shard_stores

 

 

GET /_shard_stores

 

PUT anzhy_rollover

 

PUT anzhy_rollover/mytype/4

{

​    "name":"anzhy4"

}

 

 

 

POST _cache/clear

POST _refresh

POST _flush

 

 

GET anzhy_index_2/_search

{

  "_source": {

​    "includes": ["C_HCZJLX","C_DSRLX"]

  },

  "query": {

​    "term": {

​       "C_CSRQ": "19521022"

​    }

  }

}

 

 

 

GET anzhy_index_2/_search

{

  "query": {

​    "term": {

​       "C_CSRQ": "19521022"

​    }

  }

}

 

 

GET anzhy_index_2/_search?stored_fields=ALL_ANSJ_INFO,C_HCZJLX

{

  "query": {

​    "term": {

​       "C_CSRQ": "19521022"

​    }

  }

}

 

GET anzhy_index_3/_search?stored_fields=ALL_ANSJ_INFO,C_HCZJLX

{

  "query": {

​    "term": {

​       "C_CSRQ": "19521022"

​    }

  }

}

 

 

 

GET anzhy_index_2/t_dsr_all/E10C11F1309A7EE77B875B70F2A51276/_termvectors

 

GET anzhy_index_3/t_dsr_all/E10C11F1309A7EE77B875B70F2A51276/_termvectors

 

GET anzhy_index_3/t_dsr_all/E10C11F1309A7EE77B875B70F2A51276/_termvectors?fields=C_HCZJLX,C_DSRLX

 

 

 

GET anzhy_index_2/t_dsr_all/E10C11F1309A7EE77B875B70F2A51276/_termvectors?pretty=true

{

  "fields" : ["ALL_ANSJ_INFO","C_HCZJLX"],

  "offsets" : true,

  "payloads" : true,

  "positions" : true,

  "term_statistics" : true,

  "field_statistics" : true

}

 

GET anzhy_index_3/t_dsr_all/E10C11F1309A7EE77B875B70F2A51276/_termvectors?pretty=true

{

  "fields" : ["ALL_ANSJ_INFO","C_HCZJLX"],

  "offsets" : true,

  "payloads" : true,

  "positions" : true,

  "term_statistics" : true,

  "field_statistics" : true

}

 

GET anzhy_index_2/_search

{

  "query": {

​    "term": {

​      "C_SFZJHM":"150424198805033013"

​    }

  }

}

 

 

GET _search

{

  "query": {

​    "match": {

​      "C_DSRMC.search_indx":"北京华宇信息"

​    }

  }

}

 

 

GET _search

{

  "query": {

​    "match": {

​      "C_SFZJHM.search_indx":"150424198805033013"

​    }

  }

}

 

GET _search

{

  "query": {

​    "match": {

​      "C_AJMC.search_indx":"无因管理、不当得利"

​    }

  }

}

 

 

GET _search

{

  "query": {

​    "bool": {

​      "must": {

​        "match": {

​          "C_AJMC.search_indx":"甘丽娜张丽交通事故"

​        }

​      },

​      "filter": {

​        "term": {"C_DSRMC": "张丽"}

​      }

​    }

  }

}

 

 

GET _search

{

  "query": {

​    "match": {

​      "ALL_ANSJ_INFO":  "甘丽娜张丽交通事故"

​    }

  }

}

 

GET _search

{

  "query": {

​    "match": {

​      "ALL_ANSJ_INFO":  "甘丽娜 张丽交通事故"

​    }

  }

}

 

 

GET _search

{

  "query": {

​    "match": {

​      "ALL_ANSJ_INFO":  "虎什哈镇虎什哈村东街97号"

​    }

  }

}

 

 

 

 

GET anzhy_index_2/_analyze

{

  "analyzer": "my_analyzer",

  "text": "1329321945 2815"

}

 

 

 

GET anzhy_index_2/_analyze

{

  "analyzer": "index_ansj",

  "text": "路北区 高新火炬 国泰公寓，D座，17号"

}

 

 

 

 

GET anzhy_index_2

GET anzhy_index_3

GET anzhy_index_4

 

PUT anzhy_index_4/_settings

{

 

​        "index": {

​            "number_of_replicas": 1,

​           

​            "refresh_interval": "30s"

​        }

 

  

}

 

 

 

 

 

 

 