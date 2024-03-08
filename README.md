# HohhotLLM
Urban Large Language Model for Hohhot (呼和浩特城市大模型)
## Urban Large Language Model for Hohhot (呼和浩特城市大模型)
国内在垂直领域大模型的研究方面已经取得了显著的进展。随着深度学习技术的不断发展和计算能力的提升，越来越多的研究者开始关注如何利用大模型来解决特定领域的问题。
本项目基于Langchain为基础，结合LLM大语言模型，并以丰富的呼和浩特城市数据为基础构建了本地问答知识库来宣传呼和浩特的城市文化。
* LangChain是一个基于大语言模型（如ChatGPT）用于构建端到端语言模型应用的Python框架。[Langchain](https://github.com/langchain-ai/langchain)    
* 经过百万规模心理咨询领域中文长文本指令与多轮共情对话数据联合指令微调的 [InternLM大语言模型](https://github.com/InternLM/InternLM)   

我们期望，**Langchain构建本地知识库** 可以帮助学术界加速大模型在特定区域文化宣传的研究与应用。本项目为 **Urban Large Language Model for Hohhot** 。
## 最近更新
- 👏🏻  2024.3.08：我们的呼和浩特城市文化大模型正式发布！！！
## 简介
   我们调研了当前常见的心理咨询平台，发现，用户寻求在线心理帮助时，通常需要进行较长篇幅地进行自我描述，然后提供帮助的心理咨询师同样地提供长篇幅的回复（见[figure/single_turn.png](./figure/single_turn.png)），缺失了一个渐进式的倾诉过程。但是，在实际的心理咨询过程当中，用户和心理咨询师之间会存在多轮次的沟通过程，在该过程当中，心理咨询师会引导用户进行倾诉，并且提供共情，例如：“非常棒”、“我理解你的感受”、“当然可以”等等（见下图）。
<p align="center">
    <img src="./figure/Langchain.jpeg" width=900px/>
</p>

我们选择了 [InternLM大语言模型](https://github.com/InternLM/InternLM) 作为底座大语言模型，进行了**本地知识库的接入**，旨在提升模型的针对呼和浩特地区的交流问答能力。

