

NLP云[#](#nlp-cloud "此标题的永久链接")
==============================

[NLP云](https://nlpcloud.io)提供高性能的预训练或自定义模型，用于命名实体识别、情感分析、分类、摘要、改写、文法和拼写纠正、关键词和关键词短语提取、聊天机器人、产品描述和广告生成、意图分类、文本生成、图像生成、博客文章生成、代码生成、问答、自动语音识别、机器翻译、语言检测、语义搜索、语义相似度、标记化、词性标注、嵌入和依赖解析。它已经准备好投入生产，通过REST API提供服务。

此示例介绍如何使用LangChain与`NLP Cloud` [模型](https://docs.nlpcloud.com/#models)交互。

```
!pip install nlpcloud

```

```
# get a token: https://docs.nlpcloud.com/#authentication

from getpass import getpass

NLPCLOUD_API_KEY = getpass()

```

```
import os

os.environ["NLPCLOUD_API_KEY"] = NLPCLOUD_API_KEY

```

```
from langchain.llms import NLPCloud
from langchain import PromptTemplate, LLMChain

```

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

```
llm = NLPCloud()

```

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

```
question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"

llm_chain.run(question)

```

```
' Justin Bieber was born in 1994, so the team that won the Super Bowl that year was the San Francisco 49ers.'

```

