# TS-VIS 框架无关的深度学习可视化工具包

## 什么是TS-VIS

TS-VIS（天枢Vis）是[天枢人工智能开源开放平台](https://gitee.com/zhijiangtianshu/Dubhe)的可视化组件，支持Tensorflow、Pytorch、Oneflow等主流深度学习框架的可视化功能

![](quick_start/images/demo.gif)

## 亮点

* 框架无关，支持TensorFlow、Pytorch、Oneflow等主流深度学习框架可视化
* 超快的响应速度
* 支持大规模的可视化
* 支持训练过程实时可视化
* 支持降维分析样本可视化
* 支持神经网络异常可视化
* 支持多种特征图可视化
* 支持Transformer可视化
* 支持RNN隐含状态可视化


## 支持功能

- 模型结构可视化：可视化网络结构，包括计算图和结构图
- 特征分析可视化：可视化卷积神经网络的卷积特征
- 标量数据可视化：可视化包括神经网络`accuary`和`loss`等的标量数据
- 媒体数据可视化：可视化包括图像、文字、音频在内的媒体数据
- 统计分析可视化：可视化神经网络中权重、偏置等的分布
- 降维分析可视化：通过降维算法，可视化任意高维数据
- 超参分析可视化：可视化不同超参数下的神经网络指标
- 异常检测可视化：将神经网络张量数据映射到二维，可视化张量数据统计信息
- 注意力分析可视化：可视化图像和文本transformer模型的注意力数据
- 隐状态分析可视化：可视化RNN隐含状态值并进行匹配分析
- 用户定制可视化：可以将所有功能移动到该模块进行可视化

## 如何开始？

首先，你可以从[快速使用](quick_start/install.md)了解如何安装TS-VIS，并亲自动手运行一个简单的例子。

在[可视化日志写入](write_log/write_graph.md)章节中，你可以了解到如何使用TS-VIS在Python代码中导出可视化日志。另外还可以得知写入可视化日志API的一些调用方法和参数。

[使用可视化模块](use_visualization/graph.md)中，我们介绍了可视化系统的操作、UI交互以及每一个可视化图像的意义，你可以通过可视化UI对可视化结果做出调整并完成一些交互。

[开发者文档](developer/api.md)提供了一些针对开发者的说明，包括所有可视化后端API请求参数及返回结果，TS-VIS的启动方式等。如果你发现了一些Bug，你也可以在该章节下找到Bug反馈的方式。

希望TS-VIS可以帮助你在深度学习的旅程中乘风破浪！

## 版权

TS-VIS 由杭州电子科技大学智能可视建模与仿真实验室(iGame Lab)和之江实验室(Zhejiang Lab)联合开发
Copyright © 2022 [iGame](http://igame.hdu.edu.cn/) & [Zhejiang](https://www.zhejianglab.com/) Lab



