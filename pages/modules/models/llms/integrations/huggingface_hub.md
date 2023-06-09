

拥抱面孔中心[#](#hugging-face-hub "跳转到标题链接")
======================================

[拥抱面孔中心](https://huggingface.co/docs/hub/index)是一个平台，拥有超过120k个模型、20k个数据集和50k个演示应用程序（空间），所有内容都是开源和公开的，在这个在线平台上，人们可以轻松合作和构建机器学习。

此示例展示了如何连接到拥抱面孔中心。

要使用，您应该安装了`huggingface_hub`的python[软件包](https://huggingface.co/docs/huggingface_hub/installation)。

```
!pip install huggingface_hub > /dev/null

```

```
# get a token: https://huggingface.co/docs/api-inference/quicktour#get-your-api-token

from getpass import getpass

HUGGINGFACEHUB_API_TOKEN = getpass()

```

```
import os
os.environ["HUGGINGFACEHUB_API_TOKEN"] = HUGGINGFACEHUB_API_TOKEN

```

**选择模型**

```
from langchain import HuggingFaceHub

repo_id = "google/flan-t5-xl" # See https://huggingface.co/models?pipeline_tag=text-generation&sort=downloads for some other options

llm = HuggingFaceHub(repo_id=repo_id, model_kwargs={"temperature":0, "max_length":64})

```

```
from langchain import PromptTemplate, LLMChain

template = """Question: {question}

Answer: Let's think step by step."""
prompt = PromptTemplate(template=template, input_variables=["question"])
llm_chain = LLMChain(prompt=prompt, llm=llm)

question = "Who won the FIFA World Cup in the year 1994? "

print(llm_chain.run(question))

```

示例[#](#examples "跳转到标题链接")
--------------------------

以下是通过拥抱面孔中心集成可以访问的一些模型示例。

### 由稳定性AI提供的StableLM[#](#stablelm-by-stability-ai "跳转到标题链接")

请参阅[稳定性AI](https://huggingface.co/stabilityai)的组织页面以获取可用模型列表。

```
repo_id = "stabilityai/stablelm-tuned-alpha-3b"
# Others include stabilityai/stablelm-base-alpha-3b
# as well as 7B parameter versions

```

```
llm = HuggingFaceHub(repo_id=repo_id, model_kwargs={"temperature":0, "max_length":64})

```

```
# Reuse the prompt and question from above.
llm_chain = LLMChain(prompt=prompt, llm=llm)
print(llm_chain.run(question))

```

### DataBricks的Dolly[#](#dolly-by-databricks "Permalink to this headline")

请查看[DataBricks](https://huggingface.co/databricks)组织页面，了解可用模型列表。

```
from langchain import HuggingFaceHub

repo_id = "databricks/dolly-v2-3b"

llm = HuggingFaceHub(repo_id=repo_id, model_kwargs={"temperature":0, "max_length":64})

```

```
# Reuse the prompt and question from above.
llm_chain = LLMChain(prompt=prompt, llm=llm)
print(llm_chain.run(question))

```

### Writer的Camel[#](#camel-by-writer "Permalink to this headline")

请查看[Writer](https://huggingface.co/Writer)组织页面，了解可用模型列表。

```
from langchain import HuggingFaceHub

repo_id = "Writer/camel-5b-hf" # See https://huggingface.co/Writer for other options
llm = HuggingFaceHub(repo_id=repo_id, model_kwargs={"temperature":0, "max_length":64})

```

```
# Reuse the prompt and question from above.
llm_chain = LLMChain(prompt=prompt, llm=llm)
print(llm_chain.run(question))

```

**还有更多！**

