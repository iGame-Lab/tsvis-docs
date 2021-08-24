# 后端请求API

## 说明

TS-VIS在设计和开发方式上采用RESTful风格，前后端分离，对外提供的是数据资源的访问接口。在返回的数据格式上，所有的API均采用一下的格式

```json
{
  "code": code, 
  "msg": "", 
  "data": {}
}
```

其中`code`表示返回数据的状态码如下表所示

| 状态码 |            说明            |
| :------: | :------------------------: |
| 200    |          请求成功          |
| 500    | 请求失败，后端发生内部错误 |

`msg`为返回数据的消息字段，用于提示返回成功信息或错误信息，`data`为返回的数据字段。

!!! tip 注意
    为了方便说明，下文中所有的API返回数据格式均为`data`字段中的内容，不包含`code`和`msg`字段

## 初始请求

!!! 资源访问路径
    
    ```
    /api/init
    ```

当用户首次打开TS-VIS前端页面时，需要先访问后端初始化API。该API会通知后端解析用户指定路径的日志文件。
待后端完成全部的日志解析后，则返回：
```
{"code": 200, "msg": "ok", "data": {"msg": "success"}}
```

由于深度学习程序导出的日志体积较大，一直等待解析完成对用户极不友好，所以该接口最多等待30s后会强制返回结果。

## 类目信息

!!! 资源访问路径
    
    ```
    /api/getCategory
    ```

在初始化完成后，需请求类目信息。该接口返回当前可视化日志中所有类目的标签(tag)，若日志中不存在某一类目信息，
则该类目名不会出现在返回数据中。

其中模型结构`graph`的tag信息固定为`s_graph`与`c_graph`，`s_graph`表示结构图而`c_graph`表示结构图；
超参数没有tag，若日志中含有超参数则返回tag值为`true`。

由于统计分析中的分布图(Distribution)是通过直方图(Histogram)数据计算得到的，所以若日志中存在直方图，则必存在分布图。

该接口返回数据格式如下：

```json
{
  "runname1": {
    "scalar": {"scalar": ["scalar_tag1", ...]}, 
    "media": {
      "image": ["image_tag1", ...], 
      "audio": ["audio_tag1", ...], 
      "text": ["text_tag1", ...]
    },
    "statistic": {
      "histogram": ["histogram_tag1", ...],
      "distribution": ["distribution_tag1", ...]
    }, 
    "graph": ["c_graph"],
  },
  "runname2": {
    "scalar": {
      "epoch_loss": ["epoch_loss"], 
      "epoch_accuracy": ["epoch_accuracy"]
    },
    "graph": ["c_graph", "s_graph"],
    "embedding": ["embedding_tag1", ...],
    "hparams": ["true"],
    "custom": ["true"]
  }
}
```

## 模型结构Graph

TS-VIS中支持两种方式的模型结构，一种是包含计算节点的计算图(Calculation graph)，另一种是只有网络结构的结构图(Structer graph)

!!! 资源访问路径
    
    ```
    /api/graph?run={run}&tag={tag}
    ```

其中`run`与`tag`缺一不可。在模型结构中，`tag`的取值固定为`c_graph`和`s_graph`，当`tag=c_graph`时表示请求计算图，而`tag=s_graph`时表示请求结构图

例如请求`/api/graph?run=train&tag=c_graph`表示得到`run=train`下的计算图数据。

该接口返回数据格式如下：

```json
{
  "net": "[{...}]",  //graph图结构数据
  "operator": "[...]" //操作结点分类数据
}
```

## 标量Scalar

!!! 资源访问路径
    
    ```
    /api/scalar?run={run}&tag={tag}
    ```

请求标量数据需要提供参数`run`和`tag`，其中`run`与`tag`缺一不可，这两个参数都分别对应`类目信息`一节中的类目信息的返回。

例如请求`api/scalar?run=.&tag=layer1/weights/summaries/mean`表示得到`run=.`，`tag=layer1/weights/summaries/mean`的标量数据。

该接口返回数据格式如下：

```
{
  "scalar_tag": 
  [
    {"wall_time": 1587176310.3070214, "step": 0, "value": -6.488610961241648e-05}, 
    	                                ..., 
    {"wall_time": 1587176425.348953, "step": 190, "value": -0.002039810409769416}
  ]
}
```

