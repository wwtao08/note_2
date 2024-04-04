## 基础作业 
部署 InternLM2-Chat-1.8B 模型进行智能对话  
#### 配置基础环境
配置开发机系统  
![image](https://github.com/wwtao08/note_2/blob/main/image1.png?raw=true)    
terminal 中输入环境配置命令  
![image](https://github.com/wwtao08/note_2/blob/main/image2.png?raw=true)   
![image](https://github.com/wwtao08/note_2/blob/main/image3.png?raw=true)  
#### 下载 InternLM2-Chat-1.8B,运行 cli_demo
指定字数，多写一篇
![image](https://github.com/wwtao08/note_2/blob/main/image4.png?raw=true)  
## 进阶作业 
1. **进阶作业1**：  
   - 熟悉 huggingface 下载功能，使用 huggingface_hub python 包，下载 InternLM2-Chat-7B 的 config.json 文件到本地（需截图下载过程）
使用 Hugging Face官方提供的 `huggingface-cli` 命令行工具来安装依赖:
```bash
pip install -U huggingface_hub
```
然后新建 `python` 文件，填入以下代码，运行即可。
+ resume-download：断点续下
+ local-dir：本地存储路径。
```python
import os
os.system('huggingface-cli download --resume-download internlm/internlm2-chat-7b --local-dir your_path')
```  
![image](https://github.com/wwtao08/note_2/blob/main/image5.png?raw=true)      
2. **进阶作业2**：  
   - 完成 浦语·灵笔2 的 图文创作 及 视觉问答 部署（需截图）
**环境**
进入开发机，启动 `conda` 环境：
```bash
conda activate demo
# 补充环境包
pip install timm==0.4.12 sentencepiece==0.1.99 markdown2==2.4.10 xlsxwriter==3.1.2 gradio==4.13.0 modelscope==1.9.5
```

下载 **InternLM-XComposer 仓库** 相关的代码资源：

```bash
cd /root/demo
git clone https://gitee.com/internlm/InternLM-XComposer.git
# git clone https://github.com/internlm/InternLM-XComposer.git
cd /root/demo/InternLM-XComposer
git checkout f31220eddca2cf6246ee2ddf8e375a40457ff626
```
![image](https://github.com/wwtao08/note_2/blob/main/image11.png?raw=true)  
在 `terminal` 中输入指令，构造软链接快捷访问方式：

```bash
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm-xcomposer2-7b /root/models/internlm-xcomposer2-7b
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm-xcomposer2-vl-7b /root/models/internlm-xcomposer2-vl-7b
```

**图文创作**

继续输入指令，用于启动 `InternLM-XComposer`：

```bash
cd /root/demo/InternLM-XComposer
python /root/demo/InternLM-XComposer/examples/gradio_demo_composition.py  \
--code_path /root/models/internlm-xcomposer2-7b \
--private \
--num_gpus 1 \
--port 6006
```

待程序运行的同时，参考章节 3.3 部分对端口环境配置本地 `PowerShell` 。使用快捷键组合 `Windows + R`（Windows 即开始菜单键）打开指令界面，（Mac 用户打开终端即可）并输入命令，按下回车键：
打开 PowerShell 后，先查询端口，再根据端口键入命令 （例如图中端口示例为 38374）：

```bash
# 从本地使用 ssh 连接 studio 端口
# 将下方端口号 38374 替换成自己的端口号
ssh -CNg -L 6006:127.0.0.1:6006 root@ssh.intern-ai.org.cn -p 38374
```
再复制下方的密码，输入到 `password` 中，直接回车，
打开 [http://127.0.0.1:6006](http://127.0.0.1:6006) 实践效果如下图所示：
![image](https://github.com/wwtao08/note_2/blob/main/image10.png?raw=true)  
![image](https://github.com/wwtao08/note_2/blob/main/image12.png?raw=true)
![image](https://github.com/wwtao08/note_2/blob/main/image13.png?raw=true)

### 5.4 **图片理解实战（开启 50% A100 权限后才可开启此章节）**

根据附录 6.4 的方法，关闭并重新启动一个新的 `terminal`，继续输入指令，启动 `InternLM-XComposer2-vl`：
```bash
conda activate demo
cd /root/demo/InternLM-XComposer
python /root/demo/InternLM-XComposer/examples/gradio_demo_chat.py  \
--code_path /root/models/internlm-xcomposer2-vl-7b \
--private \
--num_gpus 1 \
--port 6006
```
![image](https://github.com/wwtao08/note_2/blob/main/image14.png?raw=true) 
打开 [http://127.0.0.1:6006](http://127.0.0.1:6006) (上传图片后) 键入内容示例如下：

    请分析一下图中内容
 
![image](https://github.com/wwtao08/note_2/blob/main/image15.png?raw=true)


3. **进阶作业3**：  
   - 完成 Lagent 工具调用 数据分析 Demo 部署（需截图）
**使用 `Lagent` 运行 `InternLM2-Chat-7B` 模型为内核的智能体**
打开 lagent 路径：

```bash
cd /root/demo/lagent
```
![image](https://github.com/wwtao08/note_2/blob/main/image6.png?raw=true)  
在 terminal 中输入指令，构造软链接快捷访问方式：

```bash
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-7b /root/models/internlm2-chat-7b
```

打开 `lagent` 路径下 `examples/internlm2_agent_web_demo_hf.py` 文件，并修改对应位置 (71行左右) 代码：

```bash
# 其他代码...
value='/root/models/internlm2-chat-7b'
# 其他代码...
```

输入运行命令 - **点开 6006 链接后，大约需要 5 分钟完成模型加载：**

```bash
streamlit run /root/demo/lagent/examples/internlm2_agent_web_demo_hf.py --server.address 127.0.0.1 --server.port 6006
```

待程序运行的同时，对本地端口环境配置本地 `PowerShell` 。使用快捷键组合 `Windows + R`（Windows 即开始菜单键）打开指令界面，并输入命令，按下回车键。（Mac 用户打开终端即可）


打开 PowerShell 后，先查询端口，再根据端口键入命令 （例如图中端口示例为 38374）：


```bash
# 从本地使用 ssh 连接 studio 端口
# 将下方端口号 38374 替换成自己的端口号
ssh -CNg -L 6006:127.0.0.1:6006 root@ssh.intern-ai.org.cn -p 38374
```
![image](https://github.com/wwtao08/note_2/blob/main/image8.png?raw=true)  
再复制下方的密码，输入到 `password` 中，直接回车；
打开 [http://127.0.0.1:6006](http://127.0.0.1:6006) 后，（会有较长的加载时间）勾上数据分析，其他的选项不要选择，进行计算方面的 Demo 对话，即完成本章节实战。键入内容示例：

    请解方程 2*X=1360 之中 X 的结果

![image](https://github.com/wwtao08/note_2/blob/main/image9.png?raw=true) 


