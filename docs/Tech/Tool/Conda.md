---
title: Conda使用笔记
---

# Conda

在使用python的时候，不可避免地会遇到版本管理的问题。我采用miniconda来进行虚拟环境的管理，这里记录一下常用的命令。

## 常用命令

在powershell中集成conda：（用于在powershell中激活虚拟环境等，实测会导致powershell冷启动速度大幅降低）

```bash
conda init powershell
conda init --reverse    # 撤销
```

创建虚拟环境：

```bash
conda create -n myenv python=3.7    # -n表示name
```

激活虚拟环境：

```bash
conda activate myenv
```

退出虚拟环境：

```bash
conda deactivate
```

查看虚拟环境列表：

```bash
conda env list
```

删除虚拟环境：

```bash
conda env remove -n myenv
```

安装包：

```bash
conda install numpy
conda install --file requirements.txt
```

增加conda-forge channel（由社区维护，包更全）

```bash
conda config --add channels conda-forge
```

# 2 遇到问题

## pip将依赖包默认安装至C盘

pip将依赖包均安装至`C:\Users\<User-Name>\AppData\Roaming\Python`下，而非conda目录下。

根据网上一些教程修改`Python\Lib\site.py`文件中的`USER_SITE`和`USER_BASE`变量后无果。使用命令`python -m site`后显示的`USER_SITE`和`USER_BASE`仍然是C盘下目录。

解决方法：增加环境变量`PYTHONUSERBASE`后即可调整目录。