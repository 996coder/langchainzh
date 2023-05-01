


 ConversationSummaryMemory
 [#](#conversationsummarymemory "Permalink to this headline")
=========================================================================================



 Now let’s take a look at using a slightly more complex type of memory -
 `ConversationSummaryMemory`
 . This type of memory creates a summary of the conversation over time. This can be useful for condensing information from the conversation over time.
 



 Let’s first explore the basic functionality of this type of memory.
 







```
from langchain.memory import ConversationSummaryMemory
from langchain.llms import OpenAI

```










```
memory = ConversationSummaryMemory(llm=OpenAI(temperature=0))
memory.save_context({"input": "hi"}, {"ouput": "whats up"})

```










```
memory.load_memory_variables({})

```








```
{'history': '\nThe human greets the AI, to which the AI responds.'}

```






 We can also get the history as a list of messages (this is useful if you are using this with a chat model).
 







```
memory = ConversationSummaryMemory(llm=OpenAI(temperature=0), return_messages=True)
memory.save_context({"input": "hi"}, {"ouput": "whats up"})

```










```
memory.load_memory_variables({})

```








```
{'history': [SystemMessage(content='\nThe human greets the AI, to which the AI responds.', additional_kwargs={})]}

```






 We can also utilize the
 `predict_new_summary`
 method directly.
 







```
messages = memory.chat_memory.messages
previous_summary = ""
memory.predict_new_summary(messages, previous_summary)

```








```
'\nThe human greets the AI, to which the AI responds.'

```







 Using in a chain
 [#](#using-in-a-chain "Permalink to this headline")
-----------------------------------------------------------------------



 Let’s walk through an example of using this in a chain, again setting
 `verbose=True`
 so we can see the prompt.
 







```
from langchain.llms import OpenAI
from langchain.chains import ConversationChain
llm = OpenAI(temperature=0)
conversation_with_summary = ConversationChain(
    llm=llm, 
    memory=ConversationSummaryMemory(llm=OpenAI()),
    verbose=True
)
conversation_with_summary.predict(input="Hi, what's up?")

```








```
> Entering new ConversationChain chain...
Prompt after formatting:
The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.

Current conversation:

Human: Hi, what's up?
AI:

> Finished chain.

```






```
" Hi there! I'm doing great. I'm currently helping a customer with a technical issue. How about you?"

```










```
conversation_with_summary.predict(input="Tell me more about it!")

```








```
> Entering new ConversationChain chain...
Prompt after formatting:
The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.

Current conversation:

The human greeted the AI and asked how it was doing. The AI replied that it was doing great and was currently helping a customer with a technical issue.
Human: Tell me more about it!
AI:

> Finished chain.

```






```
" Sure! The customer is having trouble with their computer not connecting to the internet. I'm helping them troubleshoot the issue and figure out what the problem is. So far, we've tried resetting the router and checking the network settings, but the issue still persists. We're currently looking into other possible solutions."

```










```
conversation_with_summary.predict(input="Very cool -- what is the scope of the project?")

```








```
> Entering new ConversationChain chain...
Prompt after formatting:
The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.

Current conversation:

The human greeted the AI and asked how it was doing. The AI replied that it was doing great and was currently helping a customer with a technical issue where their computer was not connecting to the internet. The AI was troubleshooting the issue and had already tried resetting the router and checking the network settings, but the issue still persisted and they were looking into other possible solutions.
Human: Very cool -- what is the scope of the project?
AI:

> Finished chain.

```






```
" The scope of the project is to troubleshoot the customer's computer issue and find a solution that will allow them to connect to the internet. We are currently exploring different possibilities and have already tried resetting the router and checking the network settings, but the issue still persists."

```








