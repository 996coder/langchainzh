

入门指南[#](#getting-started "此标题的永久链接")
====================================

本笔记本涵盖了如何开始使用聊天模型。该接口基于消息而不是原始文本。

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

通过向聊天模型传递一个或多个消息，您可以获得聊天完成。响应将是一条消息。LangChain目前支持的消息类型有`AIMessage`、`HumanMessage`、`SystemMessage`和`ChatMessage` - `ChatMessage`接受任意角色参数。大多数情况下，您只需处理`HumanMessage`、`AIMessage`和`SystemMessage`

```
chat([HumanMessage(content="Translate this sentence from English to French. I love programming.")])

```

```
AIMessage(content="J'aime programmer.", additional_kwargs={})

```

OpenAI的聊天模型支持多个消息作为输入。有关更多信息，请参见[此处](https://platform.openai.com/docs/guides/chat/chat-vs-completions)。下面是向聊天模型发送系统和用户消息的示例：

```
messages = [
    SystemMessage(content="You are a helpful assistant that translates English to French."),
    HumanMessage(content="I love programming.")
]
chat(messages)

```

```
AIMessage(content="J'aime programmer.", additional_kwargs={})

```

您可以进一步使用`generate`来生成多组消息的完成，这将返回一个带有额外`message`参数的`LLMResult`。

```
batch_messages = [
    [
        SystemMessage(content="You are a helpful assistant that translates English to French."),
        HumanMessage(content="I love programming.")
    ],
    [
        SystemMessage(content="You are a helpful assistant that translates English to French."),
        HumanMessage(content="I love artificial intelligence.")
    ],
]
result = chat.generate(batch_messages)
result

```

```
LLMResult(generations=[[ChatGeneration(text="J'aime programmer.", generation_info=None, message=AIMessage(content="J'aime programmer.", additional_kwargs={}))], [ChatGeneration(text="J'aime l'intelligence artificielle.", generation_info=None, message=AIMessage(content="J'aime l'intelligence artificielle.", additional_kwargs={}))]], llm_output={'token_usage': {'prompt_tokens': 57, 'completion_tokens': 20, 'total_tokens': 77}})

```

您可以从这个LLMResult中恢复诸如令牌使用情况之类的东西

```
result.llm_output

```

```
{'token_usage': {'prompt_tokens': 57,
  'completion_tokens': 20,
  'total_tokens': 77}}

```

PromptTemplates[#](#prompttemplates "Permalink to this headline")
-----------------------------------------------------------------

您可以通过使用`MessagePromptTemplate`来利用模板。您可以从一个或多个`MessagePromptTemplates`构建一个`ChatPromptTemplate`。您可以使用`ChatPromptTemplate`的`format_prompt` - 这将返回一个`PromptValue`，您可以将其转换为字符串或消息对象，具体取决于您是否想要将格式化值用作llm或chat模型的输入。

为了方便起见，模板上公开了一个`from_template`方法。如果您使用此模板，它将如下所示：

```
template="You are a helpful assistant that translates {input_language} to {output_language}."
system_message_prompt = SystemMessagePromptTemplate.from_template(template)
human_template="{text}"
human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)

```

```
chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, human_message_prompt])

# get a chat completion from the formatted messages
chat(chat_prompt.format_prompt(input_language="English", output_language="French", text="I love programming.").to_messages())

```

```
AIMessage(content="J'adore la programmation.", additional_kwargs={})

```

如果您想更直接地构建MessagePromptTemplate，可以在外部创建PromptTemplate，然后传入，例如：

```
prompt=PromptTemplate(
    template="You are a helpful assistant that translates {input_language} to {output_language}.",
    input_variables=["input_language", "output_language"],
)
system_message_prompt = SystemMessagePromptTemplate(prompt=prompt)

```

LLMChain[#](#llmchain "Permalink to this headline")
---------------------------------------------------

您可以像以前一样使用现有的LLMChain-提供提示和模型。

```
chain = LLMChain(llm=chat, prompt=chat_prompt)

```

```
chain.run(input_language="English", output_language="French", text="I love programming.")

```

```
"J'adore la programmation."

```

流处理[#](#streaming "Permalink to this headline")
-----------------------------------------------

通过回调处理，`ChatOpenAI`支持流处理。

```
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler
chat = ChatOpenAI(streaming=True, callbacks=[StreamingStdOutCallbackHandler()], temperature=0)
resp = chat([HumanMessage(content="Write me a song about sparkling water.")])

```

```
Verse 1:
Bubbles rising to the top
A refreshing drink that never stops
Clear and crisp, it's pure delight
A taste that's sure to excite

Chorus:
Sparkling water, oh so fine
A drink that's always on my mind
With every sip, I feel alive
Sparkling water, you're my vibe

Verse 2:
No sugar, no calories, just pure bliss
A drink that's hard to resist
It's the perfect way to quench my thirst
A drink that always comes first

Chorus:
Sparkling water, oh so fine
A drink that's always on my mind
With every sip, I feel alive
Sparkling water, you're my vibe

Bridge:
From the mountains to the sea
Sparkling water, you're the key
To a healthy life, a happy soul
A drink that makes me feel whole

Chorus:
Sparkling water, oh so fine
A drink that's always on my mind
With every sip, I feel alive
Sparkling water, you're my vibe

Outro:
Sparkling water, you're the one
A drink that's always so much fun
I'll never let you go, my friend
Sparkling

```

