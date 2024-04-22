[[RAG 项目]]
# 概念
检索增强生成（RAG——Retrieval **Augmented** Generation）是指对大型语言模型输出进行优化，使其能够在生成响应之前引用训练数据来源之外的权威知识库。大型语言模型（LLM）用海量数据进行训练，使用数十亿个参数为回答问题、翻译语言和完成句子等任务生成原始输出。在 LLM 本就强大的功能基础上，RAG 将其扩展为能访问特定领域或组织的内部知识库，所有这些都无需重新训练模型。这是一种经济高效地改进 LLM 输出的方法，让它在各种情境下都能保持相关性、准确性和实用性。

```
会拿这个权威知识库进行训练嘛？

```

![[Pasted image 20240303130455.png]]
数据提取——embedding（向量化）——创建索引——检索——自动排序（Rerank）——LLM归纳生成

LLM 面临的已知挑战包括：
- 在没有答案的情况下提供虚假信息。
- 当用户需要特定的当前响应时，提供过时或通用的信息。
- 从非权威来源创建响应。
- 由于术语混淆，不同的培训来源使用相同的术语来谈论不同的事情，因此会产生不准确的响应。



## Lang Chain 起源

像 ChatGPT 这样的 大语言模型 或者 LLM 可以回答许多话题的问题，但是，一个孤立的 LLM 只知道它的训练内容，这并不包括某些个人信息数据，如果您可以和这些数据与 LLM 进行对话，就会非常有用


## Lang Chain 介绍
用于构建 LLM 应用的开源开发框架

## Lang Chain 组件
1. prompts 提示
2. models 模型
3. indexes 索引
4. chains 链
5. agents 代理
## Lang Chain 使用——如何与数据对话

![[Pasted image 20240303174734.png]]
核心：
1. 向量存储
	1. 加载数据
	2. 拆分为有意义的块
2. 索引--检索文档
	1. 语义搜索：最简单的方式
3. 使得 LLM 能回答 文档相关内容
4. 记忆功能
### 文档加载器
从不同格式和来源的数据中访问和转换数据的具体细节，转换为标准化格式，即加载到一个标准的文档对象中，该对象由内容和相关元数据组成。
> Lang Chain 中有 80 种 数据加载器
```python
# 例
from langchain.document_loaders import PyPDFLoader
loader = PyPDFLoader("docs/cs229_lectures/MachineLearning-Lecture01.pdf")
pages = loader.load()

# 例
from langchain.document_loaders import WebBaseLoader

loader = WebBaseLoader("https://github.com/basecamp/handbook/blob/master/37signals-is-you.md")
docs = loader.load()
```

文档加载器之间可以组合为一个通用加载器
```python
from langchain.document_loaders.generic import GenericLoader
from langchain.document_loaders.parsers import OpenAIWhisperParser
from langchain.document_loaders.blob_loaders.youtube_audio import YoutubeAudioLoader
# ! pip install yt_dlp
# ! pip install pydub
url="https://www.youtube.com/watch?v=jGwO_UgTS7I"
save_dir="docs/youtube/"
loader = GenericLoader(
    YoutubeAudioLoader([url],save_dir),
    OpenAIWhisperParser()
)
docs = loader.load()
docs[0].page_content[0:500]
```
> 可以同时加载多个文件
```python
from langchain.document_loaders import PyPDFLoader

# Load PDF
loaders = [
    # Duplicate documents on purpose - messy data
    PyPDFLoader("docs/cs229_lectures/MachineLearning-Lecture01.pdf"),
    PyPDFLoader("docs/cs229_lectures/MachineLearning-Lecture01.pdf"),
    PyPDFLoader("docs/cs229_lectures/MachineLearning-Lecture02.pdf"),
    PyPDFLoader("docs/cs229_lectures/MachineLearning-Lecture03.pdf")
]
docs = []
for loader in loaders:
    docs.extend(loader.load())
```
### 字符切割

> 为什么需要字符切割

RAG是一种基于检索的生成模型，它结合了检索和生成的能力。在RAG中，检索阶段用于从大型文本语料库中检索相关的上下文，然后再通过生成阶段生成响应或答案。

切割文本可以帮助RAG模型更好地处理长文本，并提高生成的效率和质量。长文本可能包含大量冗余信息或无关信息，这可能会对模型的性能产生负面影响。通过将文本切割成较小的片段，RAG模型可以在生成阶段更好地处理和理解这些部分，并减少不必要的计算。

另外，文本切割还可以帮助限制输入的长度，以满足模型的输入限制或资源限制。通过将文本切割成块，可以更好地管理模型的内存和计算需求。

总的来说，在转换为向量存储之前，还要对原始数据分为有意义的块，作为后续索引的单位？？。

![[Pasted image 20240303183026.png]]
#### 导入包

##### 第一类
```python
# 例，还有其他分割器，基于文本
from langchain.text_splitter import RecursiveCharacterTextSplitter, CharacterTextSplitter
```

RecursiveCharacterTextSplitter"和"CharacterTextSplitter"都是用于文本分割的类，但它们有不同的实现方式和适用场景。

