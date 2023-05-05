

GooseAI[#](#gooseai "Permalink to this headline")
=================================================

`GooseAI`是一个完全托管的NLP-as-a-Service，通过API提供。GooseAI提供访问[这些模型](https://goose.ai/docs/models)。

本笔记本介绍了如何使用[GooseAI](https://goose.ai/)与Langchain。

安装openai[#](#install-openai "Permalink to this headline")
---------------------------------------------------------

使用GooseAI API需要安装`openai`软件包。使用`pip3 install openai`进行安装。

```
$ pip3 install openai

```

导入[#](#imports "Permalink to this headline")
--------------------------------------------

```
import os
from langchain.llms import GooseAI
from langchain import PromptTemplate, LLMChain

```

设置环境API密钥[#](#set-the-environment-api-key "Permalink to this headline")
-----------------------------------------------------------------------

确保从GooseAI获取您的API密钥。您将获得10美元的免费信用以测试不同的模型。

```
from getpass import getpass

GOOSEAI_API_KEY = getpass()

```

```
os.environ["GOOSEAI_API_KEY"] = GOOSEAI_API_KEY

```

创建GooseAI实例[#](#create-the-gooseai-instance "此标题的永久链接")
-------------------------------------------------------

您可以指定不同的参数，如模型名称、生成的最大标记、温度等。

```
llm = GooseAI()

```

创建提示模板[#](#create-a-prompt-template "此标题的永久链接")
-----------------------------------------------

我们将为问题和答案创建提示模板。

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

启动LLMChain[#](#initiate-the-llmchain "此标题的永久链接")
------------------------------------------------

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

运行LLMChain[#](#run-the-llmchain "此标题的永久链接")
-------------------------------------------

提供一个问题并运行LLMChain。

```
question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"

llm_chain.run(question)

```

