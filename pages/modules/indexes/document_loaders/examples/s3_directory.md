


 s3 Directory
 [#](#s3-directory "Permalink to this headline")
===============================================================



 This covers how to load document objects from an s3 directory object.
 







```
from langchain.document_loaders import S3DirectoryLoader

```










```
#!pip install boto3

```










```
loader = S3DirectoryLoader("testing-hwc")

```










```
loader.load()

```








```
[Document(page_content='Lorem ipsum dolor sit amet.', lookup_str='', metadata={'source': '/var/folders/y6/8_bzdg295ld6s1_97_12m4lr0000gn/T/tmpaa9xl6ch/fake.docx'}, lookup_index=0)]

```







 Specifying a prefix
 [#](#specifying-a-prefix "Permalink to this headline")
-----------------------------------------------------------------------------



 You can also specify a prefix for more finegrained control over what files to load.
 







```
loader = S3DirectoryLoader("testing-hwc", prefix="fake")

```










```
loader.load()

```








```
[Document(page_content='Lorem ipsum dolor sit amet.', lookup_str='', metadata={'source': '/var/folders/y6/8_bzdg295ld6s1_97_12m4lr0000gn/T/tmpujbkzf_l/fake.docx'}, lookup_index=0)]

```








