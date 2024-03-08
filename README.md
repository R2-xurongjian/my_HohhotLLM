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
    <img src="./figure/multi_turn.png" width=900px/>
</p>

   考虑到当前十分欠缺多轮共情对话数据集，我们一方面，构建了超过15万规模的 **单轮长文本心理咨询指令与答案（SoulChatCorpus-single_turn）** ，回答数量超过50万（指令数是当前的常见的心理咨询数据集 [PsyQA](https://github.com/thu-coai/PsyQA) 的6.7倍），并利用ChatGPT与GPT4，生成总共约100万轮次的 **多轮回答数据（SoulChatCorpus-multi_turn）** 。特别地，我们在预实验中发现，纯单轮长本文驱动的心理咨询模型会产生让用户感到厌烦的文本长度，而且不具备引导用户倾诉的能力，纯多轮心理咨询对话数据驱动的心理咨询模型则弱化了模型的建议能力，因此，我们混合SoulChatCorpus-single_turn和SoulChatCorpus-multi_turn构造成超过120万个样本的 **单轮与多轮混合的共情对话数据集SoulChatCorpus** 。所有数据采用“用户：xxx\n心理咨询师：xxx\n用户：xxx\n心理咨询师：”的形式统一为一种指令格式。

我们选择了 [ChatGLM-6B](https://huggingface.co/THUDM/chatglm-6b) 作为初始化模型，进行了**全量参数的指令微调**，旨在提升模型的共情能力、引导用户倾诉能力以及提供合理建议的能力。更多训练细节请留意我们后续发布的论文。

