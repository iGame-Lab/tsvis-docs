# 可视化前端

## 模型结构

由于模型中多个操作涉及节点的删减，设置四个数组expand、delByMenu、delByCondition、reserve分别保存展开的节点、右键菜单功能隐藏的节点、条件过滤的节点、连续使用条件隐藏过程中的节点状态，chageTag标记节点隐藏变化
### 模型结构数据

模型结构数据格式如下：

```json
[
  {
    "uid": ..., 
    "label":...,
    "op": ..., 
    "parent":...,
    "attrs": {...},
    "layer":...,
    "targets":[{"id":...,"info":...,"control":...,"num":...},...],
    "sub_net":[{"uid": ..., 
            "label":...,
            "op": ..., 
            "parent":...,
            "attrs": {...},
            "layer":...,
            "targets":[{"id":...,"info":...,"control":...,"num":...},...],
            "sub_net":[...]},...]
  },
  ...
]
```
uid为节点全名，label为节点简略名，op为节点操作属性，parent为该节点的上级节点全名，attrs为节点属性，layer为节点层级，targets为连边目标节点列表（其中id为目标节点全名，info为边信息，control表示是否为控制边，num为重叠边数量），sub_net为节点下子节点。
### 模型结构绘制

模型结构绘制方法主要使用dagre_d3.js布局库

具体步骤为：

1.遍历模型结构数据,如果节点在expand数组中，将该节点设置为cluster，同时遍历其sub_net内节点；如果节点不在expand数组中，则将节点设置为普通节点，遍历其targets属性内目标节点，如果目标节点存在当前图中，则设置边，根据control和num设置边样式

2.遍历delByMenu数组和delByCondition数组，删除相关节点和边

3.渲染模型结构图

### 节点展开/缩回

展开时将双击的节点加入到expand数组中，清空delByCondition数组和reserve数组，然后重新绘制模型结构

缩回时，从expand数组中删除双击节点及其子孙节点，然后重绘模型结构

### 边隐藏/复原

使用dagre.nodeEdges()查找相关边，对相关边调整样式

### 节点隐藏/复原

节点隐藏：将隐藏的节点加入到delByMenu数组，隐藏相关元素，changeTag置为1

节点复原：delByMenu数组删除复原的节点，changeTag置为0，模型结构重绘

### 条件过滤

具体步骤为：

#### 设置条件

1.graphpanel组件中保存条件语句,数据格式如下：

```json
[
  [ 
    {
      "type":...,
      "tag":...,
      "num":...
    },
    ...
  ],
  ...
]
```

数据格式为两级嵌套，一级嵌套的数据之间关系为或，二级嵌套的数据之间关系为与，其中type为出（入）度，tag为不等式运算符号，num为度数。

#### 隐藏

2.隐藏按键被点击时，graph组件监听到状态变化，步骤一中数据被拼接成字符串：

```
(condition && condition && ...) || (condition && condition && ...) || ...
```

3.将reserve栈顶tag为0的数组全部删除，同时在delByCondition栈中删除相同数量的栈顶元素，复原相关节点和边的样式。然后取出reserve栈顶的数组，遍历数组获取节点出入度，使用eval执行步骤二中的字符串判断节点是否符合条件，符合条件的节点加入一个新建数组，将该新建数组加入到栈delByCondition，构建新的节点状态数组设置tag为0（表示未重绘）并推入栈reserve，隐藏节点和相关边

#### 布局

4.布局按钮被点击时，检查reserve栈顶数组tag与changeTag,如果 (tag==0||changeTag==1) 为true,就重绘结构图，同时将tag置1（表示已重绘），changeTag置0,否则不进行绘制操作

#### 上一步

5.删除reserve栈顶元素，如果元素tag为0，则恢复delByCondition栈顶数组内节点样式以及相关边样式，然后删除delByCondition栈顶数组；如果tag为1，则删除delByCondition栈顶数组，重绘结构图

#### 初始化

6.清空reserve栈和delByCondition栈，重绘结构图



 =================================================================================

## 降维分析

降维分析主要是对数据进行请求, 维护一个请求数据结构 curInfo  是当前模块的状态.

### 降维分析数据

降维分析请求数据结构如下

