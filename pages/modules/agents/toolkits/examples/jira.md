


 Jira
 [#](#jira "Permalink to this headline")
===============================================



 This notebook goes over how to use the Jira tool.
The Jira tool allows agents to interact with a given Jira instance, performing actions such as searching for issues and creating issues, the tool wraps the atlassian-python-api library, for more see: https://atlassian-python-api.readthedocs.io/jira
 



 To use this tool, you must first set as environment variables:
JIRA_API_TOKEN
JIRA_USERNAME
JIRA_INSTANCE_URL
 







```
%pip install atlassian-python-api

```










```
import os
from langchain.agents import AgentType
from langchain.agents import initialize_agent
from langchain.agents.agent_toolkits.jira.toolkit import JiraToolkit
from langchain.llms import OpenAI
from langchain.utilities.jira import JiraAPIWrapper

```










```
os.environ["JIRA_API_TOKEN"] = "abc"
os.environ["JIRA_USERNAME"] = "123"
os.environ["JIRA_INSTANCE_URL"] = "https://jira.atlassian.com"
os.environ["OPENAI_API_KEY"] = "xyz"

```










```
llm = OpenAI(temperature=0)
jira = JiraAPIWrapper()
toolkit = JiraToolkit.from_jira_api_wrapper(jira)
agent = initialize_agent(
    toolkit.get_tools(),
    llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

```










```
agent.run("make a new issue in project PW to remind me to make more fried rice")

```








```
> Entering new AgentExecutor chain...
 I need to create an issue in project PW
Action: Create Issue
Action Input: {"summary": "Make more fried rice", "description": "Reminder to make more fried rice", "issuetype": {"name": "Task"}, "priority": {"name": "Low"}, "project": {"key": "PW"}}
Observation: None
Thought: I now know the final answer
Final Answer: A new issue has been created in project PW with the summary "Make more fried rice" and description "Reminder to make more fried rice".

> Finished chain.

```






```
'A new issue has been created in project PW with the summary "Make more fried rice" and description "Reminder to make more fried rice".'

```







