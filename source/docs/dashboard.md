---
title: 仪表板
---

仪表板是 datart 可视化的核心部分，它通常用于呈现一组具有相关性的数据图表，帮助使用者从多角度掌握关键信息；同时它还具有：

- 可预设的交互能力，支持使用者对图表数据进行切片、钻取
- 展示图文等媒体信息的能力，帮助使用者更好地理解与洞察数据

在视觉上构成仪表板的每一个元素被称为“组件”，数据图表、文字、图片、控制器都是不同类型的组件

在主导航栏点击`可视化`菜单，点击`目录`顶部的加号按钮创建仪表板

![新建](/datart-docs/images/dashboard/create.jpg)

# 1. 布局类型

仪表板拥有自动、自由两种布局类型

![布局](/datart-docs/images/dashboard/layout.png)

## 1.1 自动布局

![](/datart-docs/images/dashboard/auto.gif)

在调整组件尺寸和位置时，其他组件会适应该组件变化进行流式布局

仪表板可视区域宽度在 768 像素以上时组件会按照用户定义的比例进行显示，在 768 像素以下时会响应为移动端观看模式

## 1.2 自由布局

![](/datart-docs/images/dashboard/free.gif)

自由布局可以用来制作大屏类应用，可以设定面板具体尺寸，任意调整组件大小，和位置。并且组件可以上下重叠。

# 2. 制作仪表板

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

## 2.1 组件

#### 组件概述

仪表板内由多个组件(widget)组成.

组件有多种类型，目前有

1. 图表组件(chart-widget),
2. 媒体组件(media-widget),
3. 容器组件(container-widget),
4. 筛选组件(filter-widget)等。

未来还会扩展其他类型的组件等。

#### 组件设计

![](/datart-docs/images/dashboard/widgetSet.png)

##### 组件名称

![](/datart-docs/images/dashboard/widgetName.png)

可以修改组件的名称，控制组件名称的显示，不显示。

设置组件的名称颜色。

##### 组件位置

在自由布局模式可以精确设置组件的位置。

##### 组件尺寸

在自由布局模式可以精确设置组件的宽高。

##### 组件背景

组件背景可以设置，组件的背景色，以及背景图片

##### 组件内边距

设置组件的内编辑，也可以理解为 widget 内的 padding 值

##### 组件边框

组件边框可以设置其边框的显示隐藏，边框的粗细，边框的样式，边框的颜色,边框的圆角

![](/datart-docs/images/dashboard/widgetBorder.png)

##### 组件自动刷新

组件可以设置是否开启自动刷新，以及设置自动刷新的频率，开启后默认 60 秒刷新一次数据。

#### 图层列表

![](/datart-docs/images/dashboard/widgetList.jpg)

图层列表显示仪表板内所有组件的 列表。每个列表项右侧的编辑按钮，可以对组件进行删除，编辑等操作。也可以拖拽改变组件图层的值。或者改变顶部过滤器的先后顺序。

![](/datart-docs/images/dashboard/tuceng.gif)

### 2.1.1 图表组件

![](/datart-docs/images/dashboard/twoChart.jpg)

公共图表

公共图表指得是在文件目录可见的，独立图表(dataChart)，它可以被多个仪表板引入使用。

私有图表

是指在仪表板内创建的只属于当前仪表板的私有图表，不能被其他的仪表板使用。

### 2.1.2 媒体组件

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

### 2.1.3 容器组件

容器组件，顾名思义这类组件是一个容器，可以放置其他的组件。

目前你可以在仪表板上添加一个 tab 容器组件。

tab 容器(container-widget)

可以添加 Tab 控件，每个 Tab 控件可添加多个标签页，每个标签页内可放置多张图表，可灵活切换标签页进行展示，其 tab 控件的制作过程和自由布局的 tab 制作过程相同。

双击 tab 组件，进入 tab 编辑模式。点击上面`+` 按钮。可以看到提示，将其他组件拖进去即可。删除的时候，点击上面的`X`按钮，将组件移出。

![](/datart-docs/images/dashboard/tab.gif)

### 2.1.4 筛选组件

<img src="/datart-docs/images/dashboard/filter.gif" style="zoom:150%;" />

![](/datart-docs/images/dashboard/filter.jpg)

上图是一个典型的筛选组件配置面板，

