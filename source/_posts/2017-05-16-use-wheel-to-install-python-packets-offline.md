---
title: 使用wheel离线安装python依赖包
date: 2017-05-16 18:38:20
categories: python
tags:
- python
- wheel
---

本文环境为
- CentOS6.5 x86_64
- Python 2.7.13


# 一、离线存储依赖包
安装需要用到``wheel``模块，本文因为操作系统为精简版，所以不带``pip``，先要安装``pip``

<!-- more -->

### 1. 安装``setuptools``及其依赖

```bash
# install six
cd /opt/ && \
curl https://pypi.python.org/packages/b3/b2/238e2590826bfdd113244a40d9d3eb26918bd798fc187e2360a8367068db/six-1.10.0.tar.gz -o six-1.10.0.tar.gz && \
tar zxvf six-1.10.0.tar.gz && \
cd six-1.10.0 && \
python setup.py install && \
cd .. && rm -rf six-1.10.0*

cd /opt/ && \
curl https://pypi.python.org/packages/c6/70/bb32913de251017e266c5114d0a645f262fb10ebc9bf6de894966d124e35/packaging-16.8.tar.gz -o packaging-16.8.tar.gz && \
tar zxvf packaging-16.8.tar.gz && \
cd packaging-16.8 && \
python setup.py install && \
cd .. && rm -rf packaging-16.8*

cd /opt/ && \
curl https://pypi.python.org/packages/48/69/d87c60746b393309ca30761f8e2b49473d43450b150cb08f3c6df5c11be5/appdirs-1.4.3.tar.gz -o appdirs-1.4.3.tar.gz && \
tar zxvf appdirs-1.4.3.tar.gz && \
cd appdirs-1.4.3 && \
python setup.py install && \
cd .. && rm -rf appdirs-1.4.3*

cd /opt/ && \
curl https://pypi.python.org/packages/3c/ec/a94f8cf7274ea60b5413df054f82a8980523efd712ec55a59e7c3357cf7c/pyparsing-2.2.0.tar.gz -o pyparsing-2.2.0.tar.gz && \
tar zxvf pyparsing-2.2.0.tar.gz && \
cd pyparsing-2.2.0 && \
python setup.py install && \
cd .. && rm -rf pyparsing-2.2.0*

cd /opt/ && \
curl https://pypi.python.org/packages/88/13/7d560b75334a8e4b4903f537b7e5a1ad9f1a2f1216e2587aaaf91b38c991/setuptools-35.0.2.zip -o setuptools-35.0.2.zip && \
unzip setuptools-35.0.2.zip && \
cd setuptools-35.0.2 && \
python setup.py install && \
cd .. && rm -rf setuptools-35.0.2*

cd /opt/ && \
curl https://pypi.python.org/packages/11/b6/abcb525026a4be042b486df43905d6893fb04f05aac21c32c638e939e447/pip-9.0.1.tar.gz -o pip-9.0.1.tar.gz && \
tar zxvf pip-9.0.1.tar.gz && \
cd pip-9.0.1 && \
python setup.py install && \
cd .. && rm -rf pip-9.0.1*
```

### 2. 安装``wheel``
```bash
cd /opt/ && \
curl https://pypi.python.org/packages/c9/1d/bd19e691fd4cfe908c76c429fe6e4436c9e83583c4414b54f6c85471954a/wheel-0.29.0.tar.gz -o wheel-0.29.0.tar.gz && \
tar zxvf wheel-0.29.0.tar.gz && \
cd wheel-0.29.0 && \
python setup.py install && \
cd .. && rm -rf wheel-0.29.0*
```

### 3. 使用``freeze``生成依赖文件``requirements.txt``
* 这里假设运行环境为``Flask-RESTful``
```bash
pip freeze > requirements.txt
```

### 4. 使用``wheel``下载依赖包
```bash
pip wheel --wheel-dir=wheelhouse -r requirements.txt
```
上述命名会将所有的依赖包下载到当前目录的``wheelhouse``下

### 5. 使用``wheel``离线安装
然后就可以把上一步保存下来的``requirements.txt``和``wheelhouse``拿到别的相同机器上，再通过``wheel``来安装这些离线包
```bash
pip install --use-wheel --no-index --find-links=wheelhouse -r requirements.txt
```

# 延伸阅读
[Python 包管理工具解惑](http://zengrong.net/post/2169.htm)
