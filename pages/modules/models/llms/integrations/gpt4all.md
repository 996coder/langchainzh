

GPT4All[#](#gpt4all "永久链接")
===========================

[GitHub：nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) 是一个基于大量干净的助手数据集训练的开源聊天机器人生态系统，其中包括代码、故事和对话。

此示例介绍如何使用LangChain与GPT4All模型交互。

```
%pip install pygpt4all > /dev/null

```

```
Note: you may need to restart the kernel to use updated packages.

```

```
from langchain import PromptTemplate, LLMChain
from langchain.llms import GPT4All
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler

```

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

指定模型[#](#specify-model "永久链接")
------------------------------

要在本地运行，请下载兼容的ggml格式模型。有关更多信息，请访问https://github.com/nomic-ai/pygpt4all

有关完整的安装说明，请单击[此处](https://gpt4all.io/index.html)。

GPT4All Chat安装程序需要在安装过程中解压缩3GB的LLM模型！

请注意，新模型会定期上传——请查看上面的链接以获取最新的.bin URL

```
local_path = './models/ggml-gpt4all-l13b-snoozy.bin'  # replace with your desired local file path

```

取消下面的块以下载模型。您可能需要更新`url`以获取新版本。

```
# import requests

# from pathlib import Path
# from tqdm import tqdm

# Path(local_path).parent.mkdir(parents=True, exist_ok=True)

# # Example model. Check https://github.com/nomic-ai/pygpt4all for the latest models.
# url = 'http://gpt4all.io/models/ggml-gpt4all-l13b-snoozy.bin'

# # send a GET request to the URL to download the file. Stream since it's large
# response = requests.get(url, stream=True)

# # open the file in binary mode and write the contents of the response to it in chunks
# # This is a large file, so be prepared to wait.
# with open(local_path, 'wb') as f:
# for chunk in tqdm(response.iter_content(chunk_size=8192)):
# if chunk:
# f.write(chunk)

```

```
# Callbacks support token-wise streaming
callbacks = [StreamingStdOutCallbackHandler()]
# Verbose is required to pass to the callback manager
llm = GPT4All(model=local_path, callbacks=callbacks, verbose=True)

```

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

```
question = "What NFL team won the Super Bowl in the year Justin Bieber was born?"

llm_chain.run(question)

```

