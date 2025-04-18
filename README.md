<div align="center">
<h1>测试 Grounding Image Matching in 3D with MASt3R</h1>
</div>


```bash
#下载代码链接，并切换分支a100
git clone --recursive git@github.com:KwanWaiPang/MASt3R_comment.git
# rm -rf .git

```

* 创建环境：

```bash
conda create -n mast3r python=3.11 cmake=3.14.0
conda activate mast3r 
conda install pytorch torchvision pytorch-cuda=12.1 -c pytorch -c nvidia  # use the correct version of cuda for your system
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
pip install .  # or python3 setup.py build_ext --inplace
cd ..
```

* 安装RoPE 

```bash
# DUST3R relies on RoPE positional embeddings for which you can compile some cuda kernels for faster runtime.
cd dust3r/croco/models/curope/
python setup.py build_ext --inplace
cd ../../../../
```