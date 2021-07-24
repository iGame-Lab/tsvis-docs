可视化神经网络中的样本数据，如图片，音频，视频，文本等。

  **图像**

  1.单张图片

  ```python
  summarywriter.add_image(tag: str,
                          tensor: numpy_compatible,
                          step: Optional[int] = None):
      """
          添加单张图像数据到日志
      Args:
          tag: 字符串，图像的标识
          tensor: 数组，图像数据，'uint8' 或 'float' 类型的数据，大小为(H,W) 或 (H,W,C), 其中C为1,2,3,4
          step: 整数，可选参数，记录数据的step
      """
  ```

  2.多张图片

  ```python
  summarywriter.add_images(tag: str,
                           tensors: numpy_compatible,
                           step: Optional[int] = None):
      """
          添加多个图像数据到日志
      Args:
          tag: 字符串, 图像的标识, 导出过程中将自动转化为tag_1, tag_2, ... , tag_k
          tensors: 数组类型，图像数据，'uint8' 或 'float' 类型的数据，大小为(K,H,W) 或 (K,H,W,C), 其中C为1,3,4
          step: 可选参数，记录数据的step
      """
  ```

  **文本**

  ```python
  summarywriter.add_text(tag: str,
                         text_string: str,
                         step: Optional[int] = None):
      """
          添加文本数据到日志
      Args:
          tag: 字符串，文本的标识
          text_string: 字符串，文本字符串
          step: 整数，可选参数，记录数据的step
      """
  ```

  **音频**

  ```python
  summarywriter.add_audio(tag: str,
                          audio_tensor: numpy_compatible,
                          step: Optional[int],
                          sample_rate: Optional[int] = 44100):
      """
          添加音频数据到日志
      Args:
          tag: 字符串，音频的标识
          audio_tensor: 数组，音频数据，大小为(L, C), 其中L为音频帧的长度，C为通道，通常C=1，2
          step: 整数，可选参数，记录数据的step
          sample_rate: 整数，采样率 Hz
      """
  ```

  **视频**

  ```python
  summarywriter.add_video(tag: str,
                          video_tensor: numpy_compatible,
                          step: Optional[int] = None,
                          fps: Optional[Union[int, float]] = 4):
      """
          添加视频数据到日志
      Args:
          tag: 字符串，视频的标识
          video_tensor: 数组，视频数据
          step: 整数，可选参数，记录数据的step
          fps: 整数，可选参数，视频的帧率， 默认为4
      """
  ```