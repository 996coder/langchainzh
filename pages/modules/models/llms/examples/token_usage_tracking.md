

如何跟踪令牌使用[#](#how-to-track-token-usage "Permalink to this headline")
===================================================================

本笔记本介绍如何跟踪特定调用的令牌使用情况。目前仅实现了OpenAI API的跟踪。

让我们先看一个极其简单的例子，跟踪单个LLM调用的令牌使用情况。

```
from langchain.llms import OpenAI
from langchain.callbacks import get_openai_callback

```

```
llm = OpenAI(model_name="text-davinci-002", n=2, best_of=2)

```

```
with get_openai_callback() as cb:
    result = llm("Tell me a joke")
    print(cb)

```

```
Tokens Used: 42
	Prompt Tokens: 4
	Completion Tokens: 38
Successful Requests: 1
Total Cost (USD): $0.00084

```

上下文管理器内的任何内容都将被跟踪。以下是使用它来跟踪多个连续调用的示例。

```
with get_openai_callback() as cb:
    result = llm("Tell me a joke")
    result2 = llm("Tell me a joke")
    print(cb.total_tokens)

```

```
91

```

如果使用了具有多个步骤的链或代理，它将跟踪所有这些步骤。

```
from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
from langchain.llms import OpenAI

llm = OpenAI(temperature=0)
tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)

```

```
with get_openai_callback() as cb:
    response = agent.run("Who is Olivia Wilde's boyfriend? What is his current age raised to the 0.23 power?")
    print(f"Total Tokens: {cb.total_tokens}")
    print(f"Prompt Tokens: {cb.prompt_tokens}")
    print(f"Completion Tokens: {cb.completion_tokens}")
    print(f"Total Cost (USD): ${cb.total_cost}")

```

```
> Entering new AgentExecutor chain...
 I need to find out who Olivia Wilde's boyfriend is and then calculate his age raised to the 0.23 power.
Action: Search
Action Input: "Olivia Wilde boyfriend"
Observation: Sudeikis and Wilde's relationship ended in November 2020. Wilde was publicly served with court documents regarding child custody while she was presenting Don't Worry Darling at CinemaCon 2022. In January 2021, Wilde began dating singer Harry Styles after meeting during the filming of Don't Worry Darling.
Thought: I need to find out Harry Styles' age.
Action: Search
Action Input: "Harry Styles age"
Observation: 29 years
Thought: I need to calculate 29 raised to the 0.23 power.
Action: Calculator
Action Input: 29^0.23
Observation: Answer: 2.169459462491557

Thought: I now know the final answer.
Final Answer: Harry Styles, Olivia Wilde's boyfriend, is 29 years old and his age raised to the 0.23 power is 2.169459462491557.

> Finished chain.
Total Tokens: 1506
Prompt Tokens: 1350
Completion Tokens: 156
Total Cost (USD): $0.03012

```

