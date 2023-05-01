


 BashChain
 [#](#bashchain "Permalink to this headline")
=========================================================



 This notebook showcases using LLMs and a bash process to perform simple filesystem commands.
 







```
from langchain.chains import LLMBashChain
from langchain.llms import OpenAI

llm = OpenAI(temperature=0)

text = "Please write a bash script that prints 'Hello World' to the console."

bash_chain = LLMBashChain.from_llm(llm, verbose=True)

bash_chain.run(text)

```








```
> Entering new LLMBashChain chain...
Please write a bash script that prints 'Hello World' to the console.

```bash
echo "Hello World"
```
Code: ['echo "Hello World"']
Answer: Hello World

> Finished chain.

```






```
'Hello World\n'

```







 Customize Prompt
 [#](#customize-prompt "Permalink to this headline")
-----------------------------------------------------------------------



 You can also customize the prompt that is used. Here is an example prompting to avoid using the ‘echo’ utility
 







```
from langchain.prompts.prompt import PromptTemplate
from langchain.chains.llm_bash.prompt import BashOutputParser

_PROMPT_TEMPLATE = """If someone asks you to perform a task, your job is to come up with a series of bash commands that will perform the task. There is no need to put "#!/bin/bash" in your answer. Make sure to reason step by step, using this format:
Question: "copy the files in the directory named 'target' into a new directory at the same level as target called 'myNewDirectory'"
I need to take the following actions:
- List all files in the directory
- Create a new directory
- Copy the files from the first directory into the second directory
```bash
ls
mkdir myNewDirectory
cp -r target/\* myNewDirectory
```

Do not use 'echo' when writing the script.

That is the format. Begin!
Question: {question}"""

PROMPT = PromptTemplate(input_variables=["question"], template=_PROMPT_TEMPLATE, output_parser=BashOutputParser())

```










```
bash_chain = LLMBashChain.from_llm(llm, prompt=PROMPT, verbose=True)

text = "Please write a bash script that prints 'Hello World' to the console."

bash_chain.run(text)

```








```
> Entering new LLMBashChain chain...
Please write a bash script that prints 'Hello World' to the console.

```bash
printf "Hello World\n"
```
Code: ['printf "Hello World\\n"']
Answer: Hello World

> Finished chain.

```






```
'Hello World\n'

```








 Persistent Terminal
 [#](#persistent-terminal "Permalink to this headline")
-----------------------------------------------------------------------------



 By default, the chain will run in a separate subprocess each time it is called. This behavior can be changed by instantiating with a persistent bash process.
 







```
from langchain.utilities.bash import BashProcess


persistent_process = BashProcess(persistent=True)
bash_chain = LLMBashChain.from_llm(llm, bash_process=persistent_process, verbose=True)

text = "List the current directory then move up a level."

bash_chain.run(text)

```








```
> Entering new LLMBashChain chain...
List the current directory then move up a level.

```bash
ls
cd ..
```
Code: ['ls', 'cd ..']
Answer: api.ipynb llm_summarization_checker.ipynb
constitutional_chain.ipynb moderation.ipynb
llm_bash.ipynb openai_openapi.yaml
llm_checker.ipynb openapi.ipynb
llm_math.ipynb pal.ipynb
llm_requests.ipynb sqlite.ipynb
> Finished chain.

```






```
'api.ipynb\t\t\tllm_summarization_checker.ipynb\r\nconstitutional_chain.ipynb\tmoderation.ipynb\r\nllm_bash.ipynb\t\t\topenai_openapi.yaml\r\nllm_checker.ipynb\t\topenapi.ipynb\r\nllm_math.ipynb\t\t\tpal.ipynb\r\nllm_requests.ipynb\t\tsqlite.ipynb'

```










```
# Run the same command again and see that the state is maintained between calls
bash_chain.run(text)

```








```
> Entering new LLMBashChain chain...
List the current directory then move up a level.

```bash
ls
cd ..
```
Code: ['ls', 'cd ..']
Answer: examples getting_started.ipynb index_examples
generic how_to_guides.rst
> Finished chain.

```






```
'examples\t\tgetting_started.ipynb\tindex_examples\r\ngeneric\t\t\thow_to_guides.rst'

```








