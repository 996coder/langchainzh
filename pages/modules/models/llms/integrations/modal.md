

模态框[#](#modal "链接至本标题的永久链接")
============================

[Modal Python Library](https://modal.com/docs/guide)提供了方便的、按需的从本地计算机上的Python脚本访问无服务器云计算的途径。
`Modal`本身并不提供任何LLMs，只提供基础设施。

这个例子介绍了如何使用LangChain与`Modal`交互。

[这里](https://modal.com/docs/guide/ex/potus_speech_qanda)是另一个使用LangChain与`Modal`交互的例子。

```
!pip install modal-client

```

```
# register and get a new token

!modal token new

```

```
[?25lLaunching login page in your browser window...
[2KIf this is not showing up, please copy this URL into your web browser manually:
[2Km⠙ Waiting for authentication in the web browser...
]8;id=417802;https://modal.com/token-flow/tf-ptEuGecm7T1T5YQe42kwM1\[4;94mhttps://modal.com/token-flow/tf-ptEuGecm7T1T5YQe42kwM1]8;;\

[2K⠙ Waiting for authentication in the web browser...
[1A[2K^C

Aborted.

```

请按照[这些说明](https://modal.com/docs/guide/secrets)处理密钥。

```
from langchain.llms import Modal
from langchain import PromptTemplate, LLMChain

```

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

```
llm = Modal(endpoint_url="YOUR_ENDPOINT_URL")

```

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

```
question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"

llm_chain.run(question)

```

