


 LLMCheckerChain
 [#](#llmcheckerchain "Permalink to this headline")
=====================================================================



 This notebook showcases how to use LLMCheckerChain.
 







```
from langchain.chains import LLMCheckerChain
from langchain.llms import OpenAI

llm = OpenAI(temperature=0.7)

text = "What type of mammal lays the biggest eggs?"

checker_chain = LLMCheckerChain.from_llm(llm, verbose=True)

checker_chain.run(text)

```








```
> Entering new LLMCheckerChain chain...


> Entering new SequentialChain chain...

> Finished chain.

> Finished chain.

```






```
' No mammal lays the biggest eggs. The Elephant Bird, which was a species of giant bird, laid the largest eggs of any bird.'

```







