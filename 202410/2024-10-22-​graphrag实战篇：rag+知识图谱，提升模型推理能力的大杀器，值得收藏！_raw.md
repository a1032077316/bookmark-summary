Title: ​GraphRag实战篇：RAG+知识图谱，提升模型推理能力的大杀器，值得收藏！

URL Source: https://mp.weixin.qq.com/s/Nz-_V9EcwRd-S6J9FbRHOw

Markdown Content:
Weixin Official Accounts Platform
===============

             

 

![Image 1: cover_image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/IS6X6xyoPF5VfIGW3pqvdxibq3gLHI4QE1BYBvrSuC7YrKysOJcv29uqJP4fxnXG1bW8CP1O226lH1pDwxKQ6cg/0?wx_fmt=jpeg)

​GraphRag实战篇：RAG+知识图谱，提升模型推理能力的大杀器，值得收藏！
========================================

Original 风叔 [风叔云](javascript:void(0);)

**前言**

经典RAG应用的范式与架构已经非常流行，我们可以在很短的时间内借助成熟框架开发一个简单能用的RAG应用。风叔在《[RAG实战篇：构建一个最小可行性的Rag系统](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457697071&idx=1&sn=0353a64bafc08ed6d8bd7bb300dbd7c8&chksm=8cdca4b0bbab2da6b16ce9dcc5cfd74c5dd1ae27e0993f5ae4dd5f5675bf4a405afa330921a0&scene=21#wechat_redirect)》中，即介绍了一个最最基本的Naive RAG系统，以及优化RAG系统的十八般武器。

但是传统的RAG系统有个非常大的局限，即不善于处理复杂关系推理、总结性问题和多跳问题。因为传统RAG需要对文本进行分块，然后进行向量化存储，这种处理模式就会天然导致RAG在全局查询或总结上的表现不会太好。比如面对这样的问题，“《跨越鸿沟》这本书整体上讲了什么？请撰写一份2000字的总结”，传统RAG大概率会表现不及格。

同样的，在对文本分块之后，可能会丢失一些信息之间的关系和因果性，因此在处理多跳问题时，会出现逻辑关系断裂的问题。比如面对这样的问题，“在华东区所有的门店中，哪个导购的消费者客单价最高”，传统RAG系统可能会给出错误的答复。

这正是知识图谱和GraphRAG可以发挥作用的场景。

**1\. 知识图谱与GraphRAG**

![Image 2](https://mmbiz.qpic.cn/sz_mmbiz_png/IS6X6xyoPF6icyBbe5PP7Z34RdJu9DQq6ico2x0DdG5jWHrgK7O1pyqHgbMmXOS9YNb6fvliczTibrLicPOvY0MkjpQ/640?wx_fmt=png&from=appmsg)

知识图谱是一种将将信息表示为实体（节点）和关系（边）的网络，模仿了人类结构知识的组成方式。知识图谱不仅能捕获原始信息，还能捕获跨越多个文档的高阶关系，并具备强大的推理能力。

知识图谱的信息存储可以使用图数据库，图数据库是一种专门用于存储和操作图结构数据的数据库管理系统，如下图所示。与关系型数据库不同，图数据库使用节点、边和属性来表示和存储数据，非常适合处理高度连接的数据，提供高性能的复杂查询能力，用来遍历与发现有洞察力的数据关系。其最大特点是：

*   灵活的模型：可以方便地表示复杂的关系。
    
*   高效的查询：特别是多跳关系的查询，比关系数据库更高效。
    
*   可扩展性：能够处理大量节点和边。
    

![Image 3](https://mmbiz.qpic.cn/sz_mmbiz_png/IS6X6xyoPF6icyBbe5PP7Z34RdJu9DQq6iaiadcicvrbHvDnWSDqHuWYMMSXJnjlp3icKU7qWVaicWV1hrFKic1Hobx1Q/640?wx_fmt=png&from=appmsg)

目前流行的图数据库包括Neo4j，OrientDB、TigerGraph等等。

我们接着来说GraphRag，它是一种将知识图谱与Rag相结合的技术范式。传统Rag是对向量数据库进行检索，而GraphRag则对存储在图数据库中的知识图谱（而非存储在向量数据库中的知识向量）进行检索，获得关联知识并实现增强生成的。

GraphRAG在整体架构与传统RAG非常相似，也是需要先建立索引，然后再通过检索召回，生成最终输出。区别在于，GraphRAG采用了图数据库进行索引构建和检索召回。

![Image 4](https://mmbiz.qpic.cn/sz_mmbiz_png/IS6X6xyoPF6icyBbe5PP7Z34RdJu9DQq6ic0rJqJ6ObIqwic4pMHocRV6ruO5KBIrKRPl22KxlmMY8HLcYHNWu0ww/640?wx_fmt=png&from=appmsg)

基于图数据库进行知识图谱检索的时候，有两种常见方式：

第一种是借助Text-to-GraphQL，即将自然语言输入转换为图数据库的查询语言，比如Neo4j的Cypher语言，再使用图数据库的查询语言从知识图谱中检索出需要的知识，这是一种精确的检索方式。

第二种是借助Vector索引，即在构建的Graph基础上，对其中的节点与关系创建向量索引，并通过向量相似性来检索出相关的节点和关系信息，还可以结合传统的关键词做混合检索，这种检索方式的精确度不如上一种。

![Image 5](https://mmbiz.qpic.cn/sz_mmbiz_png/IS6X6xyoPF6icyBbe5PP7Z34RdJu9DQq6PJsAC2l8BdXPMeA8Bw4sYKRkRrtgS6mqgVibY5EAcqO2tMfEW5qoQmA/640?wx_fmt=png&from=appmsg)

**2\. GraphRAG适合的应用场景**

下面我们来分析一下GraphRAG适合的场景，通常来说，具备如下特征的数据和场景更适合使用GraphRAG。

第一类是有较多相互关联实体与复杂关系，且结构较明确的数据。比如：

*   人物关系网络数据：社交网络中的用户关系、历史人物关系、家族图谱等。
    
*   企业级关系数据：公司结构、供应链、客户等之间的关系。
    
*   医学类数据：疾病、症状、治疗、药物、传播、病例等之间复杂关系。
    
*   法律法规数据：法律条款之间的引用关系、解释、判例与适用法律条款的关系。
    
*   推荐系统数据：产品、用户、浏览内容、产品之间的关联、用户之间的关系等。
    

第二类是涉及复杂关系、语义推理和多步逻辑关联的查询，比如：

*   多跳关系查询：在华东区所有的门店中，哪个导购的消费者客单价最高？
    
*   知识推理查询：根据患者的症状和病史，推断可能的疾病并提供治疗方案。
    
*   聚合统计查询：在《三国演义》中，出场次数最多的人是谁？
    
*   时序关联查询：过去一年都有哪些AI大模型的投资与并购事件？
    
*   跨多文档查询：在《三体3》中，有哪些人物在《三体1》中出现？
    

基于GraphRAG的特点，我们可以提炼出一些GraphRAG适合的应用场景

*   **智能客服**：将产品手册或说明文档映射到知识图谱中，解答消费者各式各样的售后问题，不需要提前准备QA对，也不需要提前对问题进行扩写，GraphRAG可以在精确理解用户意图的基础上进行查找。
    
*   **智能检修**：通常电气设备的产品说明短则几百页，长则数千页，即便是最资深的运维工程师也很难完全消化。因此，可以将设备的故障排除指南映射到知识图谱中，当设备出现故障时，方便工程师快速排查和解决问题。
    
*   **智能问诊**：在医学领域，病人的症状和病因之间存在着非常复杂的因果关系，同时医学领域对精准度的要求也非常高，因此非常适合GraphRag的使用，比如智能问诊
    
*   **药物合成：**在医药领域，存在非常多的分子化合物，每种分子化合物的特性都相差迥异，不同的分子还能结合成新的分子化合物，其中的排列组合不胜枚举。因此，可以将各种已经发现的分子化合物的结构、特征映射到知识图谱中，帮助生物学家更高效地发现有益的大分子结构。
    
*   **报告总结**：在金融领域，行业分析师每天都要阅读海量的市场信息或研究报告，过去都得完全依靠人工提炼和总结。而通过GraphRAG，构建智能投研、智能投顾等系统，帮助研究员快速提炼分析报告的核心内容。
    

  

GraphRAG虽然强大，但是相比传统RAG，其成本要高出不少。因此在以下常规场景下，我们还是尽量使用传统RAG。

首先，当大多数查询集中在单个实体上时，传统的 RAG 就够用了，因为相关的上下文很可能包含在以实体为中心的文档中，构建知识图谱会额外增加大量成本。其次，对于小型且垂直的语料库（几百个文档），仅对文档本身进行索引可能已经足够，即使存在一些文档间的关系。第三，如果主要目标是理解主题和叙事，比如社交媒体的负面评价分析，重点更多地放在语言上而不是关系上，使用传统Rag也绰绰有余。

下面，我们结合实际的源代码，介绍一下GraphRAG的实战技巧。风叔选了两个场景：一个是利用pdf、word等非结构化数据，实现内容的推理总结；另一个是利用MySQL结构化数据，实现精确查询。

**3\. GraphRAG实战之推理总结**

构建一个非结构化数据的GraphRAG应用，首要任务是把非结构化数据转换成以图结构表示的知识图谱，并存储到GraphDB如Neo4j，用来提供后续检索与生成的基础。

从非结构化文本到知识图谱，借助LLM是一种常见且高效的方法。即利用LLM强大的语义理解与推理能力，从非结构化文本中抽取大量的类似实体-关系-实体的三元组，并借助必要的接口（如GraphDB支持的查询语言）导入到GraphDB中创建对应的实体、关系与属性，形成知识图谱。

**第一步，将非结构化数据存储进GraphDB**

这里的核心是基于LLM而实现的Extractor，即Graph结构的抽取组件。在不同的框架中有不同的组件实现，这里我们以LlamaIndex框架为例，其实现的核心组件为LLMPathExtractor，一般用最简单的SimpleLLMPathExtractor进行代码实现即可。

```
llm = OpenAI(model="gpt-4o")
```

这里需要注意的是：

*   抽取过程最重要的工具是LLM与对应的提示词，这里用了少量示例提示模式（few-shot prompt），大家可以根据需要做优化
    
*   借助于PropertyGraphIndex组件，可以快速的基于文档与抽取器生成知识图谱并存储到图数据库中
    
*   在生成知识图谱时，需要指定嵌入模型，这是为了在生成Graph的节点时，对节点的内容或者名称生成向量，用于后续的向量检索
    

运行这段代码后，原始的文本将被分割成多个chunk，并用来创建label为"chunk"的多个知识图谱节点，这个节点的text属性用来存放原始的文本，同时embedding属性用来存放生成向量。

**第二步，基于知识图谱的检索与生成**

这里先创建一个基于关键词的检索器，注意这里的prompt，是用来抽取输入中的关键词。检索器的参数通常包括使用的大模型、提取关键词的提示词、最大提取的关键词数量、检索的路径深度等。

```
def parse_fn(output: str) -> list[str]:
```

再创建一个向量的检索器，注意向量检索器需要指定的是嵌入模型而非大模型，因此也无需指定提示词参数：

```
vector_retriever = VectorContextRetriever(
```

在LlamaIndex中，可以将创建的两种类型检索器作为子检索器进行融合检索，从而形成更加丰富的上下文。借助PGRetriever这个组件即可实现：

```
retriever = PGRetriever(sub_retrievers=[synonym_retriever,vector_retriever])
```

可以直接基于上述检索器创建查询引擎，即可用来生成查询响应：

```
...
```

以上就是使用GraphRAG，对非结构化文本进行推理总结的过程

**4\. GraphRAG实战之精确查询**

下面再介绍一个利用GraphRAG实现精确查询的例子。虽然在《[RAG实战篇：将用户输入转换为精确的数据库查询语言](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457697079&idx=1&sn=7dc8205589e2de89772a046090a15c00&chksm=8cdca4a8bbab2dbef88270ac1ba339d0b40d4db5b840d989256137ce7beb89cf028b4ce2de2c&scene=21#wechat_redirect)》一文中，风叔介绍了一些text-to-SQL的方法，但是其准确度仍然存在较大不稳定性，将关系型数据库转换成图数据库是一种有效的优化方案。

**第一步，原始MySQL数据准备**

首先创建几个简单的表，可以借助工具生成必要的测试数据：

*   orders：订单表，包含客户id，产品id，数量，销售员，订单日期等。
    
*   custoemrs：客户信息表，包含客户id，姓名，电话，电子邮件，城市等。
    
*   products：产品信息表，包含产品ID，名称，单价等。
    
*   salespersons：销售员信息表，包含人员id，姓名，电话，所属部门等。
    
*   departs：部门信息表，包含部门id，部门名称等。
    

**第二步，将MySQL数据映射到Neo4j图数据库**

首先，我们将表数据读取到本地pandas的dataframe

```
import pandas as pd
```

然后，创建Graph的节点，可以使用Cypher的CREATE语句来批量创建节点

```
def create_unique_nodes_from_dataframe(df, label, unique_id_property):
```

最后，我们还要创建Graph的节点关系。这里的Cypher语句其实和SQL很类似。注意创建关系时我们使用的是MERGE，这是为了防止重复生成关系。

```
def create_relationships():
```

运行以上代码，如果一切正常，将会在Neo4j数据库中看到我们创建的所有节点和关系信息。

**第三步，实现GraphRag精确查询**

我们可以借助LangChain中的**GraphCypherQAChain**组件来快速实现对Graph的检索与答案生成，这个组件的基本原理就是把输入的自然语言转换成Cypher语句，然后获得查询结果。

```
from langchain.prompts import PromptTemplate
```

在控制台运行可以观察到输出提示与结果，可以看到生成的完整Cypher语句和运行结果，以及最后LLM的生成答案。

通过这个例子，大家知晓了如何通过GraphRag提升精确查询能力，如前文所述，另一种提升精确查询能力的方案是text-to-SQL。根据实际测试情况发现，在关系比较简单的场景下，两者在功能上并没有太大的区别，大部分任务都可以完成。但是如果涉及较复杂的多跳查询，且在数据量较大（如100万以上）时，基于GraphRAG的检索性能会更好。

**总结**

整体来说，GraphRAG 擅长处理复杂、互联的数据集以及需要深度关系理解的查询。特别是在需要多层次分析和推理的情况下，GraphRAG能够显著提升信息检索的精度和深度。

然而，这种能力的提升也必然伴随着更高的系统复杂性和资源消耗。因此，在决定是否采用 GraphRAG 之前，必须仔细分析具体的应用场景、数据结构以及典型的查询模式。在简单和事实性查询、较小数据集以及简单应用场景下，传统 RAG 仍然是更理想的选择。

GraphRAG是一个非常热门的领域，很多公司都在研究如何利用图数据库，进一步提升RAG的推理和总结能力。今年，微软开源了自主研发的Microsoft GraghRAG，犹如一枚重磅炸弹，给市场带来了非常大的反响。

下一篇文章，风叔将详细介绍Microsoft GraghRAG的原理和实战案例。

**更多精彩文章：**

[大佬们都在关注的AI Agent，到底是什么？用5W1H分析框架拆解AI Agent（上篇）](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457696696&idx=1&sn=91f17dc44eaf36bc4bfaef7500d6ccf9&chksm=8cdca627bbab2f31cbc34c76ca364768b1caf6baa64c36d297dbb2b9533a3f29f0b18e670bcf&scene=21#wechat_redirect)

[大佬们都在关注的AI Agent，到底是什么？用5W1H分析框架拆解AI Agent（中篇）](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457696709&idx=1&sn=5af1b4f842c58996d0b6fffec80e30f1&chksm=8cdca65abbab2f4cd6187bd1f1ef92bc67f404628718c7a81d5c151a1f01d26a6fb2713e2fee&scene=21#wechat_redirect)

[大佬们都在关注的AI Agent，到底是什么？用5W1H分析框架拆解AI Agent（下篇）](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457696711&idx=1&sn=e25db3dc710a8f94fde1fabc612e4d69&chksm=8cdca658bbab2f4e93b42c756bd97962899b7252cb2eeef5fc1193aafd083aa97a5011e41590&scene=21#wechat_redirect)

[AI大模型实战篇：AI Agent设计模式 - ReAct](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457696846&idx=1&sn=ad0fb24ca436121f9e36df9eba53edd3&chksm=8cdca5d1bbab2cc7fc5af83725d8dc940123e22bc244eab8ffdba693b7c1accad89d40e51eb5&scene=21#wechat_redirect)

[RAG实战篇：构建一个最小可行性的Rag系统](http://mp.weixin.qq.com/s?__biz=MzAxMDEwMzUzNg==&mid=2457697071&idx=1&sn=0353a64bafc08ed6d8bd7bb300dbd7c8&chksm=8cdca4b0bbab2da6b16ce9dcc5cfd74c5dd1ae27e0993f5ae4dd5f5675bf4a405afa330921a0&scene=21#wechat_redirect)

![Image 6](https://mmbiz.qpic.cn/sz_mmbiz_png/IS6X6xyoPF4L1yZzcgCTqLJGSggloLr2X70KTmSznVfzX3UaHEqEM32UkibBoQUibhEetco26FPu3WIXiaM1qjGDA/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp)

风叔，某互联网大厂产品总监，十多年产品设计和商业化经验，对电商、营销、AI和大数据产品具备丰富的实战经验。风叔将坚持输出自己的总结和思考，希望对你有帮助。

预览时标签不可点

个人观点，仅供参考

![Image 7](https://mp.weixin.qq.com/s/Nz-_V9EcwRd-S6J9FbRHOw)Scan to Follow

继续滑动看下一个

轻触阅读原文

![Image 8](http://mmbiz.qpic.cn/sz_mmbiz_png/IS6X6xyoPF4AgeTC5570iaj95Jc4Cz0pUum6ibthiaXczqHX2D7woCASOXz0fgvmicMGGPtu03eh5UqeLXlYmWoUBg/0?wx_fmt=png)

风叔云

向上滑动看下一个

[Got It](javascript:;)

 

![Image 9](https://mp.weixin.qq.com/s/Nz-_V9EcwRd-S6J9FbRHOw) Scan with Weixin to  
use this Mini Program

[Cancel](javascript:void(0);) [Allow](javascript:void(0);)

[Cancel](javascript:void(0);) [Allow](javascript:void(0);)

× 分析

 : ， .   Video Mini Program Like ，轻点两下取消赞 Wow ，轻点两下取消在看 Share Comment Favorite
