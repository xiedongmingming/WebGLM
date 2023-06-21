# 笔记

## 部署相关

1. 对应的虚拟环境：webglm
2. 安装依赖：

```
pip install -r requirements.txt

pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117

pip install sentencepiece
```

3. 下载模型数据

```
python download.py retriever-pretrained-checkpoint --save ./tmp
```

4. 运行代码：

```
python web_demo.py --retriever_ckpt_path ./tmp/retriever-pretrained-checkpoint
```

注意：需要先设置环境变量SERPAPI_KEY