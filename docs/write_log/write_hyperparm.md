可视化同一个或多个网络模型在不同超参数组合的训练结果

  ```python
  summarywriter.add_hparams(hparam_dict: Dict[str, Union[bool, str, float, int]],
                            metrics: Optional[Union[list, tuple]] =None,
                            tag: Optional[str] = None):
      """
          添加一组数据到日志
      Args:
          hparam_dict: 字典，模型的超参数
          metrics: 列表、元组，可选参数， 需要记录的度量指标，该指标必须在scalar中已定义
          tag: 字符串，可选参数，超参数的标识
      """
  ```