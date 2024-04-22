
# 个人环境搭建
## API

### openai

[api 使用文档](https://platform.openai.com/docs/api-reference/chat-completions/create)

## wenxin
[[微信项目]]
教程：
[如何申请文心一言&文心千帆大模型API调用资格、获取access_token，并使用SpringBoot接入文心一言API_文心一言api申请-CSDN博客](https://blog.csdn.net/qq_30299877/article/details/131917097)

在线调试工具
https://console.bce.baidu.com/tools/#/api?product=AI&project=%E6%96%87%E5%BF%83%E5%8D%83%E5%B8%86%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%B9%B3%E5%8F%B0&parent=%E9%89%B4%E6%9D%83%E8%AE%A4%E8%AF%81%E6%9C%BA%E5%88%B6&api=oauth%2F2.0%2Ftoken&method=post

token
24.0339bde53ae4eed0ed979b9f9959b20e.2592000.1711684739.282335-53912329
申请：https://console.bce.baidu.com/qianfan/ais/console/applicationConsole/application/create
[api 文档]([使用网页调试工具获取access_token - 千帆大模型平台 | 百度智能云文档 (baidu.com)](https://cloud.baidu.com/doc/WENXINWORKSHOP/s/Rlkkt6kd7))
##  github Copilot 免费使用 gpt-4
https://blog.geniucker.top/2024/01/26/%E9%80%9A%E8%BF%87-GitHub-Copilot-%E5%85%8D%E8%B4%B9%E4%BD%BF%E7%94%A8-gpt-4/

https://github.com/Geniucker/CoGPT?tab=readme-ov-file

我希望你充当激励教练。我将为您提供一些关于某人的目标和挑战的信息，而您的工作就是想出可以帮助此人实现目标的策略。这可能涉及提供积极的肯定、提供有用的建议或建议他们可以采取哪些行动来实现最终目标。我的第一个请求是“我需要帮助来激励自己在为即将到来的考试学习时保持纪律”。


# 常用的大模型应用技术

## Prompt
[[Prompt]]

### Course


- **[OpenAI Prompt 指南](https://platform.openai.com/docs/guides/prompt-engineering)**
    
- **[ChatGPT Prompt Engineering for Developers](https://learn.deeplearning.ai/courses/chatgpt-prompt-eng/lesson/1/introduction)**
    
- **[ChatGPT Prompt Engineering for Developers - 中文字幕版](https://www.bilibili.com/video/BV1e8411o7NP?p=1&vd_source=31f1c950b5b95af0c48f188f0bc047c7)**
    

### Project

- 内容: 使用 GPT-4, 设计 Prompt 优化 **图说数据库系统** 的文本内容.
    
- **基本要求:**
    
    - 优化自己负责部分的一个小节, 丰富内容, 优化章节结构, 语言风格等.
        
    - 采用将大问题分解为多个小问题的方式进行优化, 使用多个 Prompt 对比生成的结果. 最后, 对比丰富后与丰富前的文本. (保留优化前的文本)
        
- 进阶:
    
    - 其他任意 Prompt 工作都可.
        
    - 例如: 优化翻译结果, 优化特定领域结果(数据库专家, 文档编写专家等)
        
- 实验室提供的 GPT-4 Web https://chatgpt-next-web-mauve-five.vercel.app/#/chat
    

### 其他资料

- [GPTs 的 Prompt, 可用于参考](https://github.com/linexjlin/GPTs)
    

## RAG
[[RAG 检索增强]]
### Course

- **[Retrieval Augmented Generation (RAG) 简介 - 中文字幕版](https://www.bilibili.com/video/BV11G411X7nZ?p=15&vd_source=31f1c950b5b95af0c48f188f0bc047c7)**
    
- **[LangChain Chat with Your Data](https://learn.deeplearning.ai/langchain-chat-with-your-data/lesson/2/document-loading)**
    
- **[LangChain Chat with Your Data - 中文字幕版](https://www.bilibili.com/video/BV148411D7d2/?spm_id_from=333.337.search-card.all.click&vd_source=31f1c950b5b95af0c48f188f0bc047c7)**
    

### Project

- 利用指定的书籍文档, 构建 RAG 系统.
    
- 利用 RAG 系统优化 **图说数据库系统** 的文本内容.
    
- **基本要求:**
    
    - 优化自己部分的一个小节, 丰富内容, 修正错误. 与原文本, Prompt 生成的文本进行对比.
        
- 进阶:
    
    - 构建其他任意 RAG 系统.
    - 例如: 分布式课程 RAG 系统, 实验室文档 RAG 系统, 医疗 RAG 系统等.
        
- 实验室提供的 API-Key
    
- 可以用于 机器人的 聊天记录进行保存
### 其他资料

- [Building and Evaluating Advanced RAG](https://learn.deeplearning.ai/building-evaluating-advanced-rag/lesson/1/introduction)
    
- [Functions, Tools and Agents with LangChain](https://learn.deeplearning.ai/functions-tools-agents-langchain/lesson/1/introduction)
    
- [devv.ai 是如何构建高效的 RAG 系统的](https://twitter.com/tisoga/status/1731478506465636749?s=61&t=TVU99VOXdlAywAcBa_iuSg)
    
- [复杂 RAG 的技术考虑](https://twitter.com/i/web/status/1737037970367283474)
    

## MoE
[[MoEs]]
### Course

- [深入理解混合专家模型](https://baoyu.io/translations/llm/mixture-of-experts-explained)
    

## 微调

[[Finetuning]]
### Course

- [Finetuning Large Language Models](https://learn.deeplearning.ai/finetuning-large-language-models/lesson/1/introduction)
[吴恩达《微调大型语言模型》| Finetuning Large Language Models（中英字幕）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Rz4y1T7wz/?spm_id_from=333.337.search-card.all.click&vd_source=59461060c1867e9bf731e467ae6f00b5)






