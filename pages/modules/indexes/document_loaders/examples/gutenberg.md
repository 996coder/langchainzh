


 Gutenberg
 [#](#gutenberg "Permalink to this headline")
=========================================================



 This covers how to load links to Gutenberg e-books into a document format that we can use downstream.
 







```
from langchain.document_loaders import GutenbergLoader

```










```
loader = GutenbergLoader('https://www.gutenberg.org/cache/epub/69972/pg69972.txt')

```










```
data = loader.load()

```










```
data

```







