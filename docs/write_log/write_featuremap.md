可视化卷积神经网络的卷积特征
  ```python
  summarywriter.add_featuremap(model,
                               inputs,
                               task: str,
                               label=None,
                               methods: Union[tuple] = ('PFV', 'GradCam', 'Gray', 'guidedbp')):
        """
            添加特征图数据到日志
        Args:
            model: 神经网络模型
            inputs: 张量， 一组需要展示特征图的样本
            task: 字符串， 模型的任务标识，分为'Classification', 'Segmentation', 'Detection' 三类
            label: 张量， 可选参数， 输入样本的真实标签
            methods: 元组， 可选参数， 绘制特征图的方法
         """
  ```