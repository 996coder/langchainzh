


 Facebook Chat
 [#](#facebook-chat "Permalink to this headline")
=================================================================



 This notebook covers how to load data from the Facebook Chats into a format that can be ingested into LangChain.
 







```
from langchain.document_loaders import FacebookChatLoader

```










```
loader = FacebookChatLoader("example_data/facebook_chat.json")

```










```
loader.load()

```








```
[Document(page_content='User 2 on 2023-02-05 12:46:11: Bye!\n\nUser 1 on 2023-02-05 12:43:55: Oh no worries! Bye\n\nUser 2 on 2023-02-05 12:24:37: No Im sorry it was my mistake, the blue one is not for sale\n\nUser 1 on 2023-02-05 12:05:40: I thought you were selling the blue one!\n\nUser 1 on 2023-02-05 12:05:09: Im not interested in this bag. Im interested in the blue one!\n\nUser 2 on 2023-02-05 12:04:28: Here is $129\n\nUser 2 on 2023-02-05 12:04:05: Online is at least $100\n\nUser 1 on 2023-02-05 11:59:59: How much do you want?\n\nUser 2 on 2023-02-05 07:17:56: Goodmorning! $50 is too low.\n\nUser 1 on 2023-02-04 23:17:02: Hi! Im interested in your bag. Im offering $50. Let me know if you are interested. Thanks!\n\n', lookup_str='', metadata={'source': 'docs/modules/document_loaders/examples/example_data/facebook_chat.json'}, lookup_index=0)]

```







