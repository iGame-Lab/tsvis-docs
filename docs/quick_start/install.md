# 安装指南

本节将介绍如何安装zjvis，我们提供了两种安装方式：

- pip安装
- 从源码安装

不管通过那种方式安装，都需要确保你的Python版本为3.6以上，如果不满足请先升级Python

## 使用pip安装

!!! 建议
    
    为了方便，我们建议使用pip安装方式（暂未上线）

安装命令

```
pip install zjvis
```

## 从源码安装

### 从源码编译前端

首先安装依赖

```
npm install
```

然后修改`zjvis/webapp/src/utils/constants.js`文件内参数

```
const DJANGOHOSTNAME = ''
```

使用命令打包前端生成静态文件

```
npm run build
```

### 从源码安装后端

从源码安装后端需要先将前端编译生成的静态文件移动到`zjvis/server/frontend`文件夹下

然后安装Python依赖包`setuptools`

```
pip install setuptools
```

执行`setup.py`文件安装zjvis到Python环境

```
python setup.py install
```
