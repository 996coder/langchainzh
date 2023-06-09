


 Word Documents
 [#](#word-documents "Permalink to this headline")
===================================================================



 This covers how to load Word documents into a document format that we can use downstream.
 




 Using Docx2txt
 [#](#using-docx2txt "Permalink to this headline")
-------------------------------------------------------------------



 Load .docx using
 `Docx2txt`
 into a document.
 







```
from langchain.document_loaders import Docx2txtLoader

```










```
loader = Docx2txtLoader("example_data/fake.docx")

```










```
data = loader.load()

```










```
data

```








```
[Document(page_content='Lorem ipsum dolor sit amet.', metadata={'source': 'example_data/fake.docx'})]

```








 Using Unstructured
 [#](#using-unstructured "Permalink to this headline")
---------------------------------------------------------------------------







```
from langchain.document_loaders import UnstructuredWordDocumentLoader

```










```
loader = UnstructuredWordDocumentLoader("example_data/fake.docx")

```










```
data = loader.load()

```










```
data

```








```
[Document(page_content='Lorem ipsum dolor sit amet.', lookup_str='', metadata={'source': 'fake.docx'}, lookup_index=0)]

```








 Retain Elements
 [#](#retain-elements "Permalink to this headline")
---------------------------------------------------------------------



 Under the hood, Unstructured creates different “elements” for different chunks of text. By default we combine those together, but you can easily keep that separation by specifying
 `mode="elements"`
 .
 







```
loader = UnstructuredWordDocumentLoader("example_data/fake.docx", mode="elements")

```










```
data = loader.load()

```










```
data[0]

```








```
Document(page_content='Lorem ipsum dolor sit amet.', lookup_str='', metadata={'source': 'fake.docx', 'filename': 'fake.docx', 'category': 'Title'}, lookup_index=0)

```








