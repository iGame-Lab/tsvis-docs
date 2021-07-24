可视化高维数据的聚类。以动画的形式显示样本数据在各网络层中产生的高维嵌入的聚类变化

  1.可视化的样本数据，只需在训练的开始或结束添加一次即可，tag应与产生的高维嵌入数据的tag保持一致

  ```python
  summarywriter.add_embedding_sample(tag: str,
                                     tensor: numpy_compatible,
                                     sample_type: str):
      """
          添加降维处理的高维数据对应的样本到日志，以便查看降维后数据点对应的原始样本
      Args:
          tag: 字符串，高维数据样本的标识，应与高维数据保持一致
          tensor: 数组，高维数据的样本，数据类型，大小为[N,*]
          sample_type: 字符串，样本数据的类型，支持‘image’,'text','audio'
      """
  ```

  2.样本数据产生的高维嵌入数据

  ```python
  summarywriter.add_embedding(tag: str,
                              tensor: numpy_compatible,
                              label: Optional[numpy_compatible] =None,
                              step: Optional[int] = None):
      """
          添加降维处理的高维数据到日志
      Args:
          tag: 字符串，降维处理的高维数据的标识，应与高维数据的样本标识相同
          tensor: 数组，高维数据的样本，大小为[N,*]
          label: 数组，可选参数，高维数据对应的类别标签，大小为[N]
          step: 整数，可选参数，记录数据的step
      """
  ```