## 图片Image

由于图片属于媒体数据，数据量较大，如果和标量数据一样全部返回会造成前端响应超时，所以图片请求分为两个地址。
首先请求图片的信息

!!! 资源访问路径
    
    ```
    api/image?run={run}&tag={tag}
    ```

其中`run`与`tag`缺一不可，该接口返回指定`run`下`tag={tag}`的图片信息，其中包括日志写入时的时间戳`wall_time`，
当前迭代步数`step`，图片的大小`width`和`height`。该API返回的数据样例为

```json
{
  "image_tag": 
  [
    {"wall_time": 1587176317.1721938, "step": 10, "width": 28, "height": 28},
                                          ...
    {"wall_time": 1587176425.348953, "step": 190, "width": 28, "height": 28}
  ]
}
```

得到图片信息后，再向后端请求图片本身

!!! 资源访问路径
    
    ```
    api/image_raw?step={step}&run={run}&tag={tag}
    ```

其中`step`、`run`与`tag`缺一不可。

例如`api/image_raw?step=0&run=.&tag=input_reshape/input/image/0`
表示访问`step`为0，指定`run`为`.`且`tag`为`input_reshape/input/image/0`的图片数据。
该API返回经过base64编码的图片数据。

## 音频Audio

与图片类似，音频数据量较大，如果和标量数据一样全部返回会造成前端响应超时，所以音频请求分为两个地址。
首先请求音频的信息

!!! 资源访问路径
    
    ```
    api/text?run={run}&tag={tag}
    ```

其中`run`与`tag`缺一不可，该接口返回指定`run`下`tag={tag}`的音频信息，其中包括日志写入时的时间戳`wall_time`，
当前迭代步数`step`，音频属性`label`和音频的格式`contentType`。该API返回的数据的样例为

```json
{
  "waveform/audio_summary":
  [
    {"wall_time": 1587475006.5022004, "step": 1, 
     "label": "<p><em>Wave type:</em> <code>sine_wave</code>. <em>Frequency:</em> 448.98 Hz. <em>Sample:</em> 1 of 1.</p>", 
     "contentType": "audio/wav"
    },
                                        ...
    {"wall_time": 1587475006.7304769, "step": 49, 
     "label": "<p><em>Wave type:</em> <code>sine_wave</code>. <em>Frequency:</em> 880.00 Hz. <em>Sample:</em> 1 of 1.</p>", 
     "contentType": "audio/wav"
    }
  ]
}
```

得到音频信息后，再向后端请求音频本身

!!! 资源访问路径
    
    ```
    api/audio_raw?step={step}&run={run}&tag={tag}
    ```

其中`step`、`run`与`tag`缺一不可。

例如`api/audio_raw?step=0&run=.&tag=waveform/audio_summary`
表示访问`step`为0，指定`run`为`.`且`tag`为`waveform/audio_summary`的音频数据。
该API返回经过base64编码的音频数据。

## 文本Text

!!! 资源访问路径
    
    ```
    api/text?run={run}&tag={tag}
    ```

其中`run`与`tag`缺一不可。该接口返回指定`run`下`tag={tag}`的文本信息，

例如`api/text?run=.&tag=custom_tag`表示访问`tag`为`custom_tag`的文本数据。

该接口返回的数据格式如下

```json
{
  "custom_tag": 
  [
    {"wall_time": 1585807655.373738, "step": 0, 
     "value": "\u8fd9\u662f\u7b2c0\u53e5\u8bdd"},
                      ..., 
    {"wall_time": 1585807656.327519, "step": 99, 
    "value": "\u8fd9\u662f\u7b2c99\u53e5\u8bdd"}
  ]
}
```

值得注意的是，该API返回的文本数据是经过URL编码后的文本

## 直方图Histogram

!!! 资源访问路径
    
    ```
    api/histogram?run={run}&tag={tag}
    ```

请求直方图数据需要提供参数`run`和`tag`，其中`run`与`tag`缺一不可。

例如`api/histogram?run=.&tag=layer1/weights/summaries/histogram/histogram_summary`表示访问
`tag`为`layer1/weights/summaries/histogram/histogram_summary`的直方图数据

