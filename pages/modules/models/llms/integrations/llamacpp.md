

Llama-cpp[#](#llama-cpp "此标题的永久链接")
===================================

[llama-cpp](https://github.com/abetlen/llama-cpp-python) 是 [llama.cpp](https://github.com/ggerganov/llama.cpp) 的 Python 绑定。
它支持 [多个 LLMs](https://github.com/ggerganov/llama.cpp)。

本笔记本介绍如何在 LangChain 中运行 `llama-cpp`。

```
!pip install llama-cpp-python

```

请确保您遵循所有说明以[安装所有必要的模型文件](https://github.com/ggerganov/llama.cpp)。

您不需要一个 `API_TOKEN`！

```
from langchain.llms import LlamaCpp
from langchain import PromptTemplate, LLMChain
from langchain.callbacks.manager import CallbackManager
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler

```

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

```
# Callbacks support token-wise streaming
callback_manager = CallbackManager([StreamingStdOutCallbackHandler()])
# Verbose is required to pass to the callback manager

# Make sure the model path is correct for your system!
llm = LlamaCpp(
    model_path="./ggml-model-q4_0.bin", callback_manager=callback_manager, verbose=True
)

```

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

```
question = "What NFL team won the Super Bowl in the year Justin Bieber was born?"

llm_chain.run(question)

```

```
 First we need to identify what year Justin Beiber was born in. A quick google search reveals that he was born on March 1st, 1994. Now we know when the Super Bowl was played in, so we can look up which NFL team won it. The NFL Superbowl of the year 1994 was won by the San Francisco 49ers against the San Diego Chargers.

```

```
' First we need to identify what year Justin Beiber was born in. A quick google search reveals that he was born on March 1st, 1994. Now we know when the Super Bowl was played in, so we can look up which NFL team won it. The NFL Superbowl of the year 1994 was won by the San Francisco 49ers against the San Diego Chargers.'

```

