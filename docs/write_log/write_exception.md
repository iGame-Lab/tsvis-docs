可视化各网络层的权重，偏置，梯度，输出的异常情况。主要以直方图，箱线图，颜色映射矩阵等方式进行展示

  ```python
  summarywriter.add_exception(tag: str,
                              tensor: numpy_compatible,
                              step: Optional[int] = None):
      """
          添加监测的异常数据到日志
      Args:
          tag: 字符串，待监测的异常数据的标识
          tensor: 数组，待监测的异常数据
          step: 整数，可选参数，记录数据的step
      """
  ```