1. 首先选择你需要筛选的组件(可多选)，因为组件本身可能会有自身的筛选，你在这里可以选择是否接管(覆盖)组件自身的筛选条件(让原来组件自身的筛选条件失效)。
2. 选择要关联的组件以后，下面会展示选择的组件背后关联的数据视图，你可以选择这个筛选组件需要关联的数据视图的哪个字段或者变量，当然这个字段可能是（字符型，日期型，或者数值型）其中的一种。
   1. 接下来做相对应的控制器配置。

#### 字符型

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

#### 数值型

![](/datart-docs/images/dashboard/filter_num_jh.jpg)

![](/datart-docs/images/dashboard/filter_num_sx.jpg)

![](/datart-docs/images/dashboard/filter_num_kzq.png)

当选择的字段是 number 数值型的时候，对应的`聚合方式`如上图，有平均值，最大值，最小是等等。

`筛选方式` 如上图可以选择 区间或者 大于某值 小于某值

`控制器` 可以选择 滑块 或者数值等。

#### 日期型

![](/datart-docs/images/dashboard/filter_Date_1.png)

当你选择的字段是 日期类型的时候

`筛选方式` 可选常规 如上图 可以选择一个相对时间 例如你选择了`今天` 筛选区的筛选值 就是每当程序运行的当天。举例来讲，当你设置好`今天` 以后每次程序运行，它都会实时计算出当天的 0 点 0 分到当天 23 点 59 分的值，并不确指某一天。

除了选择常规相对日期，还可以进行自定义设置

![](/datart-docs/images/dashboard/filter_date_zdy.png)

`自定义`设置 可以选择一个时间范围， 选择一个开始时间，一个结束时间，这两个时间既可以是精确时间，也可以是一个程序运行时的相对时间。

## 2.2 操作

### 前进/后退

当用户 不小心误删除了组件或者 其他误操作，都可以点击撤销按钮来回退操作， 重做按钮是和撤销按钮对应的操作。

当用户在点击保存按钮之前。都是可以撤销重做的。

![](/datart-docs/images/dashboard/redo.gif)

### 删除

像上图演示的 可以对组件进行删除操作，在保存之前，都可以对删除的组件进行恢复。

### 复制/粘贴/

利用复制粘贴按钮，可以轻松地复制一个组件

复制出来的组件和原来的组件不会相互干扰。

![](/datart-docs/images/dashboard/copy.gif)

### 置顶/置底

在自由布局中 可以对组件进行图层置顶 和置底操作。

![](/datart-docs/images/dashboard/toTop.gif)

也可以通过右侧图层列表进行编辑

![](/datart-docs/images/dashboard/toTop2.gif)

## 2.3 交互设置

图表组件可以在右侧配置面板的“交互”栏设置交互行为；目前支持设置 3 种交互行为：联动、跳转和查看数据

### 2.3.1 联动

联动是以图表组件作为筛选控制器，点击图表元素，使其关联的其他图表组件筛选条件发生变化、并重新查询数据

![](/datart-docs/images/dashboard/linkage1.gif)

选中图表组件，在右侧配置面板点击“交互”栏，勾选左侧选框启用联动，点击标题打开配置面板

![](/datart-docs/images/dashboard/linkage2.png)
![](/datart-docs/images/dashboard/linkage3.png)

需要配置以下内容：

1. 交互事件：触发联动交互的事件

- 左键单击：鼠标左键点击触发
- 右键菜单：打开鼠标右键菜单，通过选择菜单中的选项触发

2. 关联图表组件设置

首先，在列表里勾选上期望联动的图表组件；然后在关系设置里配置字段关系，关系设置有 2 个选项：

- 自动：如果参与联动交互的两个图表组件数据来源于同一个数据视图，关系可设为自动。在触发联动交互时，会将作用在触发器图表上的所有筛选条件，全部传递给拥有相同数据视图的关联图表组件
- 自定义：以下情况适合设置自定义关系

  1. 参与联动交互的两个图表组件数据来源于不同数据视图；
  2. 来源于同一个数据视图，但希望在联动时仅传递部分字段的筛选条件

  在这两种情况下，需要配置触发器与关联图表之间的字段关系，可以配置多条。在触发联动交互时，会将满足配置关系的筛选条件传递到关联图表

### 2.3.2 跳转