```json
//  curInfo
  curInfo { // 当前所处于的信息节点
    curTag: '', // 当前选定的tag信息
    curStep: 0, // 当前所处于的step下标
    curMapStep: 0, // 当前映射的step
    curMethod: 'PCA', // 当前的查看方式
    curDim: '3维', // 查看的维度
    received: false // 当前的信息都被填充结束后改变
  }
```


### 降维分析二维绘制

降维分析二维视图绘制方法主要使用d3.js布局库

具体步骤为：

1. 数据绑定

`vm.circleUpdate = gscatter.selectAll('circle').data(localData.data)`

将绘制圆形svg绑定数据

2. 动画绘制

`vm.circleUpdate.merge(vm.circleEnter).transition().ease(d3.easeLinear).duration(1000)`

以动画的方式实现数据的变化

3. 颜色绑定

维护一个颜色标签数组
```
  legendColor: [ // 颜色条因为很多地方都会用到直接放在这里
    '#EF6F38',
    '#EFDD79',
    '#C5507A',
    '#9359B0',
    '#525C99',
    '#47C1D6',
    '#B5D4E8',
    '#15746C',
    '#81c19c',
    '#A08983'
  ]
```


### 降维分析三维绘制

降维分析二维视图绘制方法主要使用echarts.js echarts-gl布局库

1. 数据绑定 & 更新

通过维护 option 这个结构体实现可视化



//=================================================================================

## 超参数分析

超参数分析首先请求后端数据，维护一个state数据。

### 超参数分析数据

超参数分析state数据结构如下

```json
state = {
  categoryInfo: [], 
  allData: '', 
  key: '', 
  items: [], 
  selected: '',
  focusData: [],
  globalSelectedDatas: '',
  axisType: {},
  globalChange: 1,
  hypEmpty: false,
  errorMessage: '',
  mainParams: [],
  axisParams: [] 
}
```

### 超参数分析平行坐标系绘制

平行坐标系绘制方法主要使用d3.js

具体步骤如下：

1. 数轴绘制

   ```js
   dimensions.forEach(function(d) {
           var vals = data.map(function(p) {
             return p[d]
           })
           if (vals.every(quantP)) { //不同数据类型绘制不同的坐标系
            ...
           } else {
             ...
         })
   ```

2. 曲线绘制

   ```js
    const foreground = svg.append('g')
           .attr('class', 'foreground')
           .selectAll('path')
           .data(data)
           .enter().append('path')
           .attr('d', path)
           .attr('stroke', function(d) {
             return colorMap(d[colorItem])
           }).attr('fill', 'none')
           .on('mouseover', function(d, i) {
            ...
           })
           .on('mouseout', function(d, i) {
             ...
           })
   ```

   

3. 坐标轴操作绑定

   ```js
   g2.append('g')
       .attr('class', 'brush')
       .each(function(d) {
       if (_this.items.indexOf(d) <= -1) {
           d3.select(this).call(y[d].brush = d3.brushY().extent([
               [-4, 0],
               [4, height]
           ]).on('brush start', brushStart).on('brush', goBrush).on('brush',  brushParallel).on('end',brushEndOrdinal))
       } else {
           d3.select(this).call(y[d].brush = d3.brushY().extent([
               [-4, 0],
               [4, height]
           ]).on('brush start', brushStart).on('brush', goBrush).on('brush', brushParallelChart).on('end', brushEnd))
       }
   })
    
   ```

   

### 超参数表格绘制

超参数表格绘制使用了element-ui组件

1. 数据绑定 & 更新

   通过维护 data 数据进行可视化



// =================================================================================

## 媒体数据分析

媒体数据分析首先请求后端数据，维护一个state数据。

### 媒体数据分析数据

媒体数据state数据结构如下

```json
state = {
  categoryInfo: '', 
  detailData: '', 
  clickState: false,
  showrun: {},
  totaltag: '',
  freshInfo: {},
  errorMessage: '',
  showFlag: {
    firstTime: true
  }
```


### 媒体数据功能设计

布局三个标签tag，即音频，图像和文本，根据日志内容是否含有该项数据选择是否显示标签。

#### 音频文件

拖动查看不同step音频

```json
formatTooltip(val) {
   if (val === null) {
     return 0
   }
   return this.audiocontent[val]['step']
}
```

下载

