  可视化主流深度学习框架的网络模型，如**tensorflow**，**pytorch**,  **oneflow**等。

  1.主流神经网络模型

  ```python
  summarywriter.add_graph(model,
                          input_to_model: Union[tuple]=None,
                          model_type: str ='pytorch',
                          verbose: bool =False)
      """
          添加神经网络的图结构到日志，支持tensorflow，pytorch
      Args:
          model: 神经网络模型, torch.nn.Module、 tf.Session().graph， oneflow模型请通过onnx或者json格式导出
          input_to_model: 元组, 一组用于模型测试的输入
          model_type: 字符串，模型的类型为‘pytorch’ 或 'tensorflow'
          verbose: 布尔值，pytorch模型是否输出到控制台
      """
  ```
2.onnx网络模型

  ```python
  summarywriter.add_onnx_graph(onnx_model_file：str):
      """
          添加onnx模型到日志
      Args:
          onnx_model_file: 字符串，onnx模型对应的文件路径
      """
  ```

  3.json网络模型 （oneflow）

  ```python
  summarywriter.add_json_graph(model_str: str,
                               name: str = 'model'):
      """
          该功能需要神经网络框架的支持，首先使用内部函数对网络模型的结构进行序列化，然后将序列化的字符串写入到json文件
      Args:
          model_str: 字符串，模型的序列化字符串
          name: 字符串，json文件名
      """
  ```