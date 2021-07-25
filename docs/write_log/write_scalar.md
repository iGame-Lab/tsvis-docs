可视化标量数据随时间的变化趋势

  1.单个标量数据

  ```python
  summarywriter.add_scalar(tag: str,
                           scalar_value: Union[float, numpy_compatible],
                           step: Optional[int] = None):
      """
          添加单个标量数据到日志
      Args:
          tag: 字符串，数据标识
          scalar_value: 浮点数，标量的值
          step: 整数，可选参数，记录数据的step
      """
  ```

  2.多个标量数据

  ```python
  summarywriter.add_scalars(tag_scalar_dict: Dict[str, Union[float, numpy_compatible]],
                            step: Optional[int] = None):
      """
          添加一组标量数据到日志
      Args:
          tag_scalar_dict: 字典，(标签, 标量值)
          step: 整数，可选参数，记录数据的step
      """
  ```