# 基本流程
数据集：
[RockYou password leak](https://wiki.skullsecurity.org/index.php/Passwords)

使用论文预训练模型：
1. 最多 10 个字符密码训练的 PassGPT [此处](https://huggingface.co/javirandor/passgpt-10characters/)。 
2. 最多 16 个字符的密码训练的版本需要我们团队的研究批准，可以在[此处](https://huggingface.co/javirandor/passgpt-16characters/)找到。

自己训练模型：
1. 密码 tokenizer ：字符级（防止像 NLP 那样将 字符 串为分词），在模型下保留有意义的概率分布。
```python

python src/create_tokenizer.py --train_path {PATH_TO_TRAINING_DATA} --output_path {PATH_TO_TOKENIZERS_FOLDER}
# Customization
python src/create_tokenizer.py --train_path data/rockyou.txt --output_path output

```
2. 自定义一个配置文件： `CONFIG_PATH` ，可以使用默认的文件 [yaml 文件](https://github.com/javirandor/passbert/blob/main/configs/passgpt-16chars.yaml).
3. 训练模型：
```python
python src/train_passgpt.py --config_path {CONFIG_PATH}
# Customization
python src/train_passgpt.py --config_path configs/passgpt-16chars.yaml
```
4. 模型使用
```python
python src/generate_passwords.py --model_path {MODEL_PATH} --out_path {PASSWORD_OUTPUT_FOLDER} --num_generate {NUM_PASSWORDS} --train_data_path {PATH_TO_TRAINING_DATA} --eval_data_path {PATH_TO_EVAL_DATA}
# Customizeation
 python src/generate_passwords.py --model_path train_output/last/ --out_path ./train_output/last/ --num_generate 100000000 --train_data_path data/rockyou.txt --eval_data_path data/rockyou.txt --tokenizer_path output/byte_bpe_tokenizer_99/
```
	进一步的生成参数，例如“--temper”、“--top_p”或“--top_k”

# 环境
conda + python3.11
requirements.txt
```text
accelerate==0.28.0
aiohttp==3.9.3
aiosignal==1.3.1
appdirs==1.4.4
asttokens==2.4.1
attrs==23.2.0
azure-core==1.30.1
azure-storage-blob==12.19.1
certifi==2024.2.2
cffi==1.16.0
charset-normalizer==3.3.2
click==8.1.7
comm==0.2.1
cryptography==42.0.5
datasets==2.18.0
debugpy==1.6.7
decorator==5.1.1
dill==0.3.8
docker-pycreds==0.4.0
exceptiongroup==1.2.0
executing==2.0.1
filelock==3.13.1
frozenlist==1.4.1
fsspec==2024.2.0
gitdb==4.0.11
GitPython==3.1.42
huggingface-hub==0.21.4
idna==3.6
importlib_metadata==7.0.2
ipykernel==6.29.3
ipython==8.22.2
isodate==0.6.1
jedi==0.19.1
Jinja2==3.1.3
joblib==1.3.2
jsonlines==4.0.0
jupyter_client==8.6.0
jupyter_core==5.7.1
lamini==2.1.2
lamini-configuration==0.8.3
MarkupSafe==2.1.5
matplotlib-inline==0.1.6
mpmath==1.3.0
multidict==6.0.5
multiprocess==0.70.16
nest_asyncio==1.6.0
networkx==3.2.1
numpy==1.26.4
packaging==24.0
pandas==2.2.1
parso==0.8.3
pexpect==4.9.0
pickleshare==0.7.5
pip==23.3.1
platformdirs==4.2.0
prompt-toolkit==3.0.42
protobuf==4.25.3
psutil==5.9.0
ptyprocess==0.7.0
pure-eval==0.2.2
pyarrow==15.0.1
pyarrow-hotfix==0.6
pycparser==2.21
Pygments==2.17.2
python-dateutil==2.9.0
pytz==2024.1
PyYAML==6.0.1
pyzmq==25.1.2
regex==2023.12.25
requests==2.31.0
safetensors==0.4.2
scikit-learn==1.4.1.post1
scipy==1.12.0
sentry-sdk==1.43.0
setproctitle==1.3.3
setuptools==68.2.2
six==1.16.0
smmap==5.0.1
stack-data==0.6.2
sympy==1.12
threadpoolctl==3.3.0
tokenizers==0.15.2
torch==2.2.1
tornado==6.3.3
tqdm==4.66.2
traitlets==5.14.1
transformers==4.38.2
triton==2.2.0
typing_extensions==4.10.0
tzdata==2024.1
urllib3==2.2.1
wandb==0.16.4
wcwidth==0.2.13
wheel==0.41.2
xxhash==3.4.1
yarl==1.9.4
zipp==3.17.0
```
# 问题

## 问题： 'utf-8' 解析出错

```python
    with open(args.train_path, "r", encoding="utf-8") as f:

    # with open(args.train_path, "r", encoding="utf-8", errors='replace') as f:

        lines = f.read().splitlines()
```
> UnicodeDecodeError: 'utf-8' codec can't decode byte 0xf1 in position 5079963: invalid continuation byte

解决1：使用默认字符替换，默认是 “?”。
`with open(args.train_path, "r", encoding="utf-8", errors='replace') as f`

解决2：定位到没有解析的字符行，重新输入


## 问题： 包缺失
```text
  File "/opt/conda/envs/py3_11/lib/python3.11/site-packages/transformers/training_args.py", line 1905, in _setup_devices
    raise ImportError(
ImportError: Using the `Trainer` with `PyTorch` requires `accelerate>=0.21.0`: Please run `pip install transformers[torch]` or `pip install accelerate -U`

```

```text
Traceback (most recent call last):
  File "/root/PSG/passgpt-main/src/train_passgpt.py", line 120, in <module>
    trainer = Trainer(
              ^^^^^^^^
  File "/opt/conda/envs/py3_11/lib/python3.11/site-packages/transformers/trainer.py", line 533, in __init__
    self.callback_handler = CallbackHandler(
                            ^^^^^^^^^^^^^^^^
  File "/opt/conda/envs/py3_11/lib/python3.11/site-packages/transformers/trainer_callback.py", line 313, in __init__
    self.add_callback(cb)
  File "/opt/conda/envs/py3_11/lib/python3.11/site-packages/transformers/trainer_callback.py", line 330, in add_callback
    cb = callback() if isinstance(callback, type) else callback
         ^^^^^^^^^^
  File "/opt/conda/envs/py3_11/lib/python3.11/site-packages/transformers/integrations/integration_utils.py", line 673, in __init__
    raise RuntimeError("WandbCallback requires wandb to be installed. Run `pip install wandb`.")
```

## 问题：显存不够
```text
torch.cuda.OutOfMemoryError: CUDA out of memory. Tried to allocate 288.00 MiB. GPU 0 has a total capacity of 10.75 GiB of which 44.62 MiB is free. Process 1338858 has 10.69 GiB memory in use.
```
1. 换服务器，当前服务器显卡资源不足：
![[Pasted image 20240322093550.png]]

2. 新服务器
![[Pasted image 20240322093300.png]]

## 问题： 显示 Driver Version 过低
（导致训练时不能使用 GPU）
	此时不一定是 Driver Version 版本的问题，在实际中只要 显卡 显卡驱动兼容，那么就可以使用显卡跑程序。
	通过 降低 pytorch 的版本到 2.0.0 ，成功运行。
	



![[Pasted image 20240322011742.png]]

![[Pasted image 20240322091910.png]]



# 当前的进度

目前的进度： PassGPT 训练过程结束（最大字符 长度 设置为 10）

![[Pasted image 20240322102651.png]]

当前实验存在的问题：
运行模型 需要 的配置较高。
[模型显存需求估计](https://zhuanlan.zhihu.com/p/673125345)



# 下一步计划
之前是跑了模型的训练部分，下一步就是使用模型生成密码，并且分析数据。

计划的实验目标是：
	1. 论文中没有提到只是说生成一定的数量的密码之后对测试数据集的覆盖率达到了多少，但是并没有提到那些 类型数据 是模型难以拟合的，我主要想看一下哪些没有匹配到的密码长什么样子。
	2. 对比以下 开源的预训练 10 字符 模型 和 此次训练的模型的效果，验证一下论文的效果。


目前的一个小 idea，参考 [[MoEs]] ，但了解还比较粗浅。

最后感觉论文读的还是太少了，需要接着调研论文 


模型运行结果：https://wandb.ai/xinka/huggingface/runs/f4ta95w4?nw=nwuserfengdreamin
