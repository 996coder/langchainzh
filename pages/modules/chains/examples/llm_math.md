


 LLM Math
 [#](#llm-math "Permalink to this headline")
=======================================================



 This notebook showcases using LLMs and Python REPLs to do complex word math problems.
 







```
from langchain import OpenAI, LLMMathChain

llm = OpenAI(temperature=0)
llm_math = LLMMathChain.from_llm(llm, verbose=True)

llm_math.run("What is 13 raised to the .3432 power?")

```








```
> Entering new LLMMathChain chain...
What is 13 raised to the .3432 power?
```text
13 \*\* .3432
```
...numexpr.evaluate("13 \*\* .3432")...

Answer: 2.4116004626599237
> Finished chain.

```






```
'Answer: 2.4116004626599237'

```