```json
downAudio(param) {
   const filename = 'audio.wav'
   fetch(param, {
     headers: new Headers({
       Origin: location.origin
     }),
     mode: 'cors'
   })
     .then(res => res.blob())
     .then(blob => {
       const blobUrl = window.URL.createObjectURL(blob)
       this.download(blobUrl, filename)
       window.URL.revokeObjectURL(blobUrl)
     })
}
```

快进

```json
changeSpeed() {
   const index = this.speeds.indexOf(this.audio.speed) + 1
   this.audio.speed = this.speeds[index % this.speeds.length]
   this.$refs.audio.playbackRate = this.audio.speed
}
```

静音

```json
startMutedOrNot() {
   this.$refs.audio.muted = !this.$refs.audio.muted
   this.audio.muted = this.$refs.audio.muted
}
```

#### 图像文件

查看图片

拖动查看不同step图片

```json
formatTooltip(val) {
   if (val === null) {
     return 0
   }
   return this.imagecontent[val]['step']
}
```

```json
ischeckedLocal() {
   this.checked = !this.checked
   const param = {}
   param['checked'] = this.checked
   param['copyToData'] = false
   param['content'] = this.content
   this.setImageData(param)
}
```

图片放大缩小

```json
scaleLarge() {
   this.scaleLargeSmall = true
   this.size = 24
}
scaleSmall() {
   this.size = 8
   this.scaleLargeSmall = false
}
```



#### 文本文件

拖动查看不同step文本

```json
formatTooltip(val) {
   if (val === null) {
     return 0
   }
   return this.textcontent[val]['step']
}
```
// =================================================================================


## 标量数据分析

标量数据分析首先请求后端数据，维护一个state数据。

### 标量数据分析数据

标量数据state数据结构如下
```json
const state = {
  initshowrun: {},
  categoryInfo: '', // 存放自己的类目信息
  detailData: {}, // 具体全部数据
  clickState: false,
  smoothvalue: 0, // 平滑参数
  yaxis: 'linear', // y轴数据类型
  checkeditem: {}, // 选中的数据
  mergeditem: {}, // 参与合并的数据
  startmerged: false, // 是否开始合并
  endmerged: false, // 是否结束合并
  mergestep: 0, // 合并时执行的步骤
  backstep: 0, // 还原时执行的步骤
  checkedorder: [], // 区分主副图表
  mergednumber: 0, // 合并图表的数量
  backednumber: [], // 还原图表的数量
  cleancheck: false,
  totaltag: {},
  freshInfo: {},
  freshnumber: 0,
  errorMessage: '',
  downLoadArray: [], // 下载svg图暂存id
  grade: {},
  checked: {},
  mergedorder: {},
  showFlag: {},
  subisshow: {},
  IntervalChange: false // 监听定时刷新功能
}
```

### 标量数据展示图绘制

1. 根据加载日志是否含有本类数据显示日志文件类型标签，如loss，mean等数据类型。

2. svg数据图绘制

```json
SvgDraw() {
  // load data
   let data = [].concat(JSON.parse(JSON.stringify(this.data)))
   ...
  // set the dimensions and margins of the graph
   var margin = { top: 20, right: 20, bottom: 70, left: 70 }
   var width = 350 - margin.left - margin.right
   var height = 270 - margin.top - margin.bottom
   ...
   // Add X axis
  const xdomain = d3.extent(data, function(d) { return d.step })
  ...
  // Add Y axis
  const ydomain = d3.extent(data, function(d) { return d.value })
  // append the svg object to the body of the page
  var svg = d3.select('#' + this.classname)
    .append('svg')
    .attr('id', 'svg' + this.classname)
    .attr('width', '100%')
    .attr('height', '100%')
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .attr('viewBox', '0 0 350 270')
    .append('g')
    .attr('transform',
    'translate(' + margin.left + ',' + margin.top + ')')
    ...
}
```

3. 平滑度设置
```json
smoothvalue: function() {
   if (this.grade[this.classname] === 'main') {
     if (this.mergetype === 'single') {
       d3.select('#svg' + this.classname).remove()
       this.MergeSvgDraw()
     } else if (this.mergetype === 'double') {
       d3.select('#svg' + this.classname).remove()
       this.DYMergeSvgDraw()
     }
   } else {
     d3.select('#svg' + this.classname).remove()
     this.SvgDraw()
   }
}
```

