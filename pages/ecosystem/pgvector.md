


 PGVector
 [#](#pgvector "Permalink to this headline")
=======================================================



 This page covers how to use the Postgres
 [PGVector](https://github.com/pgvector/pgvector) 
 ecosystem within LangChain
It is broken into two parts: installation and setup, and then references to specific PGVector wrappers.
 




 Installation
 [#](#installation "Permalink to this headline")
---------------------------------------------------------------


* Install the Python package with
 `pip
 

 install
 

 pgvector`





 Setup
 [#](#setup "Permalink to this headline")
-------------------------------------------------


1. The first step is to create a database with the
 `pgvector`
 extension installed.
 



 Follow the steps at
 [PGVector Installation Steps](https://github.com/pgvector/pgvector#installation) 
 to install the database and the extension. The docker image is the easiest way to get started.





 Wrappers
 [#](#wrappers "Permalink to this headline")
-------------------------------------------------------



### 
 VectorStore
 [#](#vectorstore "Permalink to this headline")



 There exists a wrapper around Postgres vector databases, allowing you to use it as a vectorstore,
whether for semantic search or example selection.
 



 To import this vectorstore:
 





```
from langchain.vectorstores.pgvector import PGVector

```





### 
 Usage
 [#](#usage "Permalink to this headline")



 For a more detailed walkthrough of the PGVector Wrapper, see
 [this notebook](../modules/indexes/vectorstores/examples/pgvector)