1. RecursiveCharacterTextSplitter（递归字符文本分割器）：
    
    - 递归字符文本分割器是基于递归的分割方法，它将文本逐层地切割成更小的片段。
    - 它使用递归算法，将文本分割成单个字符或字符的子序列。例如，将"Hello"分割为["H", "e", "l", "l", "o"]。
    - 递归字符文本分割器适用于一些需要对文本进行字符级别处理的任务，例如生成文本的字符级别表示或字符级别的语言建模。
2. CharacterTextSplitter（字符文本分割器）：
    
    - 字符文本分割器是将文本按照固定长度切割成块的方法。
    - 它将文本按照指定的块大小分割成连续的字符序列。例如，将"Hello, world!"以块大小为5分割为["Hello", ", wor", "ld!"]。
    - 字符文本分割器适用于一些需要对文本进行块级别处理的任务，例如将文本输入模型进行批处理或限制输入长度。
    - 默认的分隔符是‘\n’，如果没有这个分隔符，即使到达 长度也不会分隔。
    - 带有回溯的正则表示
##### 第二类
```python
# 基于 token
from langchain.text_splitter import TokenTextSplitter
text_splitter = TokenTextSplitter(chunk_size=1, chunk_overlap=0)

```
因为有许多基于 token 计数设计的 LLM 上下文窗口
##### 第三类
```python
# 会为块，添加元数据信息
# 例 
from langchain.document_loaders import NotionDirectoryLoader
from langchain.text_splitter import MarkdownHeaderTextSplitter
markdown_document = """# Title\n\n \
## Chapter 1\n\n \
Hi this is Jim\n\n Hi this is Joe\n\n \
### Section \n\n \
Hi this is Lance \n\n 
## Chapter 2\n\n \
Hi this is Molly"""
headers_to_split_on = [
    ("#", "Header 1"),
    ("##", "Header 2"),
    ("###", "Header 3"),
]

markdown_splitter = MarkdownHeaderTextSplitter(
    headers_to_split_on=headers_to_split_on
)
md_header_splits = markdown_splitter.split_text(markdown_document)

md_header_splits[0]

# 切割的  Hi this is Jim\n\n Hi this is Joe\n\n \ 的元数据中会有 head1 和 head2 
```
####  包的使用
只有两个成员函数
```python
create_documents()
split_documents()
```

```python
# 回溯
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=150,
    chunk_overlap=0,
    separators=["\n\n", "\n", "(?<=\. )", " ", ""]
)
r_splitter.split_text(some_text)
```
### 嵌入
将文本转为数字格式
将块放入索引之中 
#### 转换为向量
```python
from langchain.embeddings.openai import OpenAIEmbeddings
embedding = OpenAIEmbeddings()

sentence1 = "i like dogs"
sentence2 = "i like canines"
sentence3 = "the weather is ugly outside"

embedding1 = embedding.embed_query(sentence1)
embedding2 = embedding.embed_query(sentence2)
embedding3 = embedding.embed_query(sentence3)

import numpy as np

np.dot(embedding1, embedding2)
np.dot(embedding1, embedding3)
np.dot(embedding2, embedding3)


```
#### 向量存储与使用
```python 
# ! pip install chromadb

from langchain.vectorstores import Chroma
persist_directory = 'docs/chroma/'
!rm -rf ./docs/chroma  # remove old database files if any
vectordb = Chroma.from_documents(
    documents=splits,
    embedding=embedding,
    persist_directory=persist_directory
)
print(vectordb._collection.count())

# 持久化存储
vectordb.persist()

# 使用，基本查询，寓意相似性查询
question = "is there an email i can ask for help"
docs = vectordb.similarity_search(question,k=3) # 3个最佳结果

len(docs)
  
docs[0].page_content



```
#### 边缘情况检索的失效问题

原因：基于语义进行查找。
1. 重复的查询结果
2. 查询不能准确把握语义

### 高级检索
#### MMR 最大边际相关性
如果选择与嵌入空间中查询最相似的文档，实际上可能会错过一些多样化的信息
指定搜索源
这样做的代价是需要对语言模型进行更多的调用，但密非常适合将最终答案集中在最重要的内容上

#### 对话检索链
在检索问答链的基础上添加了一个新的部分，不仅有记忆，它将历史记录和新问题压缩成一个独立的问题以便传递给向量存储以查找相关文档

### 记忆功能
保持一个聊天记录的列表，作为历史记录的缓冲区，并且每次都将这些消息与问题一起传递给聊天机器人。

Feel free to copy this code and modify it to add your own features. You can try alternate memory and retriever models by changing the configuration in `load_db` function and the `convchain` method. [Panel](https://panel.holoviz.org/) and [Param](https://param.holoviz.org/) have many useful features and widgets you can use to extend the GUI.
请随意复制此代码并对其进行修改以添加您自己的功能。 您可以通过更改“load_db”函数和“convchain”方法中的配置来尝试替代内存和检索器模型。 [Panel](https://panel.holoviz.org/) 和 [Param](https://param.holoviz.org/) 有许多有用的功能和小部件，可用于扩展 GUI。



```python
 pip3  install -U docarray
```

```python

pip3  install pydantic==1.10.9
```