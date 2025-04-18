<div align="center">
<h1>测试 Grounding Image Matching in 3D with MASt3R</h1>
</div>

<h3 align="center">
<a href="https://github.com/naver/mast3r">Original Github Page</a>
</h3>
<div align="justify">
</div>

## 安装配置

```bash
#下载代码链接，并切换分支a100
git clone --recursive git@github.com:KwanWaiPang/MASt3R_comment.git
# rm -rf .git

```

* 创建环境：

```bash
conda create -n mast3r python=3.11 cmake=3.14.0
conda activate mast3r 
# conda remove --name mast3r --all

conda install pytorch torchvision pytorch-cuda=12.1 -c pytorch -c nvidia  # use the correct version of cuda for your system A100中采用的为cuda12.2
pip install -r requirements.txt
pip install -r dust3r/requirements.txt
# Optional: you can also install additional packages to:
# - add support for HEIC images
# - add required packages for visloc.py
pip install -r dust3r/requirements_optional.txt

```

* 安装ASMK

```bash
pip install cython

git clone https://github.com/jenicek/asmk
cd asmk/cython/
cythonize *.pyx
cd ..
python3 setup.py build_ext --inplace
cd ..
```

* 安装RoPE 

```bash
# DUST3R relies on RoPE positional embeddings for which you can compile some cuda kernels for faster runtime.
cd dust3r/croco/models/curope/
python setup.py build_ext --inplace
cd ../../../../
```

* 下载模型

```bash
mkdir -p checkpoints/
wget https://download.europe.naverlabs.com/ComputerVision/MASt3R/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric.pth -P checkpoints/
```

## 运行测试

```bash
conda activate mast3r 
conda install faiss-gpu
# python3 demo.py --model_name MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric
CUDA_VISIBLE_DEVICES=0 python3 demo.py --model_name MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric
# vscode终端运行上面命令后，本机网页打开（采用的是网页UI）

# Use --weights to load a checkpoint from a local file, eg --weights checkpoints/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric.pth
# Use --retrieval_model and point to the retrieval checkpoint (*trainingfree.pth) to enable retrieval as a pairing strategy, asmk must be installed
# Use --local_network to make it accessible on the local network, or --server_name to specify the url manually
# Use --server_port to change the port, by default it will search for an available port starting at 7860
# Use --device to use a different device, by default it's "cuda"

demo_dust3r_ga.py is the same demo as in dust3r (+ compatibility for MASt3R models)
see https://github.com/naver/dust3r?tab=readme-ov-file#interactive-demo for details

```

* 注意，可能会出现报错`/tmp/gradio/fd76484cbb6559e836bf4706b63ca0dd7d9b622da6b42f22249c8f4d4d3394fd`,对于用服务器的用户可能出现访问权限的问题，在代码中设置访问临时目录即可,详细请见[code](demo.py)

```py
import os
os.environ['GRADIO_TEMP_DIR'] = '/path/to/your/temp/dir'
```

* 下面通过代码[link](matching.ipynb)来测试匹配的效果

```bash
conda activate mast3r 
pip install ipykernel 
```