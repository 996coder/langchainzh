


 Directory Loader
 [#](#directory-loader "Permalink to this headline")
=======================================================================



 This covers how to use the DirectoryLoader to load all documents in a directory. Under the hood, by default this uses the
 [UnstructuredLoader](unstructured_file)








```
from langchain.document_loaders import DirectoryLoader

```






 We can use the
 `glob`
 parameter to control which files to load. Note that here it doesn’t load the
 `.rst`
 file or the
 `.ipynb`
 files.
 







```
loader = DirectoryLoader('../', glob="\*\*/\*.md")

```










```
docs = loader.load()

```










```
len(docs)

```








```
1

```







 Show a progress bar
 [#](#show-a-progress-bar "Permalink to this headline")
-----------------------------------------------------------------------------



 By default a progress bar will not be shown. To show a progress bar, install the
 `tqdm`
 library (e.g.
 `pip
 

 install
 

 tqdm`
 ), and set the
 `show_progress`
 parameter to
 `True`
 .
 







```
%pip install tqdm
loader = DirectoryLoader('../', glob="\*\*/\*.md", show_progress=True)
docs = loader.load()

```








```
Requirement already satisfied: tqdm in /Users/jon/.pyenv/versions/3.9.16/envs/microbiome-app/lib/python3.9/site-packages (4.65.0)

```






```
0it [00:00, ?it/s]

```








 Change loader class
 [#](#change-loader-class "Permalink to this headline")
-----------------------------------------------------------------------------



 By default this uses the UnstructuredLoader class. However, you can change up the type of loader pretty easily.
 







```
from langchain.document_loaders import TextLoader

```










```
loader = DirectoryLoader('../', glob="\*\*/\*.md", loader_cls=TextLoader)

```










```
docs = loader.load()

```










```
len(docs)

```








```
1

```






 If you need to load Python source code files, use the
 `PythonLoader`
 .
 







```
from langchain.document_loaders import PythonLoader

```










```
loader = DirectoryLoader('../../../../../', glob="\*\*/\*.py", loader_cls=PythonLoader)

```










```
docs = loader.load()

```










```
len(docs)

```








```
691

```








