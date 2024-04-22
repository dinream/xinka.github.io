

vscode 终端不显示 环境

https://github.com/microsoft/vscode-python/wiki/Activate-Environments-in-Terminal-Using-Environment-Variables


# 相关工具
### Anaconda 命令行界面使用

1. 防止不是最新的，更新：

   `conda update -n base -c defaults conda`

2. 查看版本：
	`conda --version`

3. 管理虚拟环境

   1. active【环境名】：引导用户进入独立的环境。在不加参数的情况下，默认进入base环境。这里不加参数

   2. conda create -n 【环境名】 python=【版本号】：创建一个虚拟环境。

      `conda create -n test_env python=3`

      (python版本为3(这里conda会自动找python3中最新的版本下载))

   3. conda env list：列出所有环境。验证。

   4. activate 【环境名】：进入环境

      `activate test_env`

   5. conda 

   6. activate 【环境名】：进入（激活）环境

4. 安装
	`conda install 【包名】` 


#### Conda常见命令

**使用 conda包管理工具需要进入`anaconda prompt`**

conda --version，输出anaconda的版本

```python
conda install --offline ../input/efficient-net/dist/efficientnet_pytorch-0.7.0.tar`
# 其中 `../input/efficient-net/dist/efficientnet_pytorch-0.7.0.tar
```




# 安装 python Jupyter 版

### 介绍

Jupyter Notebook是基于网页的用于交互计算的应用程序。其可被应用于全过程计算：开发、文档编写、运行代码和展示结果。——[Jupyter Notebook官方介绍](https://link.zhihu.com/?target=https%3A//jupyter-notebook.readthedocs.io/en/stable/notebook.html)

Jupyter Notebook是以网页的形式打开，可以在网页页面中**直接编写代码**和**运行代码**，代码的**运行结果**也会直接在代码块下显示的程序。如在编程过程中需要编写说明文档，可在同一个页面中直接编写，便于作及时的说明和解释。

# 安装 python
参考：https://zhuanlan.zhihu.com/p/33105153
参考：

1. [神器 VS Code，讲讲超详细的 Python 配置使用指南_用vs code开发python需要准备什么-CSDN博客](https://blog.csdn.net/pythonhy/article/details/131701597)
2. [Anaconda搭建python环境 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/643022828?utm_id=0)

补充：conda、anaconda概念的差别

1. conda可以理解为一个工具，也是一个可执行命令，其核心功能是包管理与环境管理。包管理与pip的使用类似，环境管理则允许用户方便地安装不同版本的python并快速切换。

2. Anaconda是一个打包的集合，里面预装好了conda、某个版本的python、众多packages、科学计算工具等，也称为Python的一种发行版。

## Windows安装

### 一、Python安装

依照教程，Python 可以从 Python 官网下载安装，也可以使用 Anaconda 来安装。本教程选择了 用  Anaconda。

##### 下载地址

https://www.anaconda.com/products/individual

下载完成之后直接按照默认设置一步步安装即可。（挺大的，5G左右，换个合适的目录）

### 二、Vscode 安装

这里好多教程，最好直接官网上下一个。



### 三、环境配置

进行到这一步之后，只是工具的准备完成，但是：

1. win 的 cmd 命令行 不能找到 python 指令
2. VScode 的 终端都不能识别到 python 指令
3. 打开 Anaconda 界面，在界面中打开 VScode，可以识别 python 指令

#### 环境变量配置

1. 保存 anaconda 路径到系统 “用户变量” 的 path 变量中。保存。此时 Win cmd 中就可以 识别到 conda 指令。



### 四、VScode插件安装

1. 扩展插件：Python 

2. 代码补齐：Pylance

3. JupyterNotebook 支持：Jupyter

4. 高亮缩进：indent-rainbow

5. 括号高亮：Bracket pair Colorizer

6. 注释高亮：Better Comments

7. settings.json

   ```json
       
       // python 
       "python.defaultInterpreterPath": "D:\\Users\\fyn\\anaconda3\\python.exe", 
       "python.condaPath": "D:\\Users\\fyn\\anaconda3\\Scripts\\conda-script.py",
       "python.experiments.optInto": [
           "pythonTerminalEnvVarActivation"
       ],
       // "python.formatting.provider": "D:\\Users\\fyn\\anaconda3\\lib\\yapf",
       // "python.pythonPath": "D:\\Users\\fyn\\anaconda3\\python.exe",
       "python.terminal.activateEnvInCurrentTerminal": true,
       "python.terminal.activateEnvironment": true,
   ```

   



## Linux Conda 安装

[Ubuntu安装anaconda,tensorflow等 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/43348901)

### 一、Conda 安装

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
bash a*.sh

# 回车 查看协议 
# q 退出查看
# yes 确认
# 回车确定 安装目录， 一般为 /home/Anaconda  
# 对应目录增加权限方便所有用户使用 conda chmod -R a+rx /opt/anaconda3

# yes 确定通过 conda init 启动
```

### 二、Conda 启动环境变量

```bash
# 首先 关闭并重新打开终端，使得conda 生效
# 升级
conda update -n base -c defaults conda
# 修改 Path
export PATH="/root/anaconda3/bin:$PATH"
source ~/.bashrc

```

### 三、Conda 修改国内源

```bash
# 输入
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda create -n py39 python=3.9

# 若是CondaHTTPError，

conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
（http不要s)

```

### 四、Conda 进行环境管理

#### 环境创建

```bash
# 查看 conda 信息
conda info --env
# 创建环境 
conda create -n your_env_name python=x.x
conda create -n fyn_python2 python=2.7.12
# 删除环境
conda remove -n your_env_name --all
# 切换环境
conda activate your_env_name 
# 退出/关闭环境


  
pip3 list --format=columns
```

