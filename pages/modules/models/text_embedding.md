

文本嵌入模型[#](#text-embedding-models "Permalink to this headline")
==============================================================



注


[概念指南](https://docs.langchain.com/docs/components/models/text-embedding-model)



本文档介绍了如何在LangChain中使用Embedding类。


Embedding类是一个用于与嵌入进行交互的类。有许多嵌入提供商（OpenAI、Cohere、Hugging Face等）- 这个类旨在为所有这些提供商提供一个标准接口。


嵌入会创建文本的向量表示。这很有用，因为这意味着我们可以在向量空间中考虑文本，并执行诸如语义搜索之类的操作，其中我们在向量空间中寻找最相似的文本片段。


LangChain中的基本Embedding类公开了两种方法：embed\_documents和embed\_query。最大的区别在于这两种方法具有不同的接口：一个适用于多个文档，而另一个适用于单个文档。除此之外，将这两个方法作为两个单独的方法的另一个原因是，某些嵌入提供商针对要搜索的文档与查询本身具有不同的嵌入方法。


以下是文本嵌入的集成。



* [Aleph Alpha](text_embedding/examples/aleph_alpha.html)

* [AzureOpenAI](text_embedding/examples/azureopenai.html)

* [Cohere](text_embedding/examples/cohere.html)

* [Fake Embeddings](text_embedding/examples/fake.html)

* [Hugging Face Hub](text_embedding/examples/huggingfacehub.html)

* [InstructEmbeddings](text_embedding/examples/instruct_embeddings.html)

* [Jina](text_embedding/examples/jina.html)

* [Llama-cpp](text_embedding/examples/llamacpp.html)

* [OpenAI](text_embedding/examples/openai.html)

* [SageMaker Endpoint Embeddings](text_embedding/examples/sagemaker-endpoint.html)

* [Self Hosted Embeddings](text_embedding/examples/self-hosted.html)

* [Sentence Transformers Embeddings](text_embedding/examples/sentence_transformers.html)

* [TensorflowHub](text_embedding/examples/tensorflowhub.html)




