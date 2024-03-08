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
git clone https://github.com/scutcyr/SoulChat.git
```

* 安装依赖    
需要注意的是torch的版本需要根据你的服务器实际的cuda版本选择，详情参考[pytorch安装指南](https://pytorch.org/get-started/previous-versions/)
```bash
cd SoulChat
conda env create -n proactivehealthgpt_py38 --file proactivehealthgpt_py38.yml
conda activate proactivehealthgpt_py38

pip install cpm_kernels
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
```

* 【补充】Windows下的用户推荐参考如下流程配置环境
```bash
cd BianQue
conda create -n proactivehealthgpt_py38 python=3.8
conda activate proactivehealthgpt_py38
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
pip install -r requirements.txt
pip install rouge_chinese nltk jieba datasets
# 以下安装为了运行demo
pip install streamlit
pip install streamlit_chat
```
* 【补充】Windows下配置CUDA-11.6：[下载并且安装CUDA-11.6](https://developer.nvidia.com/cuda-11-6-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local)、[下载cudnn-8.4.0，解压并且复制其中的文件到CUDA-11.6对应的路径](https://developer.nvidia.com/compute/cudnn/secure/8.4.0/local_installers/11.6/cudnn-windows-x86_64-8.4.0.27_cuda11.6-archive.zip)，参考：[win11下利用conda进行pytorch安装-cuda11.6-泛用安装思路](https://blog.csdn.net/qq_34740266/article/details/129137794)

* 在Python当中调用SoulChat模型    
```python
import torch
from transformers import AutoModel, AutoTokenizer
# GPU设置
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
# 加载模型与tokenizer
model_name_or_path = 'scutcyr/SoulChat'
model = AutoModel.from_pretrained(model_name_or_path, trust_remote_code=True).half()
model.to(device)
tokenizer = AutoTokenizer.from_pretrained(model_name_or_path, trust_remote_code=True)

# 单轮对话调用模型的chat函数
user_input = "我失恋了，好难受！"
input_text = "用户：" + user_input + "\n心理咨询师："
response, history = model.chat(tokenizer, query=input_text, history=None, max_length=2048, num_beams=1, do_sample=True, top_p=0.75, temperature=0.95, logits_processor=None)

# 多轮对话调用模型的chat函数
# 注意：本项目使用"\n用户："和"\n心理咨询师："划分不同轮次的对话历史
# 注意：user_history比bot_history的长度多1
user_history = ['你好，老师', '我女朋友跟我分手了，感觉好难受']
bot_history = ['你好！我是你的个人专属数字辅导员甜心老师，欢迎找我倾诉、谈心，期待帮助到你！']
# 拼接对话历史
context = "\n".join([f"用户：{user_history[i]}\n心理咨询师：{bot_history[i]}" for i in range(len(bot_history))])
input_text = context + "\n用户：" + user_history[-1] + "\n心理咨询师："

response, history = model.chat(tokenizer, query=input_text, history=None, max_length=2048, num_beams=1, do_sample=True, top_p=0.75, temperature=0.95, logits_processor=None)
```


* 启动服务   

本项目提供了[soulchat_app.py](./soulchat_app.py)作为SoulChat模型的使用示例，通过以下命令即可开启服务，然后，通过http://<your_ip>:9026访问。
```bash
streamlit run soulchat_app.py --server.port 9026
```
特别地，在[soulchat_app.py](./soulchat_app.py)当中，
可以修改以下代码更换指定的显卡：
```python
os.environ['CUDA_VISIBLE_DEVICES'] = '2'
```
**对于Windows单显卡用户，需要修改为：```os.environ['CUDA_VISIBLE_DEVICES'] = '0'```，否则会报错！**

可以通过更改以下代码指定模型路径为本地路径：
```python
model_name_or_path = 'scutcyr/SoulChat'
```


## 示例
* 样例1：失恋
*
<p align="center">
    <img src="./figure/example_shilian.png" width=600px/>
</p>

* 样例2：宿舍关系

<p align="center">
    <img src="./figure/example_sushe.png" width=600px/>
</p>

* 样例3：期末考试

<p align="center">
    <img src="./figure/example_kaoshi.png" width=600px/>
</p>

* 样例4：科研压力

<p align="center">
    <img src="./figure/example_keyan.png" width=600px/>
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