#### 利用依赖包安装环境

```bash
pip install -r requirements.txt
# 引用清华镜像源
# -i https://pypi.tuna.tsinghua.edu.cn/simple/ 
```

####  生成依赖包配置

```bash
# 第一种，一般只适合完全相同的环境
pip freeze > d:/requirements.txt
# 可能出现格式： 包名＋版本号＋@ file:///tmp/build/xxx/xxx/work
# 此格式无法在其他系统识别，

# 第二种，解决第一种的问题。
pip list --format=freeze > requirements.txt
```

## Linux 离线安装
```bash
## 准备 离线依赖包 和 python 安装包
# 离线包安装依赖
yum -y localinstall ./gcc/*.rpm
yum -y localinstall ./bzip2-devel/*.rpm
yum -y localinstall ./libffi-devel/*.rpm
yum -y localinstall ./openssl-devel/*.rpm
yum -y localinstall ./zlib-devel/*.rpm

# 安装
tar -xf Python-3.9.10.tgz
cd Python-3.9.10
./configure --prefix=/usr/local/python-3.9.10 --enable-optimizations
make 
make altinstall
ln -s /usr/local/python-3.9.10/bin/python3.9 /usr/bin/python3
```
# 卸载 python
## Linux
```bash
# 卸载 python3.6---没测
rm -rf /usr/local/bin/python3
rm -rf /usr/local/bin/python3.6
rm -rf /usr/local/include/python3.6m
rm -rf /usr/local/lib/python3.6
rm -rf /usr/local/lib/libpython3.6m.a
```
# 使用 python 库
## 离线包安装
1. 打包
	```python
	python3 -m pip download <package>
	```
2. 使用
	```python
	 pip install <package>.whl
	```

## 安装 CUDA

https://link.zhihu.com/?target=https%3A//developer.nvidia.com/rdp/cudnn-download

[Log in | NVIDIA Developer](https://developer.nvidia.com/login)

```bash
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## 安装 tensorflow/torch

```bash
https://link.zhihu.com/?target=https%3A//github.com/mind/wheels/releases
```

- PyTorch: `pip install torch`
- TensorFlow: `pip install tensorflow`
- Flax: `pip install flax`

来更新已安装的库：

- PyTorch: `pip install --upgrade torch`
- TensorFlow: `pip install --upgrade tensorflow`
- Flax: `pip install --upgrade flax`

安装 参考https://blog.csdn.net/CLOUD_J/article/details/112474224

1. 安装 runfile

   https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=runfile_local

   选择runfile，按照给出来的指令执行：

   ```
   wget https://developer.download.nvidia.com/compute/cuda/12.3.2/local_installers/cuda_12.3.2_545.23.08_linux.run
   sudo sh cuda_12.3.2_545.23.08_linux.run
   ```

   1. 第一个ACCEPT接受
   2. 第二个选择安装的东西
   3. 带X的是要安装的，有驱动就把驱动关掉，只安装cuda tool kit即可
   4. 然后移动到最下面install
   5. 点回车
   6. 等待片刻安装好了

2. 环境变量

   ```
   vim ~/.bashrc
   #下面加进去有
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
   export PATH=$PATH:/usr/local/cuda/bin
   export CUDA_HOME=$CUDA_HOME:/usr/local/cuda
   #source一下
   source ~/.bashrc
   
   ### /usr/local/cuda-12.3/bin  实际上与上面相同
   
   ```

   安装结束

   ```
   ===========
   = Summary =
   ===========
   
   Driver:   Not Selected
   Toolkit:  Installed in /usr/local/cuda-12.3/
   
   Please make sure that
    -   PATH includes /usr/local/cuda-12.3/bin
    -   LD_LIBRARY_PATH includes /usr/local/cuda-12.3/lib64, or, add /usr/local/cuda-12.3/lib64 to /etc/ld.so.conf and run ldconfig as root
   
   To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-12.3/bin
   ***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least 545.00 is required for CUDA 12.3 functionality to work.
   To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
       sudo <CudaInstaller>.run --silent --driver
   
   Logfile is /var/log/cuda-installer.log
   ```

   

3. nvcc -V检查安装是否成功，显示版本则成功。

   此时就可以执行

4. 安装 CUDNN




## 深度学习库比较
> scikit-learn、PyTorch 和 Keras 的信息：
>
>1. scikit-learn（sklearn）:
>   - 是一个流行的 Python 机器学习库，提供了各种经典的机器学习算法和工具。
>   - 包含用于分类、回归、聚类、降维、特征选择等任务的算法。
>   - 提供了数据预处理、特征工程、模型评估和模型选择的功能。
>   - 具有简单且一致的API，易于使用和上手。
>   - 适合处理传统机器学习任务，特别是在数据量较小或特征工程较为重要的情况下。
>2. PyTorch:
>   - 是一个用于构建和训练神经网络的深度学习库。
>   - 提供了动态计算图的能力，使得模型构建和调整更加灵活。
>   - 具有丰富的深度学习函数和模块，用于处理张量操作、构建神经网络层、定义损失函数等。
>   - 支持自动求导，方便进行反向传播和梯度更新。
>   - 提供了灵活的调试和自定义选项，适合深入研究和开发深度学习模型。
>3. Keras:
>   - 是一个高级的神经网络 API，用于构建和训练深度学习模型。
>   - 提供了简洁、一致的接口，使得模型的原型设计和实验更加快速和便捷。
>   - 可以作为 TensorFlow、Theano 和 Microsoft CNTK 等深度学习框架的后端。
>   - 支持多种网络结构，包括顺序模型、函数式 API 和子类化 API。
>   - 提供了丰富的预处理工具、模型评估和可视化工具。

#



