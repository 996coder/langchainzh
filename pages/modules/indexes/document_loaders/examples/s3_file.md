


 s3 File
 [#](#s3-file "Permalink to this headline")
=====================================================



 This covers how to load document objects from an s3 file object.
 







```
from langchain.document_loaders import S3FileLoader

```










```
#!pip install boto3

```










```
loader = S3FileLoader("testing-hwc", "fake.docx")

```










```
loader.load()

```








```
[Document(page_content='Lorem ipsum dolor sit amet.', lookup_str='', metadata={'source': '/var/folders/y6/8_bzdg295ld6s1_97_12m4lr0000gn/T/tmpxvave6wl/fake.docx'}, lookup_index=0)]

```







