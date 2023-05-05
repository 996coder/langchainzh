与聊天相关的提示模板
====


[Chat Models](../models/chat)以聊天消息列表作为输入——这个列表通常称为提示。这些聊天消息与原始字符串（您将传递给[LLM](../models/llms)模型的字符串）不同，因为每个消息都与一个角色相关联。

例如，在OpenAI的[Chat Completion API](https://platform.openai.com/docs/guides/chat/introduction)中，聊天消息可以与AI、人类或系统角色相关联。模型应更密切地遵循系统聊天消息的指示。

因此，LangChain提供了几个相关的提示模板，以便轻松构建和处理提示。在查询聊天模型时，建议您使用这些与聊天相关的提示模板，而不是`PromptTemplate`，以充分发挥基础聊天模型的潜力。

```
from langchain.prompts import (
    ChatPromptTemplate,
    PromptTemplate,
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

To create a message template associated with a role, you use `MessagePromptTemplate`.

For convenience, there is a `from_template` method exposed on the template. If you were to use this template, this is what it would look like:

```
template="You are a helpful assistant that translates {input_language} to {output_language}."
system_message_prompt = SystemMessagePromptTemplate.from_template(template)
human_template="{text}"
human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)

```

If you wanted to construct the `MessagePromptTemplate` more directly, you could create a PromptTemplate outside and then pass it in, eg:

```
prompt=PromptTemplate(
    template="You are a helpful assistant that translates {input_language} to {output_language}.",
    input_variables=["input_language", "output_language"],
)
system_message_prompt_2 = SystemMessagePromptTemplate(prompt=prompt)

assert system_message_prompt == system_message_prompt_2

```

After that, you can build a `ChatPromptTemplate` from one or more `MessagePromptTemplates`. You can use `ChatPromptTemplate`’s `format_prompt` – this returns a `PromptValue`, which you can convert to a string or Message object, depending on whether you want to use the formatted value as input to an llm or chat model.

```
chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, human_message_prompt])

# get a chat completion from the formatted messages
chat_prompt.format_prompt(input_language="English", output_language="French", text="I love programming.").to_messages()

```

```
[SystemMessage(content='You are a helpful assistant that translates English to French.', additional_kwargs={}),
 HumanMessage(content='I love programming.', additional_kwargs={})]

```

Format output[#](#format-output "Permalink to this headline")
-------------------------------------------------------------

The output of the format method is available as string, list of messages and `ChatPromptValue`

As string:

```
output = chat_prompt.format(input_language="English", output_language="French", text="I love programming.")
output

```

```
'System: You are a helpful assistant that translates English to French.\nHuman: I love programming.'

```

```
# or alternatively 
output_2 = chat_prompt.format_prompt(input_language="English", output_language="French", text="I love programming.").to_string()

assert output == output_2

```

As `ChatPromptValue`

```
chat_prompt.format_prompt(input_language="English", output_language="French", text="I love programming.")

```

```
ChatPromptValue(messages=[SystemMessage(content='You are a helpful assistant that translates English to French.', additional_kwargs={}), HumanMessage(content='I love programming.', additional_kwargs={})])

```

As list of Message objects

```
chat_prompt.format_prompt(input_language="English", output_language="French", text="I love programming.").to_messages()

```

```
[SystemMessage(content='You are a helpful assistant that translates English to French.', additional_kwargs={}),
 HumanMessage(content='I love programming.', additional_kwargs={})]

```

不同类型的 `MessagePromptTemplate`[#](#different-types-of-messageprompttemplate "Permalink to this headline")
--------------------------------------------------------------------------------------------------------

LangChain 提供了不同类型的 `MessagePromptTemplate`。其中最常用的是 `AIMessagePromptTemplate`、`SystemMessagePromptTemplate` 和 `HumanMessagePromptTemplate`，分别用于创建 AI 消息、系统消息和人类消息。

但是，在聊天模型支持使用任意角色发送聊天消息的情况下，您可以使用 `ChatMessagePromptTemplate`，允许用户指定角色名称。

```
from langchain.prompts import ChatMessagePromptTemplate

prompt = "May the {subject} be with you"

chat_message_prompt = ChatMessagePromptTemplate.from_template(role="Jedi", template=prompt)
chat_message_prompt.format(subject="force")

```

```
ChatMessage(content='May the force be with you', additional_kwargs={}, role='Jedi')

```

LangChain 还提供了 `MessagesPlaceholder`，该占位符可以在格式化期间完全控制要呈现的消息。当您不确定应该使用哪个消息提示模板的角色或者希望在格式化期间插入消息列表时，这可能非常有用。

```
from langchain.prompts import MessagesPlaceholder

human_prompt = "Summarize our conversation so far in {word_count} words."
human_message_template = HumanMessagePromptTemplate.from_template(human_prompt)

chat_prompt = ChatPromptTemplate.from_messages([MessagesPlaceholder(variable_name="conversation"), human_message_template])

```

```
human_message = HumanMessage(content="What is the best way to learn programming?")
ai_message = AIMessage(content="""\
1. Choose a programming language: Decide on a programming language that you want to learn. 

2. Start with the basics: Familiarize yourself with the basic programming concepts such as variables, data types and control structures.

3. Practice, practice, practice: The best way to learn programming is through hands-on experience\
""")

chat_prompt.format_prompt(conversation=[human_message, ai_message], word_count="10").to_messages()

```

```
[HumanMessage(content='What is the best way to learn programming?', additional_kwargs={}),
 AIMessage(content='1. Choose a programming language: Decide on a programming language that you want to learn.   2. Start with the basics: Familiarize yourself with the basic programming concepts such as variables, data types and control structures.  3. Practice, practice, practice: The best way to learn programming is through hands-on experience', additional_kwargs={}),
 HumanMessage(content='Summarize our conversation so far in 10 words.', additional_kwargs={})]

```

