# 使用python

## 库

```python
# 导入数学计算
import numpy as np
# 导入结构化数据处理
import pandas as pd
# 导入绘图模块
import matplotlib.pyplot as plt
# 导入基于 matplotlib 且数据结构与 pandas 统一的统计图制作库。
import seaborn as sns


```

### 魔术命令

> 一组专门用于增强Jupyter Notebook交互性和功能的特殊命令。这些命令以`%`或`%%`开头，并且只能在Jupyter Notebook环境中使用。
>
> 注意：魔术命令不能在 python 命令行中执行。

1. **行魔术命令（line magic commands）**：以`%`开头的命令，作用于单行代码。例如：
   - `%run`：运行外部Python脚本。
   - `%pwd`：显示当前工作目录。
   - `%time`：测量代码的执行时间。
2. **单元魔术命令（cell magic commands）**：以`%%`开头的命令，作用于整个代码单元格。例如：
   - `%%time`：测量整个单元格的执行时间。
   - `%%html`：将单元格内容解释为HTML。
   - `%%bash`：在单元格中运行Bash命令。
3. **帮助命令**：以`?`结尾的命令，用于获取相关对象或函数的帮助信息。例如：
   - `len?`：获取`len`函数的帮助信息。
   - `obj?`：获取对象`obj`的帮助信息。
4. **魔术命令的参数和选项**：魔术命令可以接受参数和选项，以进一步定制其行为。例如：
   - `%matplotlib inline`：将Matplotlib图形嵌入到Notebook中。
   - `%run -i script.py`：以交互模式运行外部脚本。

### 装饰函数
[Python__](https://m.baidu.com/s?word=Python&sa=re_dqa_zy) 的装饰器是一种重要的编程概念，它们允许开发者在不改变被装饰函数源码的情况下，为函数添加额外的职责或者行为。装饰器通常由一个函数组成，它可以接收另一个函数作为输入，并返回一个新的函数对象。这些新的函数对象包含了原函数的功能和一些额外的逻辑。

以下是关于 Python 装饰器的几个例子：

1. `@lru_cache` 装饰器用于提高性能，特别是对于那些经常重复计算的函数。它会缓存函数的计算结果，以便在未来相同的参数调用下可以直接获取缓存中的结果，而无需重新计算。这种缓存机制特别适用于那些计算成本较高的场景。
    
2. `@total_ordering` 装饰器则是为了提供缺失的比较方法，特别是在没有实现这些方法的标准Python类中。通过使用这个装饰器，可以为预定义的Python类自动生成比较方法，确保不同实例之间能够进行正确的比较。
    

总结来说，Python 装饰器是设计用来简化代码、增强函数功能和提升程序效率的工具。它们使得开发者能够在保持函数接口不变的同时，灵活地扩展其功能。



# 问题解决

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

## 问题：显存不足

```
torch.cuda.OutOfMemoryError: CUDA out of memory. Tried to allocate 288.00 MiB. GPU 0 has a total capacity of 10.75 GiB of which 44.62 MiB is free. Process 1403323 has 10.69 GiB memory in use. Of the allocated memory 10.48 GiB is allocated by PyTorch, and 29.33 MiB is reserved by PyTorch but unallocated. If reserved but unallocated memory is large try setting PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True to avoid fragmentation.  See documentation for Memory Management  (https://pytorch.org/docs/stable/notes/cuda.html#environment-variables)
```

使用 `watch -n 0.1 nvidia-smi `查看 显存情况，可以看到运行的一瞬间，显存爆掉了。