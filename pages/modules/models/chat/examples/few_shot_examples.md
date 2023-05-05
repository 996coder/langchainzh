

如何使用few shot示例[#](#how-to-use-few-shot-examples "此标题的永久链接")
===========================================================

本笔记本涵盖了如何在聊天模型中使用few shot示例。

目前似乎没有关于如何最好地进行few shot提示的坚实共识。因此，我们尚未巩固任何关于此的抽象，而是使用现有的抽象。

交替的人类/ AI消息[#](#alternating-human-ai-messages "此标题的永久链接")
---------------------------------------------------------

进行few shot提示的第一种方法是使用交替的人类/ AI消息。请参见下面的示例。

```
from langchain.chat_models import ChatOpenAI
from langchain import PromptTemplate, LLMChain
from langchain.prompts.chat import (
    ChatPromptTemplate,
    SystemMessagePromptTemplate,
    AIMessagePromptTemplate,
    HumanMessagePromptTemplate,
)
from langchain.schema import (
    AIMessage,
    HumanMessage,
    SystemMessage
)

```

```
chat = ChatOpenAI(temperature=0)

```

```
template="You are a helpful assistant that translates english to pirate."
system_message_prompt = SystemMessagePromptTemplate.from_template(template)
example_human = HumanMessagePromptTemplate.from_template("Hi")
example_ai = AIMessagePromptTemplate.from_template("Argh me mateys")
human_template="{text}"
human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)

```

```
chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, example_human, example_ai, human_message_prompt])
chain = LLMChain(llm=chat, prompt=chat_prompt)
# get a chat completion from the formatted messages
chain.run("I love programming.")

```

```
"I be lovin' programmin', me hearty!"

```

系统消息[#](#system-messages "此标题的永久链接")
------------------------------------

OpenAI提供了一个可选的`name`参数，他们也建议与系统消息一起使用进行few shot提示。以下是如何执行此操作的示例。

```
template="You are a helpful assistant that translates english to pirate."
system_message_prompt = SystemMessagePromptTemplate.from_template(template)
example_human = SystemMessagePromptTemplate.from_template("Hi", additional_kwargs={"name": "example_user"})
example_ai = SystemMessagePromptTemplate.from_template("Argh me mateys", additional_kwargs={"name": "example_assistant"})
human_template="{text}"
human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)

```

```
chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, example_human, example_ai, human_message_prompt])
chain = LLMChain(llm=chat, prompt=chat_prompt)
# get a chat completion from the formatted messages
chain.run("I love programming.")

```

```
"I be lovin' programmin', me hearty."

```

