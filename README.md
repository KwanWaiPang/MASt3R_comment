[comment]: <> 

<!-- PROJECT LOGO -->

<p align="center">

  <h1 align="center"> Grounding Image Matching in 3D with MASt3R
  </h1>

[comment]: <> (  <h2 align="center">PAPER</h2>)
  <h3 align="center">
  <a href="https://kwanwaipang.github.io/File/Blogs/Poster/MASt3R-SLAM.html">Blog</a> 
  | <a href="https://github.com/KwanWaiPang/DUSt3R_comment">My Test on DUSt3R</a>
  | <a href="https://github.com/naver/mast3r">Original Github Page</a>
  </h3>
  <div align="justify">
  </div>

<br>

* 安装步骤
~~~
conda create -n mast3r python=3.11 cmake=3.14.0
conda activate mast3r 
# conda remove --name mast3r --all
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.7 -c pytorch -c nvidia

pip install -r requirements.txt
pip install -r dust3r/requirements.txt
# Optional: you can also install additional packages to:
# - add support for HEIC images
# - add required packages for visloc.py
pip install -r dust3r/requirements_optional.txt

# DUST3R relies on RoPE positional embeddings for which you can compile some cuda kernels for faster runtime.
cd dust3r/croco/models/curope/
python setup.py build_ext --inplace
cd ../../../../

pip install numpy==1.24.1
~~~

* 下载模型
~~~
mkdir -p checkpoints/
wget https://download.europe.naverlabs.com/ComputerVision/MASt3R/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric.pth -P checkpoints/
~~~

* 运行测试
~~~
# demo.py is the updated demo for MASt3R. It uses our new sparse global alignment method that allows you to reconstruct larger scenes

conda activate mast3r 
python3 demo.py --model_name MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric

CUDA_VISIBLE_DEVICES=1 python3 demo.py --model_name MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric


# 服务器端运行上面命令后，本机网页打开（采用的是网页UI）
http://localhost:7860/

# Use --weights to load a checkpoint from a local file, eg --weights checkpoints/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric.pth
# Use --local_network to make it accessible on the local network, or --server_name to specify the url manually
# Use --server_port to change the port, by default it will search for an available port starting at 7860
# Use --device to use a different device, by default it's "cuda"

demo_dust3r_ga.py is the same demo as in dust3r (+ compatibility for MASt3R models)
see https://github.com/naver/dust3r?tab=readme-ov-file#interactive-demo for details
~~~