图表组件的跳转交互配置与使用与独立图表一致，请参考[图表跳转配置](datachart#1-5-1-跳转)和[图表交互行为](datachart#2-1-图表交互)

### 2.3.3 查看数据

图表组件的查看数据交互配置与使用与独立图表一致，请参考[图表查看数据配置](datachart#1-5-2-查看数据)和[图表交互行为](datachart#2-1-图表交互)

# 3. 使用仪表板

## 3.1 刷新

单个组件刷新

![](/datart-docs/images/dashboard/reload.png)

## 3.2 全屏

组件可以全屏展示

![](/datart-docs/images/dashboard/full.gif)

并且可以切换全屏展示的组件

![](/datart-docs/images/dashboard/full2.gif)

## 3.3 分享链接

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

## 3.4 导出数据

可以将仪表板数据导出为 Excel，或是将仪表板截图导出为图片文件或 PDF

![](/datart-docs/images/dashboard/download.png)

### 3.4.1 导出到 Excel

当用户下载的仪表板中包含多个组件时，将分不同 sheet 页存储各个组件的明细数据。

整个下载过程是异步的；点击下载按钮后，服务端生成下载任务，用户可以在主导航栏查看下载任务列表，当任务处理完毕之后，可以点击文件名称下载 Excel 文件

![](/datart-docs/images/dashboard/download3.png)

### 3.4.2 导出到图片和 PDF

导出为图表和 PDF 需要在部署阶段完成[截图配置](index#3-2-5-截图配置)后才能正常使用。使用方式和导出 Excel 一样，点击按钮之后生成下载任务，任务处理完成之后，在主导航栏的下拉列表中点击下载图片或 PDF 文件

## 3.5 导出与导入模板

可以将仪表板配置导出为 `.drt`（datart template file）模板文件，并支持在任意 datart 服务导入模板文件来创建仪表板

### 3.5.1 导出模板

点击扩展按钮中的“导出为模板”选项，会弹出样例数据编辑面板；datart 会将样例数据存放到模板中供图表展示使用，在该面板中用户可以给样例数据脱敏，或是替换新的样例数据。完成之后，点击确定执行导出任务

![](/datart-docs/images/dashboard/template1.png)
![](/datart-docs/images/dashboard/template2.png)

导出任务执行完成后，在主导航栏的下载列表下载模板文件

![](/datart-docs/images/dashboard/template3.png)

### 3.5.2 导入模板

点击可视化目录顶部的新建按钮，选择“导入模板”，在弹出的表单中输入名称，选择模板文件和路径，点击确定，即可完成导入操作

![](/datart-docs/images/dashboard/template4.png)
![](/datart-docs/images/dashboard/template5.png)

通过导入模板新建的仪表板中所展示的是样式数据，如果需要替换为数据视图中的真实数据，还需要手动匹配一下。

打开图表编辑界面，可以看到数据栏中的配置字段呈警告色，说明该项配置与数据视图不匹配。选择数据视图，在弹出的对话框中选择“保留”

![](/datart-docs/images/dashboard/template6.png)
![](/datart-docs/images/dashboard/template7.png)

数据配置项会按照字段名称自动匹配，匹配成功的字段会正常显示，未匹配成功的字段依然呈警告色

![](/datart-docs/images/dashboard/template8.png)

点击数据栏中显示警告色的字段，在下拉菜单中替换为数据视图中的字段，这样就可以保留原始配置信息并切换到数据视图的真实数据了

![](/datart-docs/images/dashboard/template9.png)
![](/datart-docs/images/dashboard/template10.png)

# 4. 基础功能

## 4.1 新建&编辑

### 4.1.1 新建&编辑仪表板

仪表板支持新建和编辑操作

![](/datart-docs/images/dashboard/edit.png)

### 4.1.2 新建&编辑文件夹

![](/datart-docs/images/dashboard/wenjianjia.png)

## 4.2 移至回收站

在目录上每行右侧点击，可以修改其基本信息，或者将其移到回收站。

![](/datart-docs/images/dashboard/huishouzhan.jpg)

## 4.3 还原

点击下图进入回收站目录,可以将回收站里面的资源，还原或者彻底删除。

![](/datart-docs/images/dashboard/huishouzhan2.jpg) ![](/datart-docs/images/dashboard/huishouzahn3.jpg)

## 4.4 删除

如上图所示。

## 4.5 搜索

点击目录上的 🔍 放大镜 图表，就会出现搜索输入框，输入您要搜索的资源，就会有对应的显示。

![](/datart-docs/images/dashboard/sousuo.png)
