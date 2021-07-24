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



// =================================================================================

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



// =================================================================================

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

