


 Graphsignal
 [#](#graphsignal "Permalink to this headline")
=============================================================



 This page covers how to use
 [Graphsignal](https://app.graphsignal.com) 
 to trace and monitor LangChain. Graphsignal enables full visibility into your application. It provides latency breakdowns by chains and tools, exceptions with full context, data monitoring, compute/GPU utilization, OpenAI cost analytics, and more.
 




 Installation and Setup
 [#](#installation-and-setup "Permalink to this headline")
-----------------------------------------------------------------------------------


* Install the Python library with
 `pip
 

 install
 

 graphsignal`
* Create free Graphsignal account
 [here](https://graphsignal.com)
* Get an API key and set it as an environment variable (
 `GRAPHSIGNAL_API_KEY`
 )





 Tracing and Monitoring
 [#](#tracing-and-monitoring "Permalink to this headline")
-----------------------------------------------------------------------------------



 Graphsignal automatically instruments and starts tracing and monitoring chains. Traces and metrics are then available in your
 [Graphsignal dashboards](https://app.graphsignal.com) 
 .
 



 Initialize the tracer by providing a deployment name:
 





```
import graphsignal

graphsignal.configure(deployment='my-langchain-app-prod')

```




 To additionally trace any function or code, you can use a decorator or a context manager:
 





```
@graphsignal.trace_function
def handle_request():    
    chain.run("some initial text")

```






```
with graphsignal.start_trace('my-chain'):
    chain.run("some initial text")

```




 Optionally, enable profiling to record function-level statistics for each trace.
 





```
with graphsignal.start_trace(
        'my-chain', options=graphsignal.TraceOptions(enable_profiling=True)):
    chain.run("some initial text")

```




 See the
 [Quick Start](https://graphsignal.com/docs/guides/quick-start/) 
 guide for complete setup instructions.
 