该接口返回的数据样例如下

```json
{
  "histogram_tag": 
  [
    [1587176317.1721938, 10, 0.0, 5.297224044799805, 
      [[0.0, 0.17657413482666015, 2801825.0], 
                ...
      [5.120649909973144, 5.297224044799805, 2.0]]
    ],
                      ...
    [1587176425.348953, 190, 0.0, 6.3502936363220215, 
      [[0.0, 0.2116764545440674, 3229943.0],
                ...
      [6.1386171817779545, 6.3502936363220215, 8.0]]
    ]
  ]
}
```

其格式为
```
[
  wall_time, step, min, max, 
  [[left, right, num],
      ...
  [left, right, num]]
]
```

## 分布图Distribution

!!! 资源访问路径
    
    ```
    api/distribution?run={run}&tag={tag}
    ```

请求分布图数据需要提供参数`run`和`tag`，其中`run`与`tag`缺一不可。

例如`api/distribution?run=.&tag=layer1/weights/summaries/histogram/histogram_summary`表示访问
`tag`为`layer1/weights/summaries/histogram/histogram_summary`的分布图数据

该接口返回的数据样例如下

```json
{"distribution_tag":
  [
    [1587176310.3070214, 0, 
      [[0, 0.0], [668, 0.020253705367086237],..., [10000, 4.971314430236816]]],
                              ...,
    [1587176419.1604998, 180,
      [[0, 0.0], [668, 0.021192153914175192],..., [10000, 6.3502936363220215]]]
  ]
}
```

其格式为

```
[
  wall_time, step,
  [[precentage, value],...,[precentage, value]]
]
```

其中`precentage`取值分别为0, 668, 1587, 3085, 5000, 6915, 8413, 9332, 10000 对应标准正态分布的百分位数。

## 高维数据Embeddding

高维数据，由于降维过程较为费时，数据首先在后端进行处理，然后返回降维后的数据。

首先请求某一`tag`对应的`step`和张量维度`shape`

!!! 资源访问路径
    
    ```
    /api/projector?run={run}&tag={tag}
    ```

其中`run`、`tag`缺一不可，该API返回数据样例如下：

```json
{
  "embedding_tag": [0, 1, 2, ... , n],
  "shape": [n, m]
}
```

请求得到该`tag`下embedding数据的所有`step`和维度信息`shape`后，接下来可以请求获取降维后的数据和原始标签

!!! 资源访问路径
    
    ```
    api/projector_data?run={run}&tag={tag}&step={step}&method={method}&dims={dims}
    ```

其中`run`、`tag`、`step`、`method`缺一不可，`method`为降维方法可以从`pca, tsne`中任选其一，`dims`默认为3。

例如`api/projector_data?run=train&tag=outputs&step=0&method=pca&dims=3`表示访问
`run`为`train`下的`tag`为`outputs`的张量降维数据，其中使用的降维方法是`pca`，降维维数是3

该API返回数据样例如下

```json
{
  "0": [
    // 降维后的数据
    [[-1.5627723114617857, -3.9668523435955056, -0.18872563897943656],
     [-1.5627723114617857, -3.9668523435955056, -0.18872563897943656],
     [-1.5627723114617857, -3.9668523435955056, -0.18872563897943656],
                              ...
     [-1.5627723114617857, -3.9668523435955056, -0.18872563897943656]],
    // label信息
    [7,0,5,6,7,1,...,9,6,4]
  ]
}
```

最后还可以请求某一张量对应的训练集中样本的信息

!!! 资源访问路径
    
    ```
    api/projector_sample?run={run}&tag={tag}&index={index}
    ```

其中`run`、`tag`、`index`缺一不可，参数`index`表示请求第几个点对应的样本。

该API返回该张量对应训练集中样本的信息，其类型可以是图片，音频，文本。返回的图片，音频是base64编码的，
而文本是经过URL编码后的。

## 异常Exception

异常数据的请求首先需要得到指定`run`和`tag`下所有的`step`信息

!!! 资源访问路径
    
    ```
    /api/exception?run={run}&tag={tag}
    ```

