---
title: 仪表板
---

仪表板是 datart 可视化功能中最重要的部分；在仪表板中，我们通过拖拽即和设置可生成一份具有交互的报告或者大屏，

![widget](/datart-docs/images/dashboard/widget.png)

在主导航栏点击`可视化`菜单，点击`目录`顶部的加号按钮创建仪表板

![新建](/datart-docs/images/dashboard/create.jpg)

datart 仪表板提供两种布局类型(`自动布局`,`自由布局`)。帮助用户快速打造可视化报表

![布局](/datart-docs/images/dashboard/layout.png)

## 1. 布局类型

### 1.1 自动布局

![](/datart-docs/images/dashboard/auto.gif)

在调整组件尺寸和位置时，其他组件会适应该组件变化进行流式布局

仪表板可视区域宽度在 768 像素以上时组件会按照用户定义的比例进行显示，在 768 像素以下时会响应为移动端观看模式

### 1.2 自由布局

![](/datart-docs/images/dashboard/free.gif)

自由布局可以用来制作大屏类应用，可以设定面板具体尺寸，任意调整组件大小，和位置。并且组件可以上下重叠。

## 2. 制作仪表板

仪表板编辑栏

![](/datart-docs/images/dashboard/editBar.jpg)

1. 添加图表(chart)组件，可以选择添加公共数据图表，或者仪表板私有图表

2. 添加媒体组件

3. 添加容器组件

4. 添加全局筛选组件

5. 组件图层上移（自由布局面板中）

6. 组件图层下移（自由布局面板中）

7. 撤销操作

8. 重做操作

9. 删除组件

10. 复制组件

11. 粘贴组件

面板设计

![](/datart-docs/images/dashboard/boardset.png)

上图是 自由布局的面板设计。

1. 可以设置面板的具体尺寸。默认为 1920\*1080。
2. 可以设置面板的缩放模式，['等比宽度缩放','等比高度缩放','全屏铺满','实际尺寸']
3. 可以设置整个面板的背景颜色，以及背景图片

![](/datart-docs/images/dashboard/boardset2.png)

上图是自动布局的面板设计

1. 因为是自动布局，所以面板的基础属性由面板接管，属性包括画布边距，画布组件间距等。
2. 画布的背景设计

### 2.1 组件

##### 组件概述

仪表板内由多个组件(widget)组成.

组件有多种类型，目前有

1. 图表组件(chart-widget),
2. 媒体组件(media-widget),
3. 容器组件(container-widget),
4. 筛选组件(filter-widget)等。

未来还会扩展其他类型的组件等。

##### 组件设计

![](/datart-docs/images/dashboard/widgetSet.png)

###### 组件名称

![](/datart-docs/images/dashboard/widgetName.png)

可以修改组件的名称，控制组件名称的显示，不显示。

设置组件的名称颜色。

###### 组件位置

在自由布局模式可以精确设置组件的位置。

###### 组件尺寸

在自由布局模式可以精确设置组件的宽高。

###### 组件背景

组件背景可以设置，组件的背景色，以及背景图片

###### 组件内边距

设置组件的内编辑，也可以理解为 widget 内的 padding 值

###### 组件边框

组件边框可以设置其边框的显示隐藏，边框的粗细，边框的样式，边框的颜色,边框的圆角

![](/datart-docs/images/dashboard/widgetBorder.png)

###### 组件自动刷新

组件可以设置是否开启自动刷新，以及设置自动刷新的频率，开启后默认 60 秒刷新一次数据。

##### 图层列表

![](/datart-docs/images/dashboard/widgetList.jpg)

图层列表显示仪表板内所有组件的 列表。每个列表项右侧的编辑按钮，可以对组件进行删除，编辑等操作。也可以拖拽改变组件图层的值。或者改变顶部过滤器的先后顺序。

![](/datart-docs/images/dashboard/tuceng.gif)

#### 2.1.1 图表组件

