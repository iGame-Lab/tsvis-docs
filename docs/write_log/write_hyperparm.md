可视化同一个或多个网络模型在不同超参数组合的训练结果

  ```python
  summarywriter.add_hparams(hparam_dict: Dict[str, Union[bool, str, float, int]],
                            metric_dict: Optional[Dict[str, float]] =None,
                            tag: Optional[str] = None):
      """
          添加一组数据到日志
      Args:
          hparam_dict: 字典，模型的超参数
          metric_dict: 字典，可选参数， 模型的度量数据
          tag: 字符串，可选参数，超参数的标识
      """
  ```