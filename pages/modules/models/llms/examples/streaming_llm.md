

如何流式传输LLM和聊天模型响应[#](#how-to-stream-llm-and-chat-model-responses "标题的永久链接")
==========================================================================

LangChain为LLM提供流式传输支持。目前，我们支持 `OpenAI`，`ChatOpenAI` 和 `Anthropic` 实现的流式传输，但其他LLM实现的流式传输正在路线图中。要使用流式传输，请使用实现 `on_llm_new_token` 的 [`CallbackHandler`](https://github.com/hwchase17/langchain/blob/master/langchain/callbacks/base.py)。在这个例子中，我们使用的是 `StreamingStdOutCallbackHandler`。

```
from langchain.llms import OpenAI, Anthropic
from langchain.chat_models import ChatOpenAI
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler
from langchain.schema import HumanMessage

```

```
llm = OpenAI(streaming=True, callbacks=[StreamingStdOutCallbackHandler()], temperature=0)
resp = llm("Write me a song about sparkling water.")

```

```
Verse 1
I'm sippin' on sparkling water,
It's so refreshing and light,
It's the perfect way to quench my thirst
On a hot summer night.

Chorus
Sparkling water, sparkling water,
It's the best way to stay hydrated,
It's so crisp and so clean,
It's the perfect way to stay refreshed.

Verse 2
I'm sippin' on sparkling water,
It's so bubbly and bright,
It's the perfect way to cool me down
On a hot summer night.

Chorus
Sparkling water, sparkling water,
It's the best way to stay hydrated,
It's so crisp and so clean,
It's the perfect way to stay refreshed.

Verse 3
I'm sippin' on sparkling water,
It's so light and so clear,
It's the perfect way to keep me cool
On a hot summer night.

Chorus
Sparkling water, sparkling water,
It's the best way to stay hydrated,
It's so crisp and so clean,
It's the perfect way to stay refreshed.

```

如果使用 `generate`，仍然可以访问最终的 `LLMResult`。然而，目前不支持使用流式传输的 `token_usage`。

```
llm.generate(["Tell me a joke."])

```

```
Q: What did the fish say when it hit the wall?
A: Dam!

```

```
LLMResult(generations=[[Generation(text='  Q: What did the fish say when it hit the wall?\nA: Dam!', generation_info={'finish_reason': None, 'logprobs': None})]], llm_output={'token_usage': {}, 'model_name': 'text-davinci-003'})

```

这里是一个使用 `ChatOpenAI` 聊天模型实现的示例：

```
chat = ChatOpenAI(streaming=True, callbacks=[StreamingStdOutCallbackHandler()], temperature=0)
resp = chat([HumanMessage(content="Write me a song about sparkling water.")])

```

```
Verse 1:
Bubbles rising to the top
A refreshing drink that never stops
Clear and crisp, it's oh so pure
Sparkling water, I can't ignore

Chorus:
Sparkling water, oh how you shine
A taste so clean, it's simply divine
You quench my thirst, you make me feel alive
Sparkling water, you're my favorite vibe

Verse 2:
No sugar, no calories, just H2O
A drink that's good for me, don't you know
With lemon or lime, you're even better
Sparkling water, you're my forever

Chorus:
Sparkling water, oh how you shine
A taste so clean, it's simply divine
You quench my thirst, you make me feel alive
Sparkling water, you're my favorite vibe

Bridge:
You're my go-to drink, day or night
You make me feel so light
I'll never give you up, you're my true love
Sparkling water, you're sent from above

Chorus:
Sparkling water, oh how you shine
A taste so clean, it's simply divine
You quench my thirst, you make me feel alive
Sparkling water, you're my favorite vibe

Outro:
Sparkling water, you're the one for me
I'll never let you go, can't you see
You're my drink of choice, forevermore
Sparkling water, I adore.

```

这里是一个使用 `Anthropic` LLM 实现的示例，它使用了他们的 `claude` 模型。

```
llm = Anthropic(streaming=True, callbacks=[StreamingStdOutCallbackHandler()], temperature=0)
llm("Write me a song about sparkling water.")

```

```
Sparkling water, bubbles so bright,

Fizzing and popping in the light.

No sugar or calories, a healthy delight,

Sparkling water, refreshing and light.

Carbonation that tickles the tongue,

In flavors of lemon and lime unsung.

Sparkling water, a drink quite all right,

Bubbles sparkling in the light.

```

```
'\nSparkling water, bubbles so bright,  Fizzing and popping in the light.  No sugar or calories, a healthy delight,  Sparkling water, refreshing and light.  Carbonation that tickles the tongue,  In flavors of lemon and lime unsung.  Sparkling water, a drink quite all right,  Bubbles sparkling in the light.'

```

