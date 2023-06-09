SageMaker Endpoints Embeddings类
======

让我们加载SageMaker Endpoints Embeddings类。 如果您在SageMaker上托管自己的Hugging Face模型，可以使用此类。

有关如何执行此操作的说明，请单击[此处](https://www.philschmid.de/custom-inference-huggingface-sagemaker)。 **注意**：为了处理批处理请求，您需要在自定义的`inference.py`脚本中的`predict_fn（）`函数的返回行中进行调整：

将

`return {"vectors": sentence_embeddings[0].tolist()}`

更改为：

`return {"vectors": sentence_embeddings.tolist()}`。

```
!pip3 install langchain boto3

```

```
from typing import Dict, List
from langchain.embeddings import SagemakerEndpointEmbeddings
from langchain.llms.sagemaker_endpoint import ContentHandlerBase
import json

class ContentHandler(ContentHandlerBase):
    content_type = "application/json"
    accepts = "application/json"

    def transform_input(self, inputs: list[str], model_kwargs: Dict) -> bytes:
        input_str = json.dumps({"inputs": inputs, **model_kwargs})
        return input_str.encode('utf-8')

    def transform_output(self, output: bytes) -> List[List[float]]:
        response_json = json.loads(output.read().decode("utf-8"))
        return response_json["vectors"]

content_handler = ContentHandler()

embeddings = SagemakerEndpointEmbeddings(
    # endpoint_name="endpoint-name", 
    # credentials_profile_name="credentials-profile-name", 
    endpoint_name="huggingface-pytorch-inference-2023-03-21-16-14-03-834", 
    region_name="us-east-1", 
    content_handler=content_handler
)

```

```
query_result = embeddings.embed_query("foo")

```

```
doc_results = embeddings.embed_documents(["foo"])

```

```
doc_results

```