4. y轴变换
```json
yaxis: function() {
   if (this.grade[this.classname] === 'main') {
     if (this.mergetype === 'single') {
       d3.select('#svg' + this.classname).remove()
       this.MergeSvgDraw()
     } else if (this.mergetype === 'double') {
       d3.select('#svg' + this.classname).remove()
       this.DYMergeSvgDraw()
     }
   } else {
     d3.select('#svg' + this.classname).remove()
     this.SvgDraw()
   }
}
```

5. 合并函数
```json
start: function(val) {
   if (val) {
     if (Object.keys(this.mergeditem[this.classname]).length === 1) {
       this.mergetype = 'single'
       this.legendnumber = this.checkedorder.length
       this.mergeddata = [].concat(this.mergeditem[this.classname][Object.keys(this.mergeditem[this.classname])[0]])
       d3.select('#svg' + this.classname).remove()
       this.MergeSvgDraw()
     } else if (Object.keys(this.mergeditem[this.classname]).length === 2) {
       this.mergetype = 'double'
       this.legendnumber = this.checkedorder.length
       this.mergeddata0 = [].concat(this.mergeditem[this.classname][Object.keys(this.mergeditem[this.classname])[0]])
       this.mergeddata1 = [].concat(this.mergeditem[this.classname][Object.keys(this.mergeditem[this.classname])[1]])
       d3.select('#svg' + this.classname).remove()
       this.yname0[0] = Object.keys(this.mergeditem[this.classname])[0]
       this.yname0[1] = 'log(' + this.yname0[0] + ')'
       this.yname1[0] = Object.keys(this.mergeditem[this.classname])[1]
       this.yname1[1] = 'log(' + this.yname1[0] + ')'
       this.DYMergeSvgDraw()
     }
     this.setmergestep()
   }
}
```

6. 还原函数
```json
end: function(val) {
   if (val) {
     this.deleteScalar(this.thisid)
     this.mergeddata = []
     this.mergeddata0 = []
     this.mergeddata1 = []
     this.mergetype = ''
     this.yname0 = []
     this.yname1 = []
     this.data = this.chartdata.value[Object.keys(this.chartdata.value)[0]]
     this.yname[0] = this.ytext
     this.yname[1] = 'log(' + this.ytext + ')'
     d3.select('#svg' + this.classname).remove()
     this.SvgDraw()
   }
}
```
// =================================================================================


## 注意力分析

### 注意力分析数据结构

注意力分析state数据结构如下
```json
state = {
  categoryInfo: "",
  sentences: [],
  allData: {},
  attention: {},
  defaultFilter: "all",
  bidirectional: true,
  displayMode: "light",
  defaultLayer: 0,
  defaultHead: 0,
  errorMessage: "",
  attentionInfoData: [],
  attentionLayers: [],
  selectedLH: "00-00",
  selectImgTag: "",
  selectLayer: [],
  selectX: "0",
  selectY: "0",
  selectG: "1",
  selectR: "1",
  originImg: "",
  attnMap: {},
  layerNumber: 0,
  chartWidthScale: 1,
  chartHeightScale: 1
}
```

### 注意力分析功能划分
通过监听日志解析transformer文件下是否含有特定标识来判断是否包含文本或者图像注意力数据文件，再分别进行渲染绘制。

### 文本注意力

### 统计信息表绘制
通过向后端请求注意力数据allData，再进行封装处理，维护attentionInfoData，使用Element-UI组件进行绘制。

### 统计信息图绘制
通过向后端请求注意力数据allData，再进行封装处理，维护attentionInfoData，使用d3.js进行环形图的绘制。
```json
drawRadialBar (data) {
  ...
  const container = svg.append('g')
    .attr('class', 'container')
    .attr('transform', `translate(${width / 2},${height / 2})`)
    .style('font-size', 10)
    .style('font-family', 'sans-serif');

  container
    .selectAll('path')
    .data(data)
    .join('path')
    .style('fill', d => color(d.vari))
    .style('stroke', d => color(d.vari))
    .attr('d', arc)
    .on("mouseover", mouseover)
    .on("mousemove", mousemove)
    .on("mouseleave", mouseleave)
    .on("click", mouseclick);

  container.append('g')
    .call(xAxis);

  container.append('g')
    .call(yAxis);
  ...
}
```

