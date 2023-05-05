
PromptLayer
==================


本示例演示了如何连接到[PromptLayer](https://www.promptlayer.com)，以开始记录您的ChatOpenAI请求。

安装PromptLayer[#](#安装promptlayer "此标题的永久链接")
-------------------------------------------------------------------------

使用pip安装需要使用`promptlayer`包来使用PromptLayer与OpenAI。

```
pip install promptlayer

```

导入[#](#导入 "此标题的永久链接")
-------------------------------------------------

```
import os
from langchain.chat_models import PromptLayerChatOpenAI
from langchain.schema import HumanMessage

```

设置环境API密钥[#](#设置环境api密钥 "此标题的永久链接")
-----------------------------------------------------------------------------------------

您可以在[PromptLayer](https://www.promptlayer.com)上通过单击导航栏中的设置齿轮来创建`PromptLayer API Key`。

将其设置为名为`PROMPTLAYER_API_KEY`的环境变量。

```
os.environ["PROMPTLAYER_API_KEY"] = "**********"

```

像平常一样使用PromptLayer OpenAI LLM[#](#use-the-promptlayeropenai-llm-like-normal "永久链接至此标题")
---------------------------------------------------------------------------------------

*您可以选择传递`pl_tags`来使用PromptLayer的标记功能跟踪您的请求。*

```
chat = PromptLayerChatOpenAI(pl_tags=["langchain"])
chat([HumanMessage(content="I am a cat and I want")])

```

```
AIMessage(content='to take a nap in a cozy spot. I search around for a suitable place and finally settle on a soft cushion on the window sill. I curl up into a ball and close my eyes, relishing the warmth of the sun on my fur. As I drift off to sleep, I can hear the birds chirping outside and feel the gentle breeze blowing through the window. This is the life of a contented cat.', additional_kwargs={})

```

**上述请求现在应该出现在您的[PromptLayer仪表板](https://www.promptlayer.com)上。**

使用PromptLayer跟踪[#](#using-promptlayer-track "永久链接至此标题")
-------------------------------------------------------

如果您想使用任何[PromptLayer跟踪功能](https://magniv.notion.site/Track-4deee1b1f7a34c1680d085f82567dab9)，必须在实例化PromptLayer LLM时传递`return_pl_id`参数以获取请求ID。

```
chat = PromptLayerChatOpenAI(return_pl_id=True)
chat_results = chat.generate([[HumanMessage(content="I am a cat and I want")]])

for res in chat_results.generations:
    pl_request_id = res[0].generation_info["pl_request_id"]
    promptlayer.track.score(request_id=pl_request_id, score=100)

```

这使您能够在PromptLayer仪表板中跟踪模型的性能。如果您正在使用提示模板，您还可以将模板附加到请求上。总体而言，这使您有机会在PromptLayer仪表板中跟踪不同模板和模型的性能。

