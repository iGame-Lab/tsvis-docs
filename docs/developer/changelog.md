# 更新记录

## v0.4.2(2021年8月25日)

1. 启用新名字`TS-VIS`
2. 修复了标量分析、媒体数据在空白文件夹下启动时的BUG [#17](https://github.com/iGame-Lab/TS-VIS/issues/17)
3. 修复了异常检测中切换step闪动问题解决
4. 更新onnx模型的图转换
5. 更新oneflow 日志写入demo
6. 降维分析样本支持音频数据 [#15](https://github.com/iGame-Lab/TS-VIS/issues/15)
7. 其他BUG修复

## v0.4.1(2021年7月30日)

1. 调整文件目录结构，添加pip编译安装脚本
2. 统计分析异常检测面板性能优化
3. 可视化前端新增定时刷新功能
4. 修复tsne降维动画不连续的问题
5. 修复前端UI错误 [#13](https://github.com/iGame-Lab/TS-VIS/issues/13)
6. 修复文本可视化中，文本换行问题 [#12](https://github.com/iGame-Lab/TS-VIS/issues/12)
7. 可视化后端性能优化

## v0.3(2021年7月22日)

1. 完善了日志监听，优化线程管理
2. 修复了异常检测中箱线图选取困难的问题 [#7](https://github.com/iGame-Lab/TS-VIS/issues/7)
3. 可视化后端添加单元测试
4. 降维分析页面BUG修复 [#9](https://github.com/iGame-Lab/TS-VIS/issues/9)


## v0.2(2021年7月16日)

1. 模型结构交互改进 [#8](https://github.com/iGame-Lab/TS-VIS/issues/8)
2. 修复超参数与文本数据分类错误
3. 更新启动命令行参数，加入单元测试子命令

## v0.1(2021年7月14日)

1. 更新可视化后端写日志接口
2. 修复数据降维中的显示错误 [#4](https://github.com/iGame-Lab/TS-VIS/issues/4)
3. 修复媒体数据图片高度变化导致的布局重排的问题