### AttentionVis绘制
通过向后端请求注意力数据allData，再进行封装处理，通过d3.js绘制折线图及网格图展示单个注意力头的具体内容。
```json
render () {
  ...
  this.renderText(attentionLines, leftText, "leftText", posLeftText);
  this.renderAttentionLines(attentionLines, posAttention, posRightText);
  this.renderText(attentionLines, rightText, "rightText", posRightText);
  ...
  this.renderMatrix(svg, filterData);
  ...
}

renderText (svg, text, id, leftPos) {
  ...
  wordContainer
    .append("rect")
    .classed("highlight", true)
    .attr("fill", fillColor)
    .style("opacity", 0.0)
    .attr("height", TEXTBOXHEIGHT)
    .attr("width", TEXTBOXWIDTH)
    .attr("x", leftPos)
    .attr("y", function (d, i) {
      return i * TEXTBOXHEIGHT + TEXTBOX_HEIGHT - 1;
    });

  ...

  let textContainer = wordContainer
    .append("text")
    .classed("token", true)
    .text(function (d) {
      return d;
    })
    .attr("font-size", TEXT_SIZE + "px")
    .style("fill", this.getColor("text"))
    .style("cursor", "default")
    .style("-webkit-user-select", "none")
    .attr("x", leftPos + offset)
    .attr("y", function (d, i) {
      return i * TEXTBOXHEIGHT + TEXTBOX_HEIGHT;
    })
    .attr("height", TEXTBOXHEIGHT)
    .attr("width", TEXTBOXWIDTH)
    .attr("dy", TEXT_SIZE);
    
  ...
}


renderAttentionLines (svg, start_pos, end_pos) {
  ...

  attentionContainer
    .selectAll("g")
    .data(attentionMatrix)
    .enter()
    .append("g")
    .classed("attention-line-group", true)
    .attr("source-index", function (d, i) {
      return i;
    })
    .selectAll("line")
    .data(function (d) {
      return d;
    })
    .enter()
    .append("line")
    .attr("x1", start_pos)
    .attr("y1", function (d) {
      let sourceIndex = +this.parentNode.getAttribute("source-index");
      return sourceIndex * TEXTBOXHEIGHT + TEXTBOX_HEIGHT + TEXTBOXHEIGHT / 2;
    })
    .attr("x2", end_pos)
    .attr("y2", function (d, targetIndex) {
      return targetIndex * TEXTBOXHEIGHT + TEXTBOX_HEIGHT + TEXTBOXHEIGHT / 2;
    })
    .attr("stroke-width", 2)
    .attr("stroke", this.getColor("attn"))
    .attr("stroke-opacity", function (d) {
      return d;
    })
}


renderMatrix (svg, filterData) {
  ...

  for (let i = 0; i < axisData.length; i++) {
    axisSpace.push(i * MATRIX_CELL_WIDTH);
  }

  ...

  xAxisBox
    .attr(
      "transform",
      `translate(${yAxisWidth + (MATRIX_CELL_WIDTH - 2) / 2 + 80},${xAxisHeight})`
    )
    .call(xAxis)
    .selectAll("text")
    .attr("fill", "#000")
    .style("font-size", "12")
    .style("font-weight", "500")
    .style("text-anchor", "start")
    .attr("dx", "0.7em")
    .attr("dy", "0.4em")
    .attr("transform", "rotate(-65)");
  
  ...

  for (let i = 0; i < matrixData.length; i++) {
    for (let j = 0; j < matrixData[0].length; j++) {
      let fill = this.getMatrixColor(matrixData[i][j], maxAttn, minAttn);
      svg
        .append("g")
        .append("rect")
        .attr("width", MATRIX_CELL_WIDTH - 2 + "px")
        .attr("height", MATRIX_CELL_WIDTH - 2 + "px")
        .attr("fill", "#" + fill)
        .attr("x", yAxisWidth + j * MATRIX_CELL_WIDTH + "px")
        .attr("y", xAxisHeight + i * MATRIX_CELL_WIDTH + "px")
        .attr("id", `attention_${i}_${j}`)
        .attr("transform", `translate(80, 0)`)
    }
  }

  ...
}
```
// =================================================================================