![](/datart-docs/images/dashboard/twoChart.jpg)

公共图表

公共图表指得是在文件目录可见的，独立图表(dataChart)，它可以被多个仪表板引入使用。

私有图表

是指在仪表板内创建的只属于当前仪表板的私有图表，不能被其他的仪表板使用。

#### 2.1.2 媒体组件

![](/datart-docs/images/dashboard/medialist.png)

- 图片组件
- 富文本组件
- 时间器组件
- iframe 组件
- 视频组件

当前有五种媒体组件

图片组件可以作为一个图片容器

![](/datart-docs/images/dashboard/image.gif)

富文本组件

双击富文本组件进入富文本编辑模式，编辑完成后，点击其他地方，退出编辑模式

![](/datart-docs/images/dashboard/richText.png)

时间器组件

时间器 的编辑 可以双击组件或者 右上角编辑按钮。时间格式默认是 YYYY-MM-DD HH:mm:ss ,可以设置为`YYYY-MM-DD`或者 `HH:mm:ss`都是可以的。更多地格式可以了解 [momentjs](http://momentjs.cn/docs/#/parsing/string-format/)

![](/datart-docs/images/dashboard/time.png)

iframe 组件

像其他的组件一样点击编辑，或双击进入编辑模式，可以输入网址，例如输入 `https://ant.design/index-cn` 网页就出现了

![](/datart-docs/images/dashboard/iframe.png)

视频组件

视频的编辑和 iframe 很类似，你可以输入一个视频的地址。

#### 2.1.3 容器组件

容器组件，顾名思义这类组件是一个容器，可以放置其他的组件。

目前你可以在仪表板上添加一个 tab 容器组件。

tab 容器(container-widget)

可以添加 Tab 控件，每个 Tab 控件可添加多个标签页，每个标签页内可放置多张图表，可灵活切换标签页进行展示，其 tab 控件的制作过程和自由布局的 tab 制作过程相同。

双击 tab 组件，进入 tab 编辑模式。点击上面`+` 按钮。可以看到提示，将其他组件拖进去即可。删除的时候，点击上面的`X`按钮，将组件移出。

![](/datart-docs/images/dashboard/tab.gif)

#### 2.1.4 筛选组件

<img src="/datart-docs/images/dashboard/filter.gif" style="zoom:150%;" />

![](/datart-docs/images/dashboard/filter.jpg)

上图是一个典型的筛选组件配置面板，

1. 首先选择你需要筛选的组件(可多选)，因为组件本身可能会有自身的筛选，你在这里可以选择是否接管(覆盖)组件自身的筛选条件(让原来组件自身的筛选条件失效)。
2. 选择要关联的组件以后，下面会展示选择的组件背后关联的数据视图，你可以选择这个筛选组件需要关联的数据视图的哪个字段或者变量，当然这个字段可能是（字符型，日期型，或者数值型）其中的一种。
   1. 接下来做相对应的控制器配置。

##### 字符型

![](/datart-docs/images/dashboard/filter_str_changgui.png)

字符型筛选方式有三种

第一种 常规筛选，可以选择一个数据视图的字段值，下面会列出数据库的这个字段的可选项，也就是这个过滤器的可选项，可以从中选择一个或者多个做为筛选器的默认值。

第二种 自定义筛选

![](/datart-docs/images/dashboard/filter_str_zdy.png)

自定义筛选方式，用户可以自己编写筛选器的可选项，点击按钮`添加一条备选项`即可添加，在表格上点击对应行列进行修改。也可以 点击`从字段填充备选项`进行快速填充，并可以修改对应的值以及标签名。

第二种 条件筛选

![](/datart-docs/images/dashboard/filter_str_tj.jpg)

条件筛选用户可以 选择一种逻辑关系，例如上图，筛选数据里面等于`石家庄`的对应项

`是否显示`

可以设置这个过滤器的可见性，有 显示,隐藏和条件。如下图 (图-2.1.4)

此时当仪表板的另一个`地区筛选器`的值等于`河北`的时候，才让这个城市筛选器显示出来。

![图-city](/datart-docs/images/dashboard/filter_str_city.png)

**`控制器`**

字符型可以设置 下拉列表，多选下拉列表，单选按钮 等控制器的类型

**`宽度`**

设置这个过滤器组件在仪表板上的宽度 ，只当这个过滤器的位置为顶部固定时候 有效。

**`位置`**

当选择`自由编辑`的时候，这个过滤器如同仪表板的其他类型的组件一样，可以进行拖拽，位置大小自由编辑。

当选择`顶部固定`的时候，这个过滤器固定在 自动布局仪表板的头部。宽度可以进行设置。

![](/datart-docs/images/dashboard/filter_str_2.jpg)

设置完成后 效果如上图

##### 数值型

  ![](/datart-docs/images/dashboard/filter_num_jh.jpg)

  ![](/datart-docs/images/dashboard/filter_num_sx.jpg)

  ![](/datart-docs/images/dashboard/filter_num_kzq.png)

  当选择的字段是 number 数值型的时候，对应的`聚合方式`如上图，有平均值，最大值，最小是等等。

  `筛选方式` 如上图可以选择 区间或者 大于某值 小于某值

  `控制器` 可以选择 滑块 或者数值等。

##### 日期型

  ![](/datart-docs/images/dashboard/filter_Date_1.png)

  当你选择的字段是 日期类型的时候

  `筛选方式` 可选常规 如上图 可以选择一个相对时间 例如你选择了`今天` 筛选区的筛选值 就是每当程序运行的当天。举例来讲，当你设置好`今天` 以后每次程序运行，它都会实时计算出当天的 0 点 0 分到当天 23 点 59 分的值，并不确指某一天。

  除了选择常规相对日期，还可以进行自定义设置

  ![](/datart-docs/images/dashboard/filter_date_zdy.png)

  `自定义`设置 可以选择一个时间范围， 选择一个开始时间，一个结束时间，这两个时间既可以是精确时间，也可以是一个程序运行时的相对时间。

### 2.2 操作

#### 前进/后退

当用户 不小心误删除了组件或者 其他误操作，都可以点击撤销按钮来回退操作， 重做按钮是和撤销按钮对应的操作。

当用户在点击保存按钮之前。都是可以撤销重做的。

![](/datart-docs/images/dashboard/redo.gif)

#### 删除

像上图演示的 可以对组件进行删除操作，在保存之前，都可以对删除的组件进行恢复。

#### 复制/粘贴/

利用复制粘贴按钮，可以轻松地复制一个组件

复制出来的组件和原来的组件不会相互干扰。

![](/datart-docs/images/dashboard/copy.gif)

#### 置顶/置底

在自由布局中 可以对组件进行图层置顶 和置底操作。

![](/datart-docs/images/dashboard/toTop.gif)

也可以通过右侧图层列表进行编辑

![](/datart-docs/images/dashboard/toTop2.gif)

### 2.3 联动设置

联动效果

![](/datart-docs/images/dashboard/linkage1.gif)

仪表板中的组件之间可以配置联动关系

联动设置,点击组件右上角进入编辑联动

![](/datart-docs/images/dashboard/linkage2.png)

如下图可以把`地区天气`这个饼图组件作为联动的触发器，勾选需要它触发联动的组件，我们勾选了三个组件。

其中`chart_widget12`和`widget_chart`这两个组件和 触发器`地区天气`组件背后关联的是同一个数据视图所以不用做其他配置。

而`widget_渠道`这个组件背后的数据视图和触发器的数据视图不一样，所以我们要手动的制定一下他们数据视图相关字段的对应关系。比如我们让数据视图 `gdp-view`的【地区】对应 数据视图 `渠道数据`的【name_level2】。

![](/datart-docs/images/dashboard/linkage3.jpg)

当你的组件设置了联动，并**_开启_**了它，那么你的组件上会有一个可以联动的(标记)icon。开始联动以后，点击这个 icon 可以取消联动效果

![](/datart-docs/images/dashboard/linkage5.jpg)

### 2.4 跳转设置

效果预览。点击图表上的值，到转到其他的仪表板或者图表

![](/datart-docs/images/dashboard/jumpset0.gif)

组件右上角编辑按钮，进行跳转设置，跳转和联动都响应了鼠标的点击事件，所以同一个图表，跳转和联动同一时间只能设置一个。

![](/datart-docs/images/dashboard/jumpset1.png)

选择你要跳转的目标，可以是仪表板或者公共图表等。注意，要选择的目标要至少有一个(筛选器)filter。否则不能选为目标。如下图

![](/datart-docs/images/dashboard/jumpset.jpg)

## 3. 使用仪表板

### 3.1 刷新

单个组件刷新

![](/datart-docs/images/dashboard/reload.png)

### 3.2 全屏

组件可以全屏展示

![](/datart-docs/images/dashboard/full.gif)

并且可以切换全屏展示的组件

![](/datart-docs/images/dashboard/full2.gif)

### 3.3 分享链接

![](/datart-docs/images/dashboard/share.png)

在预览模式右上角，点击扩展按钮，会有分享链接和下载数据 的按钮。

点击分享链接

![](/datart-docs/images/dashboard/share1.png)

1 选择一个分享链接有效的截止日期。

2 可以选择启用密码，也可以不启用，根据你的需求来选择。

3 点击生成链接

4 点击右侧按钮完成对链接的复制，在新的浏览器地址栏输入这个复制的链接，即可。分享页的数据权限与分享者一致。

如下图,打开的一个分享页面。URL 携带了 share 前缀。

![分享页](/datart-docs/images/dashboard/shareview.jpg)

下图是一个分享启用密码的一个展示

![分享带密码](/datart-docs/images/dashboard/share_width_pw.jpg)

在新的浏览器打开链接如下图所示，需要你填写密码。

![share](/datart-docs/images/dashboard/share_view_pw.jpg)

### 3.4 下载

用户可以下载仪表板和可视化组件的明细数据 Excel 文件.

当用户下载的仪表板中包含多个组件时，将分不同 sheet 页存储各个组件的明细数据

整个下载过程是异步的；点击下载按钮后，服务端生成下载任务，用户可以通过点击屏幕右上角云状按钮查看下载任务列表，当任务处理完毕之后，可以点击文件名称下载 Excel 文件

下载功能和上图的分享链接功能类似

![](/datart-docs/images/dashboard/download.png)

点击会询问你是否下载，确认后.左侧出现可下载的提示

![](/datart-docs/images/dashboard/download1.png)

点击打开下载任务列表，点击对应的项目即可进行下载

![](/datart-docs/images/dashboard/download3.png)

## 4. 基础功能

### 4.1 新建&编辑

#### 4.1.1 新建&编辑仪表板

仪表板支持新建和编辑操作

![](/datart-docs/images/dashboard/edit.png)

#### 4.1.2 新建&编辑文件夹

![](/datart-docs/images/dashboard/wenjianjia.png)

### 4.2 移至回收站

在目录上每行右侧点击，可以修改其基本信息，或者将其移到回收站。

![](/datart-docs/images/dashboard/huishouzhan.jpg)

### 4.3 还原

点击下图进入回收站目录,可以将回收站里面的资源，还原或者彻底删除。

![](/datart-docs/images/dashboard/huishouzhan2.jpg) ![](/datart-docs/images/dashboard/huishouzahn3.jpg)

### 4.4 删除

如上图所示。

### 4.5 搜索

点击目录上的 🔍 放大镜 图表，就会出现搜索输入框，输入您要搜索的资源，就会有对应的显示。

![](/datart-docs/images/dashboard/sousuo.png)
