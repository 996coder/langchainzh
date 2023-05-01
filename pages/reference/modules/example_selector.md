




 Example Selector
 [#](#module-langchain.prompts.example_selector "Permalink to this headline")
================================================================================================



 Logic for selecting examples to include in prompts.
 




*pydantic
 

 model*


 langchain.prompts.example_selector.
 



 LengthBasedExampleSelector
 

[[source]](../../_modules/langchain/prompts/example_selector/length_based#LengthBasedExampleSelector)
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector "Permalink to this definition") 



 Select examples based on length.
 




 Validators
 

* `calculate_example_text_lengths`
 »
 `example_text_lengths`






*field*


 example_prompt
 

*:
 



[langchain.prompts.prompt.PromptTemplate](prompts#langchain.prompts.PromptTemplate "langchain.prompts.prompt.PromptTemplate")*
*[Required]*
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector.example_prompt "Permalink to this definition") 



 Prompt template used to format the examples.
 






*field*


 examples
 

*:
 




 List
 


 [
 


 dict
 


 ]*
*[Required]*
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector.examples "Permalink to this definition") 



 A list of the examples that the prompt template expects.
 






*field*


 get_text_length
 

*:
 




 Callable
 


 [
 



 [
 


 str
 


 ]
 



 ,
 




 int
 


 ]*
*=
 




 <function
 

 _get_length_based>*
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector.get_text_length "Permalink to this definition") 



 Function to measure prompt length. Defaults to word count.
 






*field*


 max_length
 

*:
 




 int*
*=
 




 2048*
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector.max_length "Permalink to this definition") 



 Max length for the prompt, beyond which examples are cut.
 








 add_example
 


 (
 
*example
 



 :
 





 Dict
 


 [
 


 str
 


 ,
 




 str
 


 ]*

 )
 


 →
 


 None
 


[[source]](../../_modules/langchain/prompts/example_selector/length_based#LengthBasedExampleSelector.add_example)
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector.add_example "Permalink to this definition") 



 Add new example to list.
 








 select_examples
 


 (
 
*input_variables
 



 :
 





 Dict
 


 [
 


 str
 


 ,
 




 str
 


 ]*

 )
 


 →
 


 List
 


 [
 


 dict
 


 ]
 