## 隐状态分析

### 隐状态分析数据结构

隐状态分析state数据结构如下
```json
state = {
  errorMessage: "",
  stateRun: "",
  stateTag: "",
  sentenceTag: "hidden_state_word",
  stateData: null,
  sentence: [],
  rightWordsLength: 0,
  maxValue: 1,
  minValue: -1,
  pos: 0,
  range: 27,
  threshold: 0,
  selectedLineIndexs: [],
  selectedRange: [],
  signature: "",
  stateMatchData: []
};
```

### 隐状态分析功能划分

#### 折线区域绘制
通过向后端请求隐状态数据，通过前端数据组装成符合绘制要求的数据模式，通过d3.js绘制折线图。

1. 折线区域绘制函数
```json
drawStateVisBox () {
  ...
  this.drawValueLines(main, this.jointData, lineGenerator, lineOpacity);
  ...
}

drawValueLines (container, data, lineGenerator, lineOpacity) {
  const valueLines = container.selectAll('.valueLine').data(data);
  valueLines.exit().remove();

  return valueLines
    .enter()
    .append("path")
    .merge(valueLines)
    .attr("class", d => "valueLine id_" + d.dim)
    .attr("d", d => lineGenerator(d.value))
    .style("opacity", lineOpacity);
}
```   

2. 文本区域绘制函数
```json
drawStateVisAxis () {
  ...
  wordCellList
    .attr("class", d => `word noselect word_${d.index}`)
    .attr('transform', (d, i) => `translate(${xScale(i) + 35}, 0)`);
  wordCellList.select('rect')
    .attr("y", 0)
    .attr("width", 30)
    .attr("height", 24);
  wordCellList.select('text')
    .text(d => d.text)
  ...
}
``` 

3. 滑动brush绘制函数
通过d3.brushX绘制x轴方向上的区域选择刷，通过d3.drag更新严格小于区域的绘制。
```json
renderBrush (brush, bottomBrush, xScale) {
  ...
  const brushing = () => {
    ...
  }

  const brushStart = () => {
    ...
  }

  const brushEnd = () => {
    ...
  }

  const brushX = d3.brushX()
    .extent([[xScale.range()[0], 4], [xScale.range()[1], 20]])
    .on("start", brushStart)
    .on("brush", brushing)
    .on("end", brushEnd)

  ...

  this.renderBottomBrush(bottomBrush, xScale);
}

renderBottomBrush(bottomBrush, xScale){
  const dragging = () => {
    ...
  }

  const dragEnd = () => {
    ...
  }

  ...
  zHandles.enter().append("circle").attr("class", d => "zeroDeco zeroHandle handle-" + d.handle)
    .merge(zHandles)
    .attr("cx", d => xScale(d[d.handle]))
    .attr("cy", 15)
    .attr("r", 5)
    .call(d3.drag()
      .on("drag", dragging)
      .on("end", dragEnd)
    )
  ...
}
```  

4. 匹配函数
通过监听匹配模式的变化，点击匹配按钮向后端获取匹配的数据。
```json
startMatch () {
  const param = {
    run: this.getStateRun,
    tag: this.getStateTag,
    threshold: this.threshold,
    pattern: this.getSignature,
    length: this.getRange
  }
  this.fetchStateMatchData(param);
}

watchValueChange () {
  const lineIndexs = [];

  if (this.getSelectedRange.length && this.bottomBrushSelection.length) {
    const leftBound = this.getSelectedRange[0] - this.bottomBrushSelection[0];
    const rightBound = this.getSelectedRange[1] + this.bottomBrushSelection[1];

    let signature = this.selectedRangeIndexs(leftBound, rightBound)
      .map(v => (v >= this.getSelectedRange[0] && v < this.getSelectedRange[1]) ? 1 : 0)
      .join('');

    this.setSignature(signature);

    this.jointData.forEach((lineData, index) => {
      const testSignature = lineData.value.slice(leftBound, rightBound)
        .map(v => (v.item >= this.getThreshold) ? 1 : 0)
        .join('');
      if (testSignature === this.getSignature) lineIndexs.push(index);
    })
  }
  this.setSelectedLineIndexs(lineIndexs);
  this.drawStateVisBox();
}
```