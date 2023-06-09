


 ElasticSearch BM25
 [#](#elasticsearch-bm25 "Permalink to this headline")
===========================================================================



 This notebook goes over how to use a retriever that under the hood uses ElasticSearcha and BM25.
 



 For more information on the details of BM25 see
 [this blog post](https://www.elastic.co/blog/practical-bm25-part-2-the-bm25-algorithm-and-its-variables) 
 .
 







```
from langchain.retrievers import ElasticSearchBM25Retriever

```







 Create New Retriever
 [#](#create-new-retriever "Permalink to this headline")
-------------------------------------------------------------------------------







```
elasticsearch_url="http://localhost:9200"
retriever = ElasticSearchBM25Retriever.create(elasticsearch_url, "langchain-index-4")

```










```
# Alternatively, you can load an existing index
# import elasticsearch
# elasticsearch_url="http://localhost:9200"
# retriever = ElasticSearchBM25Retriever(elasticsearch.Elasticsearch(elasticsearch_url), "langchain-index")

```








 Add texts (if necessary)
 [#](#add-texts-if-necessary "Permalink to this headline")
-------------------------------------------------------------------------------------



 We can optionally add texts to the retriever (if they aren’t already in there)
 







```
retriever.add_texts(["foo", "bar", "world", "hello", "foo bar"])

```








```
['cbd4cb47-8d9f-4f34-b80e-ea871bc49856',
 'f3bd2e24-76d1-4f9b-826b-ec4c0e8c7365',
 '8631bfc8-7c12-48ee-ab56-8ad5f373676e',
 '8be8374c-3253-4d87-928d-d73550a2ecf0',
 'd79f457b-2842-4eab-ae10-77aa420b53d7']

```








 Use Retriever
 [#](#use-retriever "Permalink to this headline")
-----------------------------------------------------------------



 We can now use the retriever!
 







```
result = retriever.get_relevant_documents("foo")

```










```
result

```








```
[Document(page_content='foo', metadata={}),
 Document(page_content='foo bar', metadata={})]

```