[[source]](../../_modules/langchain/prompts/example_selector/length_based#LengthBasedExampleSelector.select_examples)
[#](#langchain.prompts.example_selector.LengthBasedExampleSelector.select_examples "Permalink to this definition") 



 Select which examples to use based on the input lengths.
 








*pydantic
 

 model*


 langchain.prompts.example_selector.
 



 MaxMarginalRelevanceExampleSelector
 

[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#MaxMarginalRelevanceExampleSelector)
[#](#langchain.prompts.example_selector.MaxMarginalRelevanceExampleSelector "Permalink to this definition") 



 ExampleSelector that selects examples based on Max Marginal Relevance.
 



 This was shown to improve performance in this paper:
 <https://arxiv.org/pdf/2211.13892.pdf>





*field*


 fetch_k
 

*:
 




 int*
*=
 




 20*
[#](#langchain.prompts.example_selector.MaxMarginalRelevanceExampleSelector.fetch_k "Permalink to this definition") 



 Number of examples to fetch to rerank.
 






*classmethod*


 from_examples
 


 (
 
*examples
 



 :
 





 List
 


 [
 


 dict
 


 ]*
 ,
 *embeddings
 



 :
 





 langchain.embeddings.base.Embeddings*
 ,
 *vectorstore_cls
 



 :
 





 Type
 


 [
 

[langchain.vectorstores.base.VectorStore](vectorstores#langchain.vectorstores.VectorStore "langchain.vectorstores.base.VectorStore")


 ]*
 ,
 *k
 



 :
 





 int
 





 =
 





 4*
 ,
 *input_keys
 



 :
 





 Optional
 


 [
 


 List
 


 [
 


 str
 


 ]
 



 ]
 






 =
 





 None*
 ,
 *fetch_k
 



 :
 





 int
 





 =
 





 20*
 ,
 *\*\*
 



 vectorstore_cls_kwargs
 



 :
 





 Any*

 )
 


 →
 

[langchain.prompts.example_selector.semantic_similarity.MaxMarginalRelevanceExampleSelector](#langchain.prompts.example_selector.MaxMarginalRelevanceExampleSelector "langchain.prompts.example_selector.semantic_similarity.MaxMarginalRelevanceExampleSelector")


[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#MaxMarginalRelevanceExampleSelector.from_examples)
[#](#langchain.prompts.example_selector.MaxMarginalRelevanceExampleSelector.from_examples "Permalink to this definition") 



 Create k-shot example selector using example list and embeddings.
 



 Reshuffles examples dynamically based on query similarity.
 




 Parameters
 

* **examples** 
 – List of examples to use in the prompt.
* **embeddings** 
 – An iniialized embedding API interface, e.g. OpenAIEmbeddings().
* **vectorstore_cls** 
 – A vector store DB interface class, e.g. FAISS.
* **k** 
 – Number of examples to select
* **input_keys** 
 – If provided, the search is based on the input variables
instead of all variables.
* **vectorstore_cls_kwargs** 
 – optional kwargs containing url for vector store




 Returns
 


 The ExampleSelector instantiated, backed by a vector store.
 










 select_examples
 


 (
 
*input_variables
 



 :
 





 Dict
 


 [
 


 str
 


 ,
 




 str
 


 ]*

 )
 


 →
 


 List
 


 [
 


 dict
 


 ]
 



[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#MaxMarginalRelevanceExampleSelector.select_examples)
[#](#langchain.prompts.example_selector.MaxMarginalRelevanceExampleSelector.select_examples "Permalink to this definition") 



 Select which examples to use based on semantic similarity.
 








*pydantic
 

 model*


 langchain.prompts.example_selector.
 



 SemanticSimilarityExampleSelector
 

[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#SemanticSimilarityExampleSelector)
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector "Permalink to this definition") 



 Example selector that selects examples based on SemanticSimilarity.
 




*field*


 example_keys
 

*:
 




 Optional
 


 [
 


 List
 


 [
 


 str
 


 ]
 



 ]*
*=
 




 None*
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.example_keys "Permalink to this definition") 



 Optional keys to filter examples to.
 






*field*


 input_keys
 

*:
 




 Optional
 


 [
 


 List
 


 [
 


 str
 


 ]
 



 ]*
*=
 




 None*
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.input_keys "Permalink to this definition") 



 Optional keys to filter input to. If provided, the search is based on
the input variables instead of all variables.
 






*field*


 k
 

*:
 




 int*
*=
 




 4*
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.k "Permalink to this definition") 



 Number of examples to select.
 






*field*


 vectorstore
 

*:
 



[langchain.vectorstores.base.VectorStore](vectorstores#langchain.vectorstores.VectorStore "langchain.vectorstores.base.VectorStore")*
*[Required]*
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.vectorstore "Permalink to this definition") 



 VectorStore than contains information about examples.
 








 add_example
 


 (
 
*example
 



 :
 





 Dict
 


 [
 


 str
 


 ,
 




 str
 


 ]*

 )
 


 →
 


 str
 


[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#SemanticSimilarityExampleSelector.add_example)
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.add_example "Permalink to this definition") 



 Add new example to vectorstore.
 






*classmethod*


 from_examples
 


 (
 
*examples
 



 :
 





 List
 


 [
 


 dict
 


 ]*
 ,
 *embeddings
 



 :
 





 langchain.embeddings.base.Embeddings*
 ,
 *vectorstore_cls
 



 :
 





 Type
 


 [
 

[langchain.vectorstores.base.VectorStore](vectorstores#langchain.vectorstores.VectorStore "langchain.vectorstores.base.VectorStore")


 ]*
 ,
 *k
 



 :
 





 int
 





 =
 





 4*
 ,
 *input_keys
 



 :
 





 Optional
 


 [
 


 List
 


 [
 


 str
 


 ]
 



 ]
 






 =
 





 None*
 ,
 *\*\*
 



 vectorstore_cls_kwargs
 



 :
 





 Any*

 )
 


 →
 

[langchain.prompts.example_selector.semantic_similarity.SemanticSimilarityExampleSelector](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector "langchain.prompts.example_selector.semantic_similarity.SemanticSimilarityExampleSelector")


[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#SemanticSimilarityExampleSelector.from_examples)
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.from_examples "Permalink to this definition") 



 Create k-shot example selector using example list and embeddings.
 



 Reshuffles examples dynamically based on query similarity.
 




 Parameters
 

* **examples** 
 – List of examples to use in the prompt.
* **embeddings** 
 – An initialized embedding API interface, e.g. OpenAIEmbeddings().
* **vectorstore_cls** 
 – A vector store DB interface class, e.g. FAISS.
* **k** 
 – Number of examples to select
* **input_keys** 
 – If provided, the search is based on the input variables
instead of all variables.
* **vectorstore_cls_kwargs** 
 – optional kwargs containing url for vector store




 Returns
 


 The ExampleSelector instantiated, backed by a vector store.
 










 select_examples
 


 (
 
*input_variables
 



 :
 





 Dict
 


 [
 


 str
 


 ,
 




 str
 


 ]*

 )
 


 →
 


 List
 


 [
 


 dict
 


 ]
 



[[source]](../../_modules/langchain/prompts/example_selector/semantic_similarity#SemanticSimilarityExampleSelector.select_examples)
[#](#langchain.prompts.example_selector.SemanticSimilarityExampleSelector.select_examples "Permalink to this definition") 



 Select which examples to use based on semantic similarity.
 







