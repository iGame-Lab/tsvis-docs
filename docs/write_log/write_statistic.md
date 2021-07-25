可视化数据的分布情况，如直方图，分布图。其中分布图由直方图转化得到

  ```python
  summarywriter.add_histogram(tag: str,
                              tensor: numpy_compatible,
                              step: Optional[int] = None,
                              max_bins: Optional[int] = None):
      """
          添加直方图数据到日志
      Args:
          tag: 字符串，直方图的标识
          tensor: 数组，直方图的原始数据
          step: 整数，可选参数，记录数据的step
          max_bins: 整数，可选参数，直方图划分的区间个数
      """
  ```