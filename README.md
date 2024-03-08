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
   在学术界，一些研究团队开始尝试将Langchain的思想应用于本地知识库的构建。他们致力于通过构建知识图谱、整合领域专家知识等方式，建立起丰富的本地知识库，并结合大型语言模型进行问题回答。这些研究旨在提高问答系统对中文语境的理解和处理能力，使其能够更准确地回答用户的问题。清华大学自然语言处理与社会人文计算实验室使用Langchain构建了一个名为“文心一言”的聊天机器人。该机器人集成了多种NLP技术，包括文本生成、语义理解、情感分析等，能够与用户进行自然、流畅的对话，并提供各种信息和服务。本文就是基于Langchain框架构建本地的知识库并接入LLM模型来构建的一个对话问答系统
<p align="center">
    <img src="./figure/Langchain.jpeg" width=900px/>
</p>

我们选择了 [InternLM大语言模型](https://github.com/InternLM/InternLM) 作为底座大语言模型，进行了**本地知识库的接入**，旨在提升模型的针对呼和浩特地区的交流问答能力。

## 使用方法
* 克隆本项目
```bash
cd ~
git clone https://github.com/R2-xurongjian/my_HohhotLLM.git
```
* 下载模型    
本项目使用的LLM模型为复旦大学研发的InternLM（书生·浦语）大语言模型，建议手动在huggingface上下载模型也可以git下载
```
git clone https://huggingface.co/internlm/internlm2-chat-7b
```

* 创建并激活conda环境    
这里我不做具体的虚拟conda环境的创建
```
conda activate InternLM
```

* 安装Langchain相关依赖
```
pip install langchain==0.0.292
pip install gradio==4.4.0
pip install chromadb==0.4.15
pip install sentence-transformers==2.2.2
pip install unstructured==0.10.30
pip install markdown==3.3.7
```
* 下载开源词向量模型    
同时，我们需要使用到开源词向量模型 [Sentence Transformer](https://huggingface.co/sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2):（我们也可以选用别的开源词向量模型来进行 Embedding，目前选用这个模型是相对    轻量、支持中文且效果较好的）

* 首先需要使用 `huggingface` 官方提供的 `huggingface-cli` 命令行工具。安装依赖:

```
pip install -U huggingface_hub
```

* 然后在根目录下新建python文件 `download.py`，填入以下代码：
```python
import os
* 运行download.py进行下载

# 下载模型
os.system('huggingface-cli download --resume-download sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2 --local-dir /root/data/model/sentence-transformer')
```
* 安装运行demo所需要的依赖
```shell
# 升级pip
python -m pip install --upgrade pip

pip install modelscope==1.9.5
pip install transformers==4.35.2
pip install streamlit==1.24.0
pip install sentencepiece==0.1.99
pip install accelerate==0.24.1
```
   
* 启动服务     
本项目提供了[run_gradio.py](./run_gradio.py)作为模型的使用示例，接通过 python 命令运行，即可在本地启动知识库助手的 Web Demo，默认会在 7860 端口运行，接下来将服务器端口映射到本地端口即可访问
```
python run_gradio.py
```

## 示例
* 整体布局
<p align="center">
    <img src="./figure/整体布局.png" width=900px/>
</p>  

* 样例1：介绍一下呼和浩特的文化
<p align="center">
    <img src="./figure/文化.png" width=900px/>
</p>

* 样例2：介绍一下呼和浩特的大昭寺

<p align="center">
    <img src="./figure/大昭寺.png" width=900px/>
</p>

* 样例3：内蒙古大学在呼和浩特的位置

<p align="center">
    <img src="./figure/内蒙古大徐也.png" width=900px/>
</p>


## 声明
* 本项目使用了ChatGLM-6B 模型的权重，需要遵循其[MODEL_LICENSE](https://github.com/THUDM/ChatGLM-6B/blob/main/MODEL_LICENSE)，因此，**本项目仅可用于您的非商业研究目的**。
* 本项目提供的SoulChat模型致力于提升大模型的共情对话与倾听能力，然而，模型的输出文本具有一定的随机性，当其作为一个倾听者的时候，是合适的，但是不建议将SoulChat模型的输出文本替代心理医生等的诊断、建议。本项目不保证模型输出的文本完全适合于用户，用户在使用本模型时需要承担其带来的所有风险！
* 您不得出于任何商业、军事或非法目的使用、复制、修改、合并、发布、分发、复制或创建SoulChat模型的全部或部分衍生作品。
* 您不得利用SoulChat模型从事任何危害国家安全和国家统一、危害社会公共利益、侵犯人身权益的行为。
* 您在使用SoulChat模型时应知悉，其不能替代医生、心理医生等专业人士，不应过度依赖、服从、相信模型的输出，不能长期沉迷于与SoulChat模型聊天。

## 致谢
本项目由[华南理工大学未来技术学院](https://www2.scut.edu.cn/ft/main.htm) 广东省数字孪生人重点实验室发起，得到了华南理工大学信息网络工程研究中心、电子与信息学院等学院部门的支撑，同时致谢广东省妇幼保健院、广州市妇女儿童医疗中心、中山大学附属第三医院、合肥综合性国家科学中心人工智能研究院等合作单位。

同时，我们感谢以下媒体或公众号对本项目的报道（排名不分先后）：
* 媒体报道
  [人民日报](https://wap.peopleapp.com/article/rmh36174922/rmh36174922)、[中国网](https://hs.china.com.cn/gd/83980.html)、[光明网](https://health.gmw.cn/2023-06/13/content_36628062.htm)、[TOM科技](https://tech.tom.com/202306/4526869977.html)、[未来网](http://www.zzfuture.cn/news/956.html)、[大众网](http://linyi.dzwww.com.3xw.site/xinwen/202306/t20230613_202306135667.htm)、[中国发展报道网](http://www.chinafzbdw.com/computer/13149.html?1686564408)、[中国日报网](http://energy.chinaduily.com.cn/c/2023/15205.html)、[新华资讯网](http://www.xinhuazxun.com/world/21762.html?1686564382)、[中华网](https://life.china.com/2023-06/12/content_215815.html)、[今日头条](https://www.toutiao.com/article/7243412314223952418/)、[搜狐](https://www.sohu.com/a/684501109_120159010)、[腾讯新闻](https://page.om.qq.com/page/OhSXIMEUtDtdg0rTi6aAoTbg0)、[网易新闻](https://www.163.com/dy/article/I70BJ9U00552UJUX.html)、[中国资讯网](http://www.chinazxun.com/world/23252.html?1686564532)、[中国传播网](http://www.chinachbo.com/a/view/11697.html?1686564509)、[中国都市报道网](http://www.zgdsbdw.com/meida/11273.html?1686564485)、[中华城市网](http://www.zhcsww.com/hot/2023/0612/9609.html?1686564434)

* 公众号
  [广东实验室建设](https://mp.weixin.qq.com/s/gemlKfLg8c-AtjiV7uTUTQ)、[智能语音新青年](https://mp.weixin.qq.com/s/vBMKXUJoAIywkXY2nY60eA)、[深度学习与NLP](https://mp.weixin.qq.com/s/qSHLT8FbvohZESp-UCah6g)、[AINLP](https://mp.weixin.qq.com/s/EX3f9WblLKM8K_nSwhno_g)

## 引用
```bib
@inproceedings{chen-etal-2023-soulchat,
    title = "{S}oul{C}hat: Improving {LLM}s{'} Empathy, Listening, and Comfort Abilities through Fine-tuning with Multi-turn Empathy Conversations",
    author = "Chen, Yirong  and
      Xing, Xiaofen  and
      Lin, Jingkai  and
      Zheng, Huimin  and
      Wang, Zhenyu  and
      Liu, Qi  and
      Xu, Xiangmin",
    editor = "Bouamor, Houda  and
      Pino, Juan  and
      Bali, Kalika",
    booktitle = "Findings of the Association for Computational Linguistics: EMNLP 2023",
    month = dec,
    year = "2023",
    address = "Singapore",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.findings-emnlp.83",
    pages = "1170--1183",
    abstract = "Large language models (LLMs) have been widely applied in various fields due to their excellent capability for memorizing knowledge and chain of thought (CoT). When these language models are applied in the field of psychological counseling, they often rush to provide universal advice. However, when users seek psychological support, they need to gain empathy, trust, understanding and comfort, rather than just reasonable advice. To this end, we constructed a multi-turn empathetic conversation dataset of more than 2 million samples, in which the input is the multi-turn conversation context, and the target is empathetic responses that cover expressions such as questioning, comfort, recognition, listening, trust, emotional support, etc. Experiments have shown that the empathy ability of LLMs can be significantly enhanced when finetuning by using multi-turn dialogue history and responses that are closer to the expression of a psychological consultant.",
}
}
```
