自定义提示模板
====================


假设我们希望LLM根据函数的名称生成英语语言的解释。为了完成这个任务，我们将创建一个自定义提示模板，它以函数名称作为输入，并格式化提示模板以提供函数的源代码。

为什么需要自定义提示模板？

LangChain提供了一组默认提示模板，可用于生成各种任务的提示。但是，可能存在默认提示模板不符合您需求的情况。例如，您可能希望创建具有特定动态说明的提示模板以供语言模型使用。在这种情况下，可以创建自定义提示模板。

查看当前默认提示模板集合[此处](../getting_started.html)。

创建自定义提示模板

基本上有两种不同的提示模板可用——字符串提示模板和聊天提示模板。字符串提示模板以字符串格式提供简单提示，而聊天提示模板生成可用于聊天API的更结构化的提示。

在本指南中，我们将使用字符串提示模板创建自定义提示。

要创建自定义字符串提示模板，需要两个要求：

1. 它具有input_variables属性，以公开提示模板期望的输入变量。
2. 它公开format方法，该方法接受与预期的input_variables对应的关键字参数，并返回格式化的提示。

我们将创建一个自定义提示模板，它以函数名称作为输入，并格式化提示来提供函数的源代码。为此，让我们首先创建一个函数，该函数将根据其名称返回函数的源代码。

```
import inspect

def get_source_code(function_name):
    # Get the source code of the function
    return inspect.getsource(function_name)

```

Next, we’ll create a custom prompt template that takes in the function name as input, and formats the prompt template to provide the source code of the function.

```
from langchain.prompts import StringPromptTemplate
from pydantic import BaseModel, validator

class FunctionExplainerPromptTemplate(StringPromptTemplate, BaseModel):
 """ A custom prompt template that takes in the function name as input, and formats the prompt template to provide the source code of the function. """

    @validator("input_variables")
    def validate_input_variables(cls, v):
 """ Validate that the input variables are correct. """
        if len(v) != 1 or "function_name" not in v:
            raise ValueError("function_name must be the only input_variable.")
        return v

    def format(self, **kwargs) -> str:
        # Get the source code of the function
        source_code = get_source_code(kwargs["function_name"])

        # Generate the prompt to be sent to the language model
        prompt = f"""
 Given the function name and source code, generate an English language explanation of the function.
 Function Name: {kwargs["function_name"].__name__}
 Source Code:
 {source_code}
 Explanation:
 """
        return prompt

    def _prompt_type(self):
        return "function-explainer"

```

Use the custom prompt template[#](#use-the-custom-prompt-template "Permalink to this headline")
-----------------------------------------------------------------------------------------------

Now that we have created a custom prompt template, we can use it to generate prompts for our task.

```
fn_explainer = FunctionExplainerPromptTemplate(input_variables=["function_name"])

# Generate a prompt for the function "get_source_code"
prompt = fn_explainer.format(function_name=get_source_code)
print(prompt)

```

```
        Given the function name and source code, generate an English language explanation of the function.
        Function Name: get_source_code
        Source Code:
        def get_source_code(function_name):
    # Get the source code of the function
    return inspect.getsource(function_name)

        Explanation:

```