其中`run`、`tag`缺一不可。

例如`/api/exception?run=train&tag=layer1/weights/Variable:0`表示访问
`run`为`train`下的`tag`为`layer1/weights/Variable:0`的异常数据。

该接口返回异常数据包含的所有`step`，样例如下

```json
{
  "layer1/weights/Variable:0": [0, 1, 2, ... ,n]
}
```

获得到`step`信息后，可以请求指定`run`，`tag`和`step`对应张量平铺后的异常数据（热力图）

!!! 资源访问路径
    
    ```
    api/exception_data?run={run}&tag={tag}&step={step}
    ```

其中`run`、`tag`、`step`缺一不可。

例如`api/exception_data?run=train&tag=layer1/weights/Variable:0&step=0`表示访问`run`为`train`下的
`tag`为`layer1/weights/Variable:0`且`step`为0时的数据。

该API返回数据样例如下

```json
{
  "0":[ 
        [c1,c2],  // 平铺前的数据维度大小(长度不定)
        min,      // 张量中的最小值
        max,      // 张量中的最大值
        mean,     // 张量的平均值
        [[-1.5627723114617857, -3.9668523435955056, -0.18872563897943656],
        [-1.5627723114617857, -3.9668523435955056, -0.18872563897943656],
        [-1.5627723114617857, -3.9668523435955056, -0.18872563897943656],
                                  ...
        [-1.5627723114617857, -3.9668523435955056, -0.18872563897943656]]
      ]
}
```

进一步，获得`step`信息后，可以请求指定`run`，`tag`和`step`对应的异常数据直方图

!!! 资源访问路径
    
    ```
    api/exception_hist?run={run}&tag={tag}&step={step}
    ```

其中`run`、`tag`、`step`缺一不可。

例如`api/exception_hist?run=train&tag=layer1/weights/Variable:0&step=0`表示访问`run`为`train`下的
`tag`为`layer1/weights/Variable:0`且`step`为0时的数据。

该API返回格式如下

```json
{
  "0":
    [
      min,
      max,
      [[left, right, count],
      [left, right, count],
              ...
      [left, right, count]]
    ]
}
```

## 超参数Hyperparam

!!! 资源访问路径
    
    ```
    api/hyperparm?run={run}
    ``

由于每次训练只可能产生一组超参数，在超参数的访问中，只需指定参数`run`即可获得相应的超参数数据。

例如`api/hyperparm?run=hparams`表示访问`run`为`hparams`下的超参数数据，样例如下

```json
{"hparamsInfo": [
  {"3df0d7cf35bec5a33c9fe551db732c24df204d7886b226c5a41cce285d0d4fd5": 
    {
      "hparams": [
          {"name": "num_units", "data": 32.0}, 
          {"name": "optimizer", "data": "sgd"}, 
          {"name": "dropout", "data": 0.2}
        ], 
        "start_time_secs": 1589421877.1109092
    }
  }
], // 超参数信息，可能有多个
 "metrics": [
   {
     "tag": "accuracy", 
     "value": [0.8216999769210815, 0.8241999745368958, 
               0.7746999859809875, 0.765999972820282, 
               0.8411999940872192, 0.8307999968528748, 
               0.7940999865531921, 0.7904999852180481]
    }
  ]
} //超参数的量度，可能有多个。适用于所有的超参数信息
```

该API返回的数据格式如下

```json
{"hparamsInfo": 
  [
    {
      "groupid_1": {
        "hparams": [{"name": 超参数1, "data": 数据1}, ..., {"name": 超参数2, "data": 数据2}], // 多个超参数的名字与值
        "start_time_secs": 开始时间
      }
    }
    ,..., 
    {
      "groupid_n": {
        "hparams": [{"name": 超参数1, "data": 数据1}, ..., {"name": 超参数2, "data": 数据2}], 
        "start_time_secs": 开始时间
      }
    }
  ], // 超参数信息，可能有多个
 "metrics": [
   {"tag": 量度1, "value": [值1, 值2, ...., 值n]}
   ,..., 
   {"tag": 量度n, "value": [值1, 值2, ...., 值n]}
  ] // 超参数的量度，可能有多个。适用于所有的超参数信息
}
```
