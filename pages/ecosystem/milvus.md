


 Milvus
 [#](#milvus "Permalink to this headline")
===================================================



 This page covers how to use the Milvus ecosystem within LangChain.
It is broken into two parts: installation and setup, and then references to specific Milvus wrappers.
 




 Installation and Setup
 [#](#installation-and-setup "Permalink to this headline")
-----------------------------------------------------------------------------------


* Install the Python SDK with
 `pip
 

 install
 

 pymilvus`





 Wrappers
 [#](#wrappers "Permalink to this headline")
-------------------------------------------------------



### 
 VectorStore
 [#](#vectorstore "Permalink to this headline")



 There exists a wrapper around Milvus indexes, allowing you to use it as a vectorstore,
whether for semantic search or example selection.
 



 To import this vectorstore:
 





```
from langchain.vectorstores import Milvus

```




 For a more detailed walkthrough of the Miluvs wrapper, see
 [this notebook](../modules/indexes/vectorstores/examples/milvus)






