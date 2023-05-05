

少量样本示例的提示模板
========


在本教程中，我们将学习如何创建使用少量样本示例的提示模板。

我们将使用`FewShotPromptTemplate`类来创建使用少量样本示例的提示模板。此类要么接受一组示例，要么接受一个`ExampleSelector`对象。在本教程中，我们将介绍这两个选项。

用例[#](#use-case "本标题的永久链接")
---------------------------

在本教程中，我们将为自我提问与搜索配置少量样本示例。

使用示例集[#](#using-an-example-set "本标题的永久链接")
------------------------------------------

### 创建示例集[#](#create-the-example-set "本标题的永久链接")

首先，创建少量样本示例列表。每个示例应该是一个字典，键是输入变量，值是这些输入变量的值。

```
from langchain.prompts.few_shot import FewShotPromptTemplate
from langchain.prompts.prompt import PromptTemplate

examples = [
  {
    "question": "Who lived longer, Muhammad Ali or Alan Turing?",
    "answer": 
"""
Are follow up questions needed here: Yes.
Follow up: How old was Muhammad Ali when he died?
Intermediate answer: Muhammad Ali was 74 years old when he died.
Follow up: How old was Alan Turing when he died?
Intermediate answer: Alan Turing was 41 years old when he died.
So the final answer is: Muhammad Ali
"""
  },
  {
    "question": "When was the founder of craigslist born?",
    "answer": 
"""
Are follow up questions needed here: Yes.
Follow up: Who was the founder of craigslist?
Intermediate answer: Craigslist was founded by Craig Newmark.
Follow up: When was Craig Newmark born?
Intermediate answer: Craig Newmark was born on December 6, 1952.
So the final answer is: December 6, 1952
"""
  },
  {
    "question": "Who was the maternal grandfather of George Washington?",
    "answer":
"""
Are follow up questions needed here: Yes.
Follow up: Who was the mother of George Washington?
Intermediate answer: The mother of George Washington was Mary Ball Washington.
Follow up: Who was the father of Mary Ball Washington?
Intermediate answer: The father of Mary Ball Washington was Joseph Ball.
So the final answer is: Joseph Ball
"""
  },
  {
    "question": "Are both the directors of Jaws and Casino Royale from the same country?",
    "answer":
"""
Are follow up questions needed here: Yes.
Follow up: Who is the director of Jaws?
Intermediate Answer: The director of Jaws is Steven Spielberg.
Follow up: Where is Steven Spielberg from?
Intermediate Answer: The United States.
Follow up: Who is the director of Casino Royale?
Intermediate Answer: The director of Casino Royale is Martin Campbell.
Follow up: Where is Martin Campbell from?
Intermediate Answer: New Zealand.
So the final answer is: No
"""
  }
]

```

### Create a formatter for the few shot examples[#](#create-a-formatter-for-the-few-shot-examples "Permalink to this headline")

Configure a formatter that will format the few shot examples into a string. This formatter should be a `PromptTemplate` object.

```
example_prompt = PromptTemplate(input_variables=["question", "answer"], template="Question: {question}\n{answer}")

print(example_prompt.format(**examples[0]))

```

```
Question: Who lived longer, Muhammad Ali or Alan Turing?

Are follow up questions needed here: Yes.
Follow up: How old was Muhammad Ali when he died?
Intermediate answer: Muhammad Ali was 74 years old when he died.
Follow up: How old was Alan Turing when he died?
Intermediate answer: Alan Turing was 41 years old when he died.
So the final answer is: Muhammad Ali

```

### Feed examples and formatter to `FewShotPromptTemplate`[#](#feed-examples-and-formatter-to-fewshotprompttemplate "Permalink to this headline")

Finally, create a `FewShotPromptTemplate` object. This object takes in the few shot examples and the formatter for the few shot examples.

```
prompt = FewShotPromptTemplate(
    examples=examples, 
    example_prompt=example_prompt, 
    suffix="Question: {input}", 
    input_variables=["input"]
)

print(prompt.format(input="Who was the father of Mary Ball Washington?"))

```

```
Question: Who lived longer, Muhammad Ali or Alan Turing?

Are follow up questions needed here: Yes.
Follow up: How old was Muhammad Ali when he died?
Intermediate answer: Muhammad Ali was 74 years old when he died.
Follow up: How old was Alan Turing when he died?
Intermediate answer: Alan Turing was 41 years old when he died.
So the final answer is: Muhammad Ali

Question: When was the founder of craigslist born?

Are follow up questions needed here: Yes.
Follow up: Who was the founder of craigslist?
Intermediate answer: Craigslist was founded by Craig Newmark.
Follow up: When was Craig Newmark born?
Intermediate answer: Craig Newmark was born on December 6, 1952.
So the final answer is: December 6, 1952

Question: Who was the maternal grandfather of George Washington?

Are follow up questions needed here: Yes.
Follow up: Who was the mother of George Washington?
Intermediate answer: The mother of George Washington was Mary Ball Washington.
Follow up: Who was the father of Mary Ball Washington?
Intermediate answer: The father of Mary Ball Washington was Joseph Ball.
So the final answer is: Joseph Ball

Question: Are both the directors of Jaws and Casino Royale from the same country?

Are follow up questions needed here: Yes.
Follow up: Who is the director of Jaws?
Intermediate Answer: The director of Jaws is Steven Spielberg.
Follow up: Where is Steven Spielberg from?
Intermediate Answer: The United States.
Follow up: Who is the director of Casino Royale?
Intermediate Answer: The director of Casino Royale is Martin Campbell.
Follow up: Where is Martin Campbell from?
Intermediate Answer: New Zealand.
So the final answer is: No

Question: Who was the father of Mary Ball Washington?

```

Using an example selector[#](#using-an-example-selector "Permalink to this headline")
-------------------------------------------------------------------------------------

### Feed examples into `ExampleSelector`[#](#feed-examples-into-exampleselector "Permalink to this headline")

我们将重用前一节中的示例集和格式化程序。但是，我们将不直接将示例馈送到`FewShotPromptTemplate`对象中，而是将其馈送到`ExampleSelector`对象中。

在本教程中，我们将使用`SemanticSimilarityExampleSelector`类。该类基于输入的相似性选择few shot示例。它使用嵌入模型计算输入和few shot示例之间的相似性，以及向量存储库执行最近邻搜索。

```
from langchain.prompts.example_selector import SemanticSimilarityExampleSelector
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings

example_selector = SemanticSimilarityExampleSelector.from_examples(
    # This is the list of examples available to select from.
    examples,
    # This is the embedding class used to produce embeddings which are used to measure semantic similarity.
    OpenAIEmbeddings(),
    # This is the VectorStore class that is used to store the embeddings and do a similarity search over.
    Chroma,
    # This is the number of examples to produce.
    k=1
)

# Select the most similar example to the input.
question = "Who was the father of Mary Ball Washington?"
selected_examples = example_selector.select_examples({"question": question})
print(f"Examples most similar to the input: {question}")
for example in selected_examples:
    print("\n")
    for k, v in example.items():
        print(f"{k}: {v}")

```

```
Running Chroma using direct local API.
Using DuckDB in-memory for database. Data will be transient.
Examples most similar to the input: Who was the father of Mary Ball Washington?

question: Who was the maternal grandfather of George Washington?
answer: 
Are follow up questions needed here: Yes.
Follow up: Who was the mother of George Washington?
Intermediate answer: The mother of George Washington was Mary Ball Washington.
Follow up: Who was the father of Mary Ball Washington?
Intermediate answer: The father of Mary Ball Washington was Joseph Ball.
So the final answer is: Joseph Ball

```

### 将示例选择器馈送到`FewShotPromptTemplate`[#](#feed-example-selector-into-fewshotprompttemplate "此标题的永久链接")

最后，创建一个`FewShotPromptTemplate`对象。该对象接受示例选择器和few shot示例的格式化程序。

```
prompt = FewShotPromptTemplate(
    example_selector=example_selector, 
    example_prompt=example_prompt, 
    suffix="Question: {input}", 
    input_variables=["input"]
)

print(prompt.format(input="Who was the father of Mary Ball Washington?"))

```

```
Question: Who was the maternal grandfather of George Washington?

Are follow up questions needed here: Yes.
Follow up: Who was the mother of George Washington?
Intermediate answer: The mother of George Washington was Mary Ball Washington.
Follow up: Who was the father of Mary Ball Washington?
Intermediate answer: The father of Mary Ball Washington was Joseph Ball.
So the final answer is: Joseph Ball

Question: Who was the father of Mary Ball Washington?

```

