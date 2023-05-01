


 Getting Started
 [#](#getting-started "Permalink to this headline")
=====================================================================



 The default recommended text splitter is the RecursiveCharacterTextSplitter. This text splitter takes a list of characters. It tries to create chunks based on splitting on the first character, but if any chunks are too large it then moves onto the next character, and so forth. By default the characters it tries to split on are
 `["\n\n",
 

 "\n",
 

 "
 

 ",
 

 ""]`




 In addition to controlling which characters you can split on, you can also control a few other things:
 


* `length_function`
 : how the length of chunks is calculated. Defaults to just counting number of characters, but it’s pretty common to pass a token counter here.
* `chunk_size`
 : the maximum size of your chunks (as measured by the length function).
* `chunk_overlap`
 : the maximum overlap between chunks. It can be nice to have some overlap to maintain some continuity between chunks (eg do a sliding window).







```
# This is a long document we can split up.
with open('../../state_of_the_union.txt') as f:
    state_of_the_union = f.read()

```










```
from langchain.text_splitter import RecursiveCharacterTextSplitter

```










```
text_splitter = RecursiveCharacterTextSplitter(
    # Set a really small chunk size, just to show.
    chunk_size = 100,
    chunk_overlap  = 20,
    length_function = len,
)

```










```
texts = text_splitter.create_documents([state_of_the_union])
print(texts[0])
print(texts[1])

```








```
page_content='Madam Speaker, Madam Vice President, our First Lady and Second Gentleman. Members of Congress and' lookup_str='' metadata={} lookup_index=0
page_content='of Congress and the Cabinet. Justices of the Supreme Court. My fellow Americans.' lookup_str='' metadata={} lookup_index=0

```







