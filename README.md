# [elasticsearch](https://www.elastic.co)

>[《Elasticsearch 权威指南》中文版](https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html)
>
>[《FOSElasticaBundle》](https://github.com/FriendsOfSymfony/FOSElasticaBundle)
>
>[《go-mysql-elasticsearch》](https://github.com/siddontang/go-mysql-elasticsearch)

## elasticsearch 基础

### 安装并运行

- 下载[elastic.co/downloads/elasticsearch](elastic.co/downloads/elasticsearch)
	
- 安装
	
		 tar -xvf elasticsearch-7.5.1.tar.gz
		 cd elasticsearch-7.5.1/bin
		 ./elasticsearch
		 
		 ps:如果你想把 Elasticsearch 作为一个守护进程在后台运行，
		 	 那么可以在后面添加参数 -d
			 	 
- 测试

	>测试 Elasticsearch 是否启动成功 浏览器访问 
	`http://localhost:9200/?pretty`
		
### 基本概念

- 索引

>存储数据到 Elasticsearch 的行为叫做 **索引**
>
>索引（名词）：
>
>一个 索引 类似于传统关系数据库中的一个 数据库 ，是一个存储关系型文档的地方。 索引 (index) 的复数词为 indices 或 indexes 
>索引（动词）：
>
>索引一个文档 就是存储一个文档到一个 索引 （名词）中以便被检索和查询。这非常类似于 SQL 语句中的 INSERT 关键词，除了文档已存在时，新文档会替换旧文档情况之外。
>
>倒排索引：
>
>关系型数据库通过增加一个 索引 比如一个 B树（B-tree）索引 到指定的列上，以便提升数据检索速度。Elasticsearch 和 Lucene 使用了一个叫做 倒排索引 的结构来达到相同的目的。
>
>默认的，一个文档中的每一个属性都是 被索引 的（有一个倒排索引）和可搜索的。一个没有倒排索引的属性是不能被搜索到的。我们将在 倒排索引 讨论倒排索引的更多细节。

- [索引文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_indexing_employee_documents.html)

		PUT /megacorp/employee/1
		{
		    "first_name" : "John",
		    "last_name" :  "Smith",
		    "age" :        25,
		    "about" :      "I love to go rock climbing",
		    "interests": [ "sports", "music" ]
		}

> 注意，路径 /megacorp/employee/1 包含了三部分的信息：
>
>	megacorp 索引名称
>	employee 类型名称
>	1 特定雇员的ID

- [检索文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_retrieving_a_document.html)

- [轻量搜索](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_search_lite.html)

		GET /megacorp/employee/_search
		GET /megacorp/employee/_search?q=last_name:Smith

- [查询表达式](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_search_with_query_dsl.html)

		{
		    "query" : {
		        "match" : { //match 查询
		            "last_name" : "Smith"
		        }
		    }
		}

- [复杂搜索](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_more_complicated_searches.html)

		GET /megacorp/employee/_search
		{
		    "query" : {
		        "bool": {
		            "must": {// 必须条件
		                "match" : {// 搜索项
		                    "last_name" : "smith" // 搜索项键值
		                }
		            },
		            "filter": {// 过滤器
		                "range" : {
		                    "age" : { "gt" : 30 } 
		                }
		            }
		        }
		    }
		}

- [全文搜索](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_full_text_search.html)

		{
		    "query" : {
		        "match" : {
		            "about" : "rock climbing"
		        }
		    }
		}

- [短语搜索](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_phrase_search.html)

		{
		    "query" : {
		        "match_phrase" : { //match_phrase搜索
		            "about" : "rock climbing"
		        }
		    }
		}

- [分析 聚合](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_analytics.html)

		{
		  "aggs": {
		    "all_interests": { // 聚合
		      "terms": { "field": "interests" }
		    }
		  }
		}
		
		{
		    "aggs" : {
		        "all_interests" : {
		            "terms" : { "field" : "interests" },
		            "aggs" : { // 分级汇总
		                "avg_age" : {
		                    "avg" : { "field" : "age" }
		                }
		            }
		        }
		    }
		}
		
- [查看ElasticSearch服务状态和结果的URL](https://www.cnblogs.com/zhengchunyuan/p/9958905.html)
- [如何通过Elasticsearch进行聚合检索 (分组统计)](https://www.lmlphp.com/user/136/article/item/355428/)
- [时间范围](http://doc.codingdict.com/elasticsearch/148/)
- [日期(数值查询)](https://www.cnblogs.com/shoufeng/p/11266136.html)