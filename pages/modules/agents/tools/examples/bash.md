


 Shell Tool
 [#](#shell-tool "Permalink to this headline")
===========================================================



 Giving agents access to the shell is powerful (though risky outside a sandboxed environment).
 



 The LLM can use it to execute any shell commands. A common use case for this is letting the LLM interact with your local file system.
 







```
from langchain.tools import ShellTool

shell_tool = ShellTool()

```










```
print(shell_tool.run({"commands": ["echo 'Hello World!'", "time"]}))

```








```
Hello World!

real	0m0.000s
user	0m0.000s
sys	0m0.000s

```






```
/Users/wfh/code/lc/lckg/langchain/tools/shell/tool.py:34: UserWarning: The shell tool has no safeguards by default. Use at your own risk.
  warnings.warn(

```







 Use with Agents
 [#](#use-with-agents "Permalink to this headline")
---------------------------------------------------------------------



 As with all tools, these can be given to an agent to accomplish more complex tasks. Let’s have the agent fetch some links from a web page.
 







```
from langchain.chat_models import ChatOpenAI
from langchain.agents import initialize_agent
from langchain.agents import AgentType

llm = ChatOpenAI(temperature=0)

shell_tool.description = shell_tool.description + f"args {shell_tool.args}".replace("{", "{{").replace("}", "}}")
self_ask_with_search = initialize_agent([shell_tool], llm, agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION, verbose=True)
self_ask_with_search.run("Download the langchain.com webpage and grep for all urls. Return only a sorted list of them. Be sure to use double quotes.")

```








```
> Entering new AgentExecutor chain...
Question: What is the task?
Thought: We need to download the langchain.com webpage and extract all the URLs from it. Then we need to sort the URLs and return them.
Action:
```
{
 "action": "shell",
 "action_input": {
 "commands": [
 "curl -s https://langchain.com | grep -o 'http[s]\*://[^\" ]\*' | sort"
 ]
 }
}
```


```






```
/Users/wfh/code/lc/lckg/langchain/tools/shell/tool.py:34: UserWarning: The shell tool has no safeguards by default. Use at your own risk.
  warnings.warn(

```






```
Observation: https://blog.langchain.dev/
https://discord.gg/6adMQxSpJS
https://docs.langchain.com/docs/
https://github.com/hwchase17/chat-langchain
https://github.com/hwchase17/langchain
https://github.com/hwchase17/langchainjs
https://github.com/sullivan-sean/chat-langchainjs
https://js.langchain.com/docs/
https://python.langchain.com/en/latest/
https://twitter.com/langchainai

Thought:The URLs have been successfully extracted and sorted. We can return the list of URLs as the final answer.
Final Answer: ["https://blog.langchain.dev/", "https://discord.gg/6adMQxSpJS", "https://docs.langchain.com/docs/", "https://github.com/hwchase17/chat-langchain", "https://github.com/hwchase17/langchain", "https://github.com/hwchase17/langchainjs", "https://github.com/sullivan-sean/chat-langchainjs", "https://js.langchain.com/docs/", "https://python.langchain.com/en/latest/", "https://twitter.com/langchainai"]

> Finished chain.

```






```
'["https://blog.langchain.dev/", "https://discord.gg/6adMQxSpJS", "https://docs.langchain.com/docs/", "https://github.com/hwchase17/chat-langchain", "https://github.com/hwchase17/langchain", "https://github.com/hwchase17/langchainjs", "https://github.com/sullivan-sean/chat-langchainjs", "https://js.langchain.com/docs/", "https://python.langchain.com/en/latest/", "https://twitter.com/langchainai"]'

```








