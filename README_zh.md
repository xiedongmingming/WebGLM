<h1>WEBGLM：基于人类偏好的高效网络增强问答系统</h1>

<p align="center">📃 <a href="https://arxiv.org/pdf/2306.07906.pdf" target="_blank">论文 (KDD 2023)</a>

本项目为WEBGLM的官方实现。

[DEMO视频](https://github.com/THUDM/WebGLM/assets/129033897/d2e1dd35-6340-4175-ac2d-fd585daa17cf
)

_READ THIS IN [ENGLISH](README.md)._

<!-- TOC -->

-   [概述](#概述)
    -   [特点](#特点)
-   [开发准备](#开发准备)
    -   [准备代码和环境](#准备代码和环境)
    -   [准备SERPAPI密钥](#准备SERPAPI密钥)
    -   [下载检索器权重](#下载检索器权重)
-   [尝试WEBGLM](#尝试WEBGLM)
    -   [导出环境变量](#导出环境变量)
    -   [以命令行界面运行](#以命令行界面运行)
    -   [以WEB服务形式运行](#以WEB服务形式运行)
-   [训练WEBGLM](#训练WEBGLM)
    -   [训练生成器](#训练生成器)
        -   [准备数据](#准备数据)
        -   [训练](#训练)
    -   [训练检索器](#训练检索器)
        -   [准备数据](#准备数据-1)
        -   [训练](#训练-1)
-   [评测](#评测)
-   [实际应用案例](#实际应用案例)
-   [引用](#引用)

# 概述

![paper](./assets/main_process.png)

WEBGLM旨在使用10亿参数的通用语言模型（GLM）提供一种高效且低成本的网络增强问答系统。它旨在通过将网络搜索和召回功能集成到预训练的语言模型中以进行实际应用的部署。

## 特点

-   **大模型增强检索器**：增强了相关网络内容的检索能力，以更好地准确回答问题。
-   **自举生成器**：利用GLM的能力为问题生成回复，提供详细的答案。
-   **基于人类偏好的打分器**：通过优先考虑人类偏好来评估生成回复的质量，确保系统能够产生有用和吸引人的内容。

# 开发准备

## 准备代码和环境

克隆此仓库，并安装所需第三方库

```bash
pip install -r requirements.txt
```

安装NODEJS。

```bash
apt install nodejs # 如果你使用UBUNTU
```

安装PLAYWRIGHT依赖项。

```bash
playwright install
```

如果你的主机中没有安装浏览器环境，则需要安装。不用担心，如果是这种情况，PLAYWRIGHT会在首次执行时出现说明。

## 准备SERPAPI密钥

在搜索过程中，我们使用SERPAPI获取搜索结果。你需要从[这里](https://serpapi.com/)获取SERPAPI密钥。

然后将环境变量`SERPAPI_KEY`设置为你的密钥。

或者，你可以通过PLAYWRIGHT使用BING。你可以在WEBGLM的启动命令行中添加`--searcher bing`以使用BING搜索。

```bash
export SERPAPI_KEY="YOUR KEY"
```

## 下载检索器权重

通过运行以下命令从[清华云](https://cloud.tsinghua.edu.cn/d/54056861b2f34bbfb3f9/)下载检索器的权重。

你可以通过`--save SAVE_PATH`手动指定检索器权重的保存路径。

```bash
python download.py retriever-pretrainedy-checkpoint
```

# 尝试WEBGLM

在运行代码之前，请确保你的设备空间足够。

## 导出环境变量

将环境变量`WEBGLM_RETRIEVER_CKPT`设定为检索器权重的路径。如果你已将检索器权重下载到默认路径，可以直接运行以下命令行。

```bash
export WEBGLM_RETRIEVER_CKPT=./download/retriever-pretrained-checkpoint
```

## 以命令行界面运行

你可以尝试WEBGLM-2B模型：

```bash
python cli_demo.py -w THUDM/WebGLM-2B
```

或直接尝试WEBGLM-10B模型：

```bash
python cli_demo.py
```

如果你想使用BING搜索而不是SERPAPI，可以在命令行中添加`--searcher bing`，例如：

```bash
python cli_demo.py -w THUDM/WebGLM-2B --searcher bing
```

## 以WEB服务形式运行

使用与`CLI_DEMO.PY`相同的参数运行`WEB_DEMO.PY`。例如，你可以通过BING搜索使用WEBGLM-2B模型：

```bash
python web_demo.py -w THUDM/WebGLM-2B --searcher bing
```

# 训练WEBGLM

## 训练生成器

### 准备数据

运行下面的命令行从[清华云](https://cloud.tsinghua.edu.cn/d/ae204894f2e842f19a3f/)下载训练数据。

```bash
python download.py generator-training-data
```

它将自动下载所有数据，并将它们预处理成可以立即在`./DOWNLOAD`中使用的SEQ2SEQ格式。

### 训练

请参考[GLM仓库](https://github.com/THUDM/GLM#train-with-your-own-data)进行SEQ2SEQ训练。

## 训练检索器

### 准备数据

通过运行以下命令行，从[清华云](https://cloud.tsinghua.edu.cn/d/fa5e6eb1afac4f08a4c6/)下载训练数据。

```bash
python download.py retriever-training-data
```

### 训练

运行以下命令行来训练检索器。如果你已经在默认路径下载了检索器训练数据，可以直接运行以下命令行。

```bash
python train_retriever.py --train_data_dir ./download/retriever-training-data
```

# 评测

你可以在TRIVIAQA、WEBQUESTIONS和NQ OPEN上重现我们的结果。以TRIVIAQA为例，可以运行以下命令行：

```bash
bash scripts/triviaqa.sh
```

并开始进行评测。

# 真实应用案例

您可以在[这里](assets/cases)查看一些WEBGLM实际应用场景的示例。

# 引用

如果您针对您的研究使用了这个代码，请引用我们的论文。

```
@misc{liu2023webglm,
    title={WebGLM: Towards An Efficient Web-Enhanced Question Answering System with Human Preferences},
    author={Xiao Liu and Hanyu Lai and Hao Yu and Yifan Xu and Aohan Zeng and Zhengxiao Du and Peng Zhang and Yuxiao Dong and Jie Tang},
    year={2023},
    eprint={2306.07906},
    archivePrefix={arXiv},
    primaryClass={cs.CL}
}
```

> 该仓库已进行简化以便于部署。
