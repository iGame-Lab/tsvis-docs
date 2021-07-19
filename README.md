# Zjvis 可视化软件文档

## 目录结构

```
zjvis-doc
├── docs                                       # 文档主目录
│   ├── developer                              # 开发者文档目录
│   │   ├── images                             # 该目录下文档使用到的图片资源
│   │   ├── api.md                             # 后端API
│   │   ├── bugs.md                            # bug反馈
│   │   ├── commandline_for_backend.md         # 后端启动命令行参数
│   │   └── frontend.md                        # 前端请求
│   ├── quick_start                            # 快速使用目录
│   │   ├── images                             # 该目录下文档使用到的图片资源
│   │   ├── install.md                         # 安装教程
│   │   ├── quickstart.md                      # 简单使用教程
│   │   └── run_visual.md                      # 运行可视化
│   ├── thanks                                 # 致谢目录
│   │   ├── images                             # 该目录下文档使用到的图片资源
│   │   └── license.md                         # 使用到的开源软件
│   ├── use_visualization                      # 前端使用教程目录
│   │   ├── images                             # 该目录下文档使用到的图片资源
│   │   ├── custom.md                          # 用户定制使用教程
│   │   ├── embedding.md                       # 降维分析使用教程
│   │   ├── exception.md                       # 异常分析使用教程
│   │   ├── graph.md                           # 模型结构使用教程
│   │   ├── hyperparm.md                       # 超参分析使用教程
│   │   ├── media.md                           # 媒体数据使用教程
│   │   ├── scalar.md                          # 标量数据使用教程
│   │   └── statistic.md                       # 统计分析使用教程
│   ├── write_log                              # 写日志教程目录
│   │   ├── images                             # 该目录下文档使用到的图片资源
│   │   ├── write_embedding.md                 # 写高维数据
│   │   ├── write_exception.md                 # 写异常数据
│   │   ├── write_graph.md                     # 写模型结构数据
│   │   ├── write_hyperparm.md                 # 写超参数数据
│   │   ├── write_media.md                     # 写媒体数据
│   │   ├── write_scalar.md                    # 写标量数据
│   │   └── write_statistic.md                 # 写统计分析数据
│   └── index.md                               # 文档主页（禁止修改！！！）
├── mkdocs.yml                                 # mkdocs配置（禁止修改！！！）
└── README.md
```

各部分需要在相应的目录下，相应的文件中书写使用文档。

## 书写要求

**文档目录结构的骨架已经搭好，大家到相应的目录下把文档补充完整即可，不要修改文件名、文件夹名字**

先把可视化前端的使用文档完成，具体每个文件的说明参考第二节目录结构

0. 文档必须使用Markdown书写
2. 每个模块要首先写清楚该模块的作用
3. 要把每个功能怎么使用都写上
4. 配图**不要截全屏**，只需要截浏览器的内容
5. 将配图放到相应目录中的`images`文件夹下，如`use_visualization/media.md`中使用到的图片应放到`use_visualization/images`文件夹

正确的截图示例

![](https://cdn.jsdelivr.net/gh/Feyily/imgbed@master/20210719225229.png)

错误的截图示例

![](https://cdn.jsdelivr.net/gh/Feyily/imgbed@master/20210719225403.png)

## 文档预览

该文档基于`mkdocs`文档生成工具及`material`主题，预览文档需要安装Python依赖

```
pip install mkdocs mkdocs-material
```

之后cd到文档目录下，使用命令启动本地预览

```
mkdocs serve
```
