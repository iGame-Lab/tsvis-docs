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
<br>
**ps**:可视化高维数据提供了两种方式：

（a）使用**同一组测试数据**在不同step生成高维嵌入数据。用户通过add_embedding_sample添加测试的样本数据，在可视化过程中可通过点击样本点查看原始样本。
```python
    model = MinistNet()
    test_x, test_y = dataset()
    
    writer.add_embedding_sample(tag='test', tensor=test_x, sample_type='iamge')   
    
    for step in range(steps):
        # 1.train stage
        model.train()
             .....
        
        # 2.eval stage
        model.eval()
        out = model.forward(test_x)
        writer.add_embedding(tag='test', tensor=out, label=test_y, step=step)
```

（b）使用**不同的数据**在不同的step生成高维嵌入数据。 此时每个step的样本数据并不相同，不再支持查看降维后每个样本点对应的样本数据。用户无需通过add_embedding_sample添加样本数据，否则查看的样本点数据将出现不一致等问题。
```python
    model = MinistNet()
    train_x, train_y = dataset()
    for step in range(steps):
        # 1.train stage
        model.train()
        out = model.forward(train_x)
        writer.add_embedding(tag='train', tensor=out, label=train_y, step=step)
```
    
