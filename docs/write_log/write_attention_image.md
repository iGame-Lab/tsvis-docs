可视化视觉Transformer的注意力图。点击可视化图像对应的坐标，显示其对应的全部的注意力图。

```python
summarywriter.add_attentionmap(input_tensor: tensor,
                               attn_map: tensor,
                               cls: Optional[bool]=True,
                               normalize: Optional[list]=None,
                               tag:Optional[str]='transformer'):
    """
    	添加视觉Transformer需要可视化的图像及其对应的的注意力图数据到日志
    Args:
    	input_tensor： tensor类型，需要可视化的图像批次，维度为[n, H, W, C]
    	attn_map: tensor类型，需要可视化的图像在模型中的attention map值。维度为[n, l, num_head, h, w]或者[n, l, h, w]。batch_size为输入图像批次的数量，l为模型的层数，num_head为每层头的个数，h,w为提取到的attention map的高宽。
    	cls: 布尔类型，可选参数，默认为True，模型有cls embbeding。
    	normalize： 列表，可选参数，默认为None, 假如input_tensor是经过归一化的图像，需要设置normalize参数来反归一化获得原图像。
    	tag:字符串，可选参数，默认为'transformer'，Transformer可视化的标识
    """
```

