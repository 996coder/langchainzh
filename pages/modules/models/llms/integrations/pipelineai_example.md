



PipelineAI允许您在云中规模运行您的ML模型。它还提供API访问[多个LLM模型](https://pipeline.ai)。

这个笔记本介绍了如何使用[PipelineAI](https://docs.pipeline.ai/docs)来使用Langchain。

安装pipeline-ai[#](#install-pipeline-ai "跳转到标题")
----------------------------------------------

使用`pip install pipeline-ai`安装`pipeline-ai`库是使用`PipelineAI` API，也称为`Pipeline Cloud`所必需的。

```
# Install the package
!pip install pipeline-ai

```

导入[#](#imports "跳转到标题")
-----------------------

```
import os
from langchain.llms import PipelineAI
from langchain import PromptTemplate, LLMChain

```

设置环境API密钥[#](#set-the-environment-api-key "跳转到标题")
--------------------------------------------------

Make sure to get your API key from PipelineAI. Check out the [cloud quickstart guide](https://docs.pipeline.ai/docs/cloud-quickstart). You’ll be given a 30 day free trial with 10 hours of serverless GPU compute to test different models.

```
os.environ["PIPELINE_API_KEY"] = "YOUR_API_KEY_HERE"

```

Create the PipelineAI instance[#](#create-the-pipelineai-instance "Permalink to this headline")
-----------------------------------------------------------------------------------------------

When instantiating PipelineAI, you need to specify the id or tag of the pipeline you want to use, e.g. `pipeline_key = "public/gpt-j:base"`. You then have the option of passing additional pipeline-specific keyword arguments:

```
llm = PipelineAI(pipeline_key="YOUR_PIPELINE_KEY", pipeline_kwargs={...})

```

Create a Prompt Template[#](#create-a-prompt-template "Permalink to this headline")
-----------------------------------------------------------------------------------

We will create a prompt template for Question and Answer.

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

Initiate the LLMChain[#](#initiate-the-llmchain "Permalink to this headline")
-----------------------------------------------------------------------------

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

Run the LLMChain[#](#run-the-llmchain "Permalink to this headline")
-------------------------------------------------------------------

提供一个问题并运行LLMChain。

```
question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"

llm_chain.run(question)

```

