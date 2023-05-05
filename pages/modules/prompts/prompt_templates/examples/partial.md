部分格式化提示模板
=====

提示模板是具有`.format`方法的类，该方法接受键-值映射并返回字符串（提示），以传递给语言模型。与其他方法一样，“部分”提示模板可能有意义——例如，传入所需值的子集，以创建仅期望剩余值子集的新提示模板。

LangChain以两种方式支持此功能：我们允许（1）带有字符串值的部分格式化的提示，（2）带有返回字符串值的函数的部分格式化提示。这两种不同的方式支持不同的用例。在下面的文档中，我们讨论了两种用例的动机以及如何在LangChain中进行操作。

使用字符串进行部分格式化

部分提示模板的一个常见用例是，如果您先获取某些变量而不是其他变量，那么您可能需要部分提示模板。例如，假设您有一个需要两个变量foo和baz的提示模板。如果您在链条的早期获取了foo值，但稍后才获取了baz值，那么等到在一个地方同时拥有两个变量才将它们传递给提示模板可能会很麻烦。相反，您可以使用foo值部分化提示模板，然后将部分化的提示模板传递下去，只需使用它即可。以下是执行此操作的示例：

```
from langchain.prompts import PromptTemplate

```

```
prompt = PromptTemplate(template="{foo}{bar}", input_variables=["foo", "bar"])
partial_prompt = prompt.partial(foo="foo");
print(partial_prompt.format(bar="baz"))

```

```
foobaz

```

You can also just initialize the prompt with the partialed variables.

```
prompt = PromptTemplate(template="{foo}{bar}", input_variables=["bar"], partial_variables={"foo": "foo"})
print(prompt.format(bar="baz"))

```

```
foobaz

```

Partial With Functions[#](#partial-with-functions "Permalink to this headline")
-------------------------------------------------------------------------------

The other common use is to partial with a function. The use case for this is when you have a variable you know that you always want to fetch in a common way. A prime example of this is with date or time. Imagine you have a prompt which you always want to have the current date. You can’t hard code it in the prompt, and passing it along with the other input variables is a bit annoying. In this case, it’s very handy to be able to partial the prompt with a function that always returns the current date.

```
from datetime import datetime

def _get_datetime():
    now = datetime.now()
    return now.strftime("%m/%d/%Y, %H:%M:%S")

```

```
prompt = PromptTemplate(
    template="Tell me a {adjective} joke about the day {date}", 
    input_variables=["adjective", "date"]
);
partial_prompt = prompt.partial(date=_get_datetime)
print(partial_prompt.format(adjective="funny"))

```

```
Tell me a funny joke about the day 02/27/2023, 22:15:16

```

You can also just initialize the prompt with the partialed variables, which often makes more sense in this workflow.

```
prompt = PromptTemplate(
    template="Tell me a {adjective} joke about the day {date}", 
    input_variables=["adjective"],
    partial_variables={"date": _get_datetime}
);
print(prompt.format(adjective="funny"))

```

```
Tell me a funny joke about the day 02/27/2023, 22:15:16

```

