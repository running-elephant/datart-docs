---
title: 自定义插件化图表
---

> 本章节主要描述了插件图表的核心概念、插件化图表加载的过程、详细的各项配置说明以及一个简单的例子作为补充说明。

## 1 图表插件化
### 1.1 核心概念
**插件化图表**：是一种“可插拔”使用的`自定义图表`，通过加载插件化图表使产品的使用更加灵活，增强软件的扩展性。同时，插件化提供了标准的输入接口，可以灵活适配由不同`WEB绘图引擎`渲染的图形，摆脱了传统的可视化BI产品在定制图表时只能通过二次开发方式的弊端。

**自定义图表**：是一种在官方提供的图表范围之外的图表制作方式。其原理是，在图表可“插拔”的基础上，Datart通过提供`图表生命周期函数`、`图表工具函数`、`图表参数设置表单配置项`、`图表元信息配置项` 以及`图表上下文信息`等资源辅助进行图表的制作，方便快速开发基于Javascript渲染引擎的图表，满足不同种类的、更专业的图表需求，**降低开发周期和准入门槛**。

**WEB绘图引擎**：这里专指基于**浏览器**运行环境的Javascript绘图类库以及图表组件库等，典型代表如`D3JS`和`EChartJS`。

**图表参数设置表单**: 通过配置相关数据及样式，方便图表绘图时对`图表数据集`进行处理以提高图表的扩展性。如数据配置项可对`数据视图`中的数据列进行操作，可聚合、格式化信息显示、别名以及着色等功能。样式配置项，记录用户的自定义图表样式配置，官方提供基础的表单组件，用户通过自定义JSON格式的表单配置项，来扩展`图表参数设置表单`以达到满足不同种类需求的目的，请参考本文[<<参考配置项详细说明>>](#4-参考配置项详细说明)章节。


### 1.2 插件化图表时序图

> 插件化图表的加载是在项目启动的时候完成的，首先通过请求获取服务端`custom-chart-plugins`文件内的所有文件，然后加载符合要求的插件图表，然后即可在`图表编辑器`以及其他模块中使用。

![插件图表加载时序图](/datart-docs/images/chart_plugin/load-plugin-chart-sequence-diagram.png)

> Note: 请将制作好的自定义插件图表保存在服务端的`custom-chart-plugins`文件内，并刷新网页重新登陆系统即可，文件名不要和其他文件冲突，并且确保`图表元信息配置项`中的**id**的唯一性。


## 2 自定义图表

### 2.1 核心概念

> 在插件化图表的架构支持下，图表的制作者只需关注如何呈现某图表的核心业务逻辑，通过Datart可视化软件将行业数据根据图表进行丰富的展示。从本质上讲，Datart在图表展现层是作为一个绘图引擎和图表数据集的“适配器”，**兼收并蓄**是Datart插件化的核心理念，最终达到业务人员也可以制作出图表的目的。

一个最基本的自定义图表由以下几部分组成：
- `图表生命周期函数`
- `图表工具函数`
- `图表参数设置表单配置项`
- `图表元信息配置项`
- `图表上下文信息`
- `图表数据集`

**图表生命周期函数**: Datart图表回调函数，辅助绘制及更新图表。
- OnMount：负责图表初始化，可初始化`WEB绘图引擎`相关资源，以及保存实例对象
- OnUpdated：负责在`图表数据集`、`图表参数设置`和`图表上下文信息`等数据更新响应，可根据最新的数据更新图表的显示。
- OnResize：负责在图表**容器**宽度或高度变化时响应，可根据最新的宽度和高度更新视图显示。
- OnUnMount: 负责**回收**绘图引擎自身的**非托管资源**，如调用EChart实例的[dispose][EChart Dispose]方法可对EChart资源进行回收，避免内存溢出。

**图表工具函数**: 基于Datart的`图表数据集`以及`图表参数配置项`的工具函数集。

**图表参数设置表单配置项**: 本质上是一个`表单生成器`的语法定义部分，是`图表参数设置表单`的配置项，该配置项包含基于`数据视图`的`数据配置`、`样式配置`项以及`其他设置`的配置项。基于Datart预定义的`基础表单组件`和`图表工具函数`中读取表单配置的工具方法，就可以组合出**任何功能**的图表配置表单。

**基础表单组件**：预定义的“单元”表单组件，包含布局组件、常规组件、基于`数据配置`的扩展组件等。通过，在`图表参数设置表单配置项`中指定属性comType来生成对应的组件，如指定comType为line，则会生成对应的线条设置组件。

**表单生成器**：通过定义表单描述文件来生成表单组件。Datart提供了一系列的`表单语法`和`基础表单组件`来生成`图表参数设置表单配置项`，使用户可在图表编辑界面进行配置。

**图表元信息配置项**: 包含图表名称、图标信息、资源依赖等。（目前，暂时还没有将依赖项合并入`图表元信息配置项`中，将会在下一个版本中更新）

**图表上下文信息**: 包含当前运行期间的资源，包含当前工作环境（IFrame内部）Document、Window对象，图表宽度高度等信息。

**图表数据集**：这里专指Datart通过请求返回给前端的图表当前条件下的数据集合，这里强调的当前条件指的是满足`图表参数设置表单`中数据配置项的逻辑的数据集合。


> 更详细配置说明，请参考本文[<<参考配置项详细说明>>](#4-参考配置项详细说明)章节。

### 2.2 自定义图表关系图

![图表组件关系图](/datart-docs/images/chart_plugin/custom-chart-structure-diagram.png)

## 3 自定义图表示例

> 需求定义: 制作一个散点图，包含一个或多个的维度列数据，两个指标列数据作为散点图的X、Y轴。

完整代码及说明如下：	

```javascript
/**
 * Datart
 *
 * Copyright 2021
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

function D3JSScatterChart({ dHelper }) {
  return {
    //【可选】扩展配置图表功能，可配合`数据视图`对数据处理
    config: {
      datas: [
        {
          label: 'metrics',
          key: 'metrics',
          required: true,
          type: 'group',
        },
        {
          label: 'deminsion',
          key: 'deminsion',
          required: true,
          type: 'aggregate',
        },
      ],
      styles: [
        {
          label: 'common.title',
          key: 'scatter',
          comType: 'group',
          rows: [
            {
              label: 'common.color',
              key: 'color',
              comType: 'fontColor',
            },
          ],
        },
      ],
      i18ns: [
        {
          lang: 'zh-CN',
          translation: {
            common: {
              title: '散点图配置',
              color: '气泡颜色',
            },
          },
        },
        {
          lang: 'en',
          translation: {
            common: {
              title: 'Scatter Setting',
              color: 'Bubble Color',
            },
          },
        },
      ],
    },

    isISOContainer: 'demo-d3js-scatter-chart',
    
    //【必须】加载D3JS绘图引擎，此处需给出CDN链接或者服务端相对资源地址即可
    dependency: ['https://d3js.org/d3.v5.min.js'],

    //【必须】设置图表的基本信息，icon可从Datart Icon图标中选取，暂时不支持自定义
    meta: {
      id: 'demo-d3js-scatter-chart',
      name: '[Plugin Demo] D3JS 散点图',
      icon: 'sandiantu',
      requirements: [
        {
          group: [0, 999],
          aggregate: 2,
        },
      ],
    },

    //【必须】Datart提供的生命周期函数，其他周期如onUpdated，onResize以及onUnMount
    onMount(options, context) {
      if (!context.document) {
        return;
      }

      const host = context.document.getElementById(options.containerId);
      var margin = { top: 10, right: 40, bottom: 30, left: 30 },
        width = context.width - margin.left - margin.right,
        height = context.height - margin.top - margin.bottom;

      // 初始化D3JS绘图区域
      this.chart = context.window.d3
        .select(host)
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')');
    },

    onUpdated(options, context) {
      if (!options.dataset || !options.dataset.columns || !options.config) {
        return;
      }

      // 获取当前绘图区域的宽高
      const clientWidth = context.window.innerWidth;
      const clientHeight = context.window.innerHeight;
      const margin = { top: 40, right: 40, bottom: 40, left: 40 };
      const width = clientWidth - margin.left - margin.right;
      const height = clientHeight - margin.top - margin.bottom;

      // 获取散点图数据及配置
      const { data, style } = this.getOptions(options.dataset, options.config);

      // 绘制基于百分比的横纵坐标轴散点图, 以下是D3JS绘图逻辑
      var x = context.window.d3
        .scaleLinear()
        .domain([0, 100]) // This is the min and the max of the data: 0 to 100 if percentages
        .range([0, width]); // This is the corresponding value I want in Pixel

      this.chart
        .append('g')
        .attr('transform', 'translate(0,' + height + ')')
        .call(context.window.d3.axisBottom(x));

      // X scale and Axis
      var y = context.window.d3
        .scaleLinear()
        .domain([0, 100]) // This is the min and the max of the data: 0 to 100 if percentages
        .range([height, 0]); // This is the corresponding value I want in Pixel

      this.chart.append('g').call(context.window.d3.axisLeft(y));

      // Add 3 dots for 0, 50 and 100%
      this.chart
        .selectAll('whatever')
        .data(data)
        .enter()
        .append('circle')
        .attr('cx', function (d) {
          return x(d.x);
        })
        .attr('cy', function (d) {
          return y(d.y);
        })
        .style('fill', style.color)
        .attr('r', 7);

      this.chart.selectAll('whatever').style('color', 'blue');
    },

    getOptions(dataset, config) {
      // 当前服务端返回的图表数据集
      const dataConfigs = config.datas || [];

      // 获取样式配置信息
      const styleConfigs = config.styles;
      const groupConfigs = dataConfigs
        .filter(c => c.type === 'group')
        .flatMap(config => config.rows || []);

      // 获取指标类型配置信息
      const aggregateConfigs = dataConfigs
        .filter(c => c.type === 'aggregate')
        .flatMap(config => config.rows || []);

      // 数据转换，根据Datart提供了Helper转换工具
      const objDataColumns = dHelper.transfromToObjectArray(
        dataset.rows,
        dataset.columns,
      );

      const data = objDataColumns.map(dc => {
        return {
          x: dc[dHelper.getValueByColumnKey(aggregateConfigs[0])],
          y: dc[dHelper.getValueByColumnKey(aggregateConfigs[1])],
        };
      });

      var xMinValue = Math.min(...data.map(o => o.x));
      var xMaxValue = Math.max(...data.map(o => o.y));

      var yMinValue = Math.min(...data.map(o => o.y));
      var yMaxValue = Math.max(...data.map(o => o.y));

      // 获取用户配置
      const color = dHelper.getStyleValueByGroup(
        styleConfigs,
        'scatter',
        'color',
      );

      return {
        style: {
          color,
        },
        data: data.map(d => {
          return {
            x: ((d.x || xMinValue - xMinValue) * 100) / (xMaxValue - xMinValue),
            y: ((d.y || yMinValue - yMinValue) * 100) / (yMaxValue - yMinValue),
          };
        }),
      };
    },
  };
}
```

最终效果图如下：

![D3JS散点图最终效果图](/datart-docs/images/chart_plugin/d3js-scatter-chart.png)

## 4 参考配置项详细说明

### 4.1 图表生命周期函数说明
![图表生命周期状态图](/datart-docs/images/chart_plugin/chart-lifecycle-state-diagram.png)

#### 4.1.1 onMount: 
> 初始化图表事件，function(options, context) => void

- options
  - containerId: string
  - dataset: [ChartDataset]
  - config: [ChartConfig]
- context
  - document: [HTML Document]
  - window: [HTML Window]
  - width: string | number
  - height: string | number

#### 4.1.2 onUpdated
> 更新图表事件，function(options, context) => void

- options
  - containerId: string
  - dataset: [ChartDataset]
  - config: [ChartConfig]
- context
  - document: [HTML Document]
  - window: [HTML Window]
  - width: string | number
  - height: string | number

#### 4.1.3 onResize
> 图表容器大小变更事件，function(options, context) => void

- options
  - containerId: string
  - dataset: [ChartDataset]
  - config: [ChartConfig]
- context
  - document: [HTML Document]
  - window: [HTML Window]
  - width: string | number
  - height: string | number

#### 4.1.4 onUnMount
> 销毁图表事件，function() => void

### 4.2 图表工具函数说明
[DHelper][Chart Util]: Datart工具函数, 从自定义图表函数中获取

#### 4.2.1 getStyleValue
> 从样式配置树对象中获取`paths`组合的路径值， function(styleConfigs, paths) => any

- styleConfigs: [ChartStyleSectionConfig] []
- paths: string[]

#### 4.2.2 getColumnRenderName
> 从[ChartDataSectionField]中获取当前数据列的显示名称，function(field) => string

- field: [ChartDataSectionField]

#### 4.2.3 getValueByColumnKey
> 获取当前数据列的唯一值，function(col) => string

- col: [ChartDataSectionField]

#### 4.2.4 valueFormatter
> 获取当前数据列的显示格式， function(config, value) => string

- config: [ChartDataSectionField]
- value: string | number

### 4.3 图表参数设置表单配置项说明
> 该配置项，需要在图表的config属性中设置。

- config: [ChartConfig]
  - datas: ChartDataSectionConfig[]
  - styles: ChartStyleSectionConfig[]
  - settings: ChartStyleSectionConfig[]
  - i18ns: ChartI18NSectionConfig[]


#### 4.3.1 ChartDataSectionConfig
> 图表配置之数据列配置，包含维度、指标、过滤、颜色、信息等，通过指定section type来显示不同作用的组件。

- type: [ChartDataSectionType], 
  - group: 维度类型组件，在当前组件中的数据列会进行数据分组操作。
  - aggregate： 指标类型组件，在当前组件中的数据列会进行聚合操作。
  - mixed: 混合类型组件，目前对于字符型（离散型）数据列会进行分组操作，对数字类型（连续型）数据列会进行聚合操作
  - filter：过滤器组件，对当前组件中数据列进行数据过滤操作。
  - info：信息类型组件，对当前类型的数据列进行聚合操作，数据不以图形的方式显示，只会在`图表提示信息`上显示
  - color: 颜色设置组件，对当前组件中的数据列进行`第二次分组`操作，并按分组后设置的颜色显示在图形上
- maxFieldCount：number，可选项，当前组件中数据列的最大数量
- allowSameField：boolean, 可选项，当前组件中是否允许相同的数据列
- required：boolean, 可选项，用于标识当前组件中的数据列是否是图形绘制的必要数据列
- rows: [ChartDataSectionField] [], 当前组件中的数据列集合
- actions：[ChartDataSectionFieldActionType] [], 设置当前组件允许的数据列操作，如别名、着色、聚合、大小等操作。

#### 4.3.2 ChartStyleSectionConfig
> 当前对象类型为[ChartStyleSectionRow]，这是一个树形嵌套的结构，本身即是ChartStyleSectionRow类型，同时含有rows属性作为递归的子元素集合。

- label: string, 显示的名称，可结合国际化翻译配置，显示不同的名称。
- key: string，组件值查找的唯一ID
- default?: any，默认值
- value?: any，当前值
- disabled?: boolean，是否禁用组件
- hide?: boolean，是否隐藏
- options?: [ChartStyleSectionRowOption]，组件扩展属性
- watcher?: [ChartStyleSectionRowWatcher]，组件属性变更依赖函数类型
- template?: [ChartStyleSectionRow]，组件模版
- comType: [ChartStyleSectionComponentType], `基础表单组件`
- rows: [ChartStyleSectionConfig] [], 当前组件的子组件集合

#### 4.3.3 ChartI18NSectionConfig
> 图表I18N国际化，配置信息，Datart有全局国际化文件，也可以在图表中单独在此处设置国际化翻译。

- lang: string, 国际化语言标识，如zh，us
- translation：object，当前语言标识下的翻译属性。例如，在[ChartStyleSectionConfig]中的label属性中设置“common.label”时，则程序首先会在translation的common属性下label属性中读取，读取后会显示在组件外观中。
```javascript
// Step 1. 首先定义一个国际化设置
i18ns: [
  {
    lang: 'zh',
    translation: {
      common: {
        title: '散点图配置',
        color: '气泡颜色',
      },
    },
  },
  {
    lang: 'en',
    translation: {
      common: {
        title: 'Scatter Setting',
        color: 'Bubble Color',
      },
    },
  },
],
...
// Step 2. 然后，假设我们设置了一个group类型的布局组件，其中包含了一个color类型的子组件
// Step 3. 当设置了label为"common.title"时，显示的名称会根据当前的语言环境设置读取响应的值，
// 例如中文时，color组件显示的名称为‘气泡颜色’，当英文环境时，显示的为‘Bubble Color’
{
  label: 'common.title', // 此处设置国际化文件对象查找路径
  key: 'scatter',
  comType: 'group',
  rows: [
    {
      label: 'common.color',
      key: 'color',
      comType: 'fontColor',
    },
  ],
},
```


### 4.4 基础表单组件说明
> 在程序中会映射成原子组件、布局组件、自定义组件等。如需要增加基础组件，请在`ItemLayoout.tsx`文件中注册。

#### 4.4.1 ChartStyleSectionComponentType
> `基础表单组件`的枚举类型

原子组件：
- checkbox：单选框类型组件
- input：输入框类型组件
- switch：开关类型组件
- select：单选下拉框组件
- font：字体设置组件
- inputNumber：数字类型输入框组件
- slider：滑块组件

布局组件：
- group：集合类型布局组件
- tabs：tab类型布局组件

自定义组件：
- cache：缓存设置相关组件
- reference：图表参考线、参考区间组件
- tableHeader：表格中的表格设置组件

#### 4.4.2 ChartStyleSectionRowOption
> `基础表单组件`的扩展设置属性，不同表单组件支持的属性不同。

- min?: number | string，inputNumber和slider类型组件中支持的最小值设置
- max?: number | string，inputNumber和slider类型组件中支持的最大值设置
- step?: number | string，inputNumber和slider类型组件中支持的步进大小设置
- type?: [ItemComponentType], 'modal', 布局类型容器是否作为弹窗模式
- editable?: boolean，是否可编辑
- modalSize?: string，弹窗大小设置
- expand?: boolean，布局类型容器是否自动展开
- items?: [ChartStyleSelectorItem] [], 当前列表值集合
- hideLabel?: boolean，是否隐藏标签显示
- style?: 组件css样式设置
- getItems?: function(cols) => [ChartStyleSelectorItem] [], 获取当前组件列表值函数，如果设置了`items`则当前函数设置无效
- needRefresh?: boolean，当组件值发生改变时，是否重新根据设置发起网络数据请求


### 4.5 图表元信息配置项说明
> [ChartMetadata], 元信息配置，主要包含图表的id，名称，依赖信息以及图表渲染约束设置等内容。

- id: 图表唯一值
- name: 图表显示名称
- icon: string, fonticon
- requirements: ChartRequirement[], 图表绘制约束类型，分`GROUP`和`AGGREGATE`类型

_**注意，将在接下来的版本中，将其移入到`图表元信息配置项`中**_
- dependency: string[], 图表依赖的第三方资源，可以是CDN地址，也可以是服务器相对资源地址。
- isISOContainer: string, 对于某些含有相同依赖的图表而言，设置相同的`isISOContainer`的值，可共享依赖资源，避免多次的网络加载请求。

#### 4.5.1 ChartRequirement
> `GROUP`和`AGGREGATE`可分别设置各自所需的数据列数目，作为图表绘制的约束条件。数字`999`代表不设置上限。

```javascript
{
  group: [0, 999],
  aggregate: [0, 999],
},
```


[//]: # (Refrence Links Below)


[EChart Dispose]: <https://echarts.apache.org/en/api.html#echartsInstance.dispose>

[ChartConfig]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L349

[ChartDataset]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartDataset.ts#L19


[HTML Document]: https://developer.mozilla.org/en-US/docs/Web/API/Document

[HTML Window]: https://developer.mozilla.org/en-US/docs/Web/API/Window

[Chart Util]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/utils/chart.ts#L18

[ChartStyleSectionConfig]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L298

[ChartDataSectionField]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L195

[ChartDataSectionConfig]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L276

[ChartI18NSectionConfig]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L344

[ChartDataSectionType]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L288

[ChartDataSectionFieldActionType]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L140

[ChartStyleSectionRow]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L305

[ChartStyleSectionComponentType]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L169

[ChartStyleSectionRowOption]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L318

[ChartStyleSectionRowWatcher]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L339

[ChartStyleSelectorItem]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartConfig.ts#L333 

[ChartMetadata]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartMetadata.ts#L21

[BrokerContext]: https://github.com/running-elephant/datart/blob/01bf976dc3ad80f100ccb71dab35f4a961587f0c/frontend/src/app/pages/ChartWorkbenchPage/models/ChartEventBroker.ts#L28







