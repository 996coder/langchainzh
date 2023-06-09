


 Email
 [#](#email "Permalink to this headline")
=================================================



 This notebook shows how to load email (
 `.eml`
 ) and Microsoft Outlook (
 `.msg`
 ) files.
 




 Using Unstructured
 [#](#using-unstructured "Permalink to this headline")
---------------------------------------------------------------------------







```
from langchain.document_loaders import UnstructuredEmailLoader

```










```
loader = UnstructuredEmailLoader('example_data/fake-email.eml')

```










```
data = loader.load()

```










```
data

```








```
[Document(page_content='This is a test email to use for unit tests.\n\nImportant points:\n\nRoses are red\n\nViolets are blue', lookup_str='', metadata={'source': 'example_data/fake-email.eml'}, lookup_index=0)]

```






### 
 Retain Elements
 [#](#retain-elements "Permalink to this headline")



 Under the hood, Unstructured creates different “elements” for different chunks of text. By default we combine those together, but you can easily keep that separation by specifying
 `mode="elements"`
 .
 







```
loader = UnstructuredEmailLoader('example_data/fake-email.eml', mode="elements")

```










```
data = loader.load()

```










```
data[0]

```








```
Document(page_content='This is a test email to use for unit tests.', lookup_str='', metadata={'source': 'example_data/fake-email.eml'}, lookup_index=0)

```









 Using OutlookMessageLoader
 [#](#using-outlookmessageloader "Permalink to this headline")
-------------------------------------------------------------------------------------------







```
from langchain.document_loaders import OutlookMessageLoader

```










```
loader = OutlookMessageLoader('example_data/fake-email.msg')

```










```
data = loader.load()

```










```
data[0]

```








```
Document(page_content='This is a test email to experiment with the MS Outlook MSG Extractor\r\n\r\n\r\n-- \r\n\r\n\r\nKind regards\r\n\r\n\r\n\r\n\r\nBrian Zhou\r\n\r\n', metadata={'subject': 'Test for TIF files', 'sender': 'Brian Zhou <brizhou@gmail.com>', 'date': 'Mon, 18 Nov 2013 16:26:24 +0800'})

```








