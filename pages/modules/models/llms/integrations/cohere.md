

Cohere[#](#cohere "Permalink to this headline")
===============================================

[Cohere](https://cohere.ai/about)是一家加拿大初创公司，提供自然语言处理模型，帮助企业改善人机交互。此示例介绍如何使用LangChain与`Cohere` [models](https://docs.cohere.ai/docs/generation-card) 进行交互。

```
# Install the package
!pip install cohere

```

```
# get a new token: https://dashboard.cohere.ai/

from getpass import getpass

COHERE_API_KEY = getpass()

```

```
from langchain.llms import Cohere
from langchain import PromptTemplate, LLMChain

```

```
template = """Question: {question}

Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

```

```
llm = Cohere(cohere_api_key=COHERE_API_KEY)

```

```
llm_chain = LLMChain(prompt=prompt, llm=llm)

```

```
question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"

llm_chain.run(question)

```

```
" Let's start with the year that Justin Beiber was born. You know that he was born in 1994. We have to go back one year. 1993.  1993 was the year that the Dallas Cowboys won the Super Bowl. They won over the Buffalo Bills in Super Bowl 26.  Now, let's do it backwards. According to our information, the Green Bay Packers last won the Super Bowl in the 2010-2011 season. Now, we can't go back in time, so let's go from 2011 when the Packers won the Super Bowl, back to 1984. That is the year that the Packers won the Super Bowl over the Raiders.  So, we have the year that Justin Beiber was born, 1994, and the year that the Packers last won the Super Bowl, 2011, and now we have to go in the middle, 1986. That is the year that the New York Giants won the Super Bowl over the Denver Broncos. The Giants won Super Bowl 21.  The New York Giants won the Super Bowl in 1986. This means that the Green Bay Packers won the Super Bowl in 2011.  Did you get it right? If you are still a bit confused, just try to go back to the question again and review the answer"

```

