---
title: 第一个可视化作品
---

我们先来快速浏览一下怎样使用 datart 制作一个可视化作品。如果想要深入了解 datart 中的具体功能，可以在之后的章节里寻找对应介绍

## 注册与登录

首次访问系统需要注册账号，点击登录页右下角`注册账号`进入注册页

![登录](/datart-docs/images/first-visualization/1-1-1.png)
![注册](/datart-docs/images/first-visualization/1-1-2.png)

如已配置[邮箱服务器](index#3-3-邮件服务配置-可选)，在注册成功后需要确认激活邮件才能进入系统；如未配置在注册完成之后可以立即登入系统

![注册邮件](/datart-docs/images/first-visualization/1-1-3.png)

## 创建数据源

进入系统之后，首页是可视化目录的展示页面，主导航栏在左侧

![首页](/datart-docs/images/first-visualization/2-1-1.jpg)

制作可视化作品首先需要一个数据源，点击主导航栏上的`数据源`菜单来创建一个 `MySQL` 数据源，输入名称、选择数据源类型并填写所需的参数，点击`测试连接`，测试成功之后就可以保存了

![数据源](/datart-docs/images/first-visualization/2-1-2.png)

## 创建数据视图

然后点击主导航栏上的`数据视图`菜单，来编辑一个数据视图。点击数据视图右上角的加号按钮，选择`新建数据视图`， 中部的编辑器会创建一个新的页签

![数据视图-1](/datart-docs/images/first-visualization/3-1-1.jpg)

在编辑器工具栏中选择刚刚创建的数据源，编写SQL，完成之后点击工具栏上的`执行`按钮，就可以在下方看到执行结果

![数据视图-2](/datart-docs/images/first-visualization/3-1-2.jpg)

可以点击执行结果列头部的按钮来编辑字段类型和权限，编辑完成后，点击右上角的保存按钮保存数据视图

![数据视图-3](/datart-docs/images/first-visualization/3-1-3.jpg)

## 创建数据图表

数据视图创建完成之后，就可以制作可视化作品了。数据图表是最基础的可视化作品，点击主导航栏的`可视化`菜单进入可视化目录，并点击目录右上角的加号进行创建；我们先创建一个公共数据图表

![数据图表-1](/datart-docs/images/first-visualization/4-1-1.png)

创建完成后，在目录上选中刚创建的数据图表，右侧区域会显示该图表，由于刚创建还没有内容，点击右上角的编辑按钮进行编辑

![数据图表-2](/datart-docs/images/first-visualization/4-1-2.jpg)

进入编辑界面后，在左上角选择刚才创建的数据视图，出现字段之后，将字段拖拽到中间的`数据`栏中来制作图表

![数据图表-3](/datart-docs/images/first-visualization/4-1-3.jpg)

在中间的`样式`栏中可以编辑图表的显示样式

![数据图表-4](/datart-docs/images/first-visualization/4-1-4.jpg)

点击右上角保存，一张简单的公共数据图表就制作好了

![数据图表-5](/datart-docs/images/first-visualization/4-1-5.jpg)

## 创建仪表板

下面我们来创建一个仪表板，和数据图表的步骤一样，先点击目录右上角的加号，在目录上创建一个仪表板，仪表板可以选择`自动`和`自由`两种布局模式，不同的布局模式适合不同的使用场景；我们先创建一个自动布局的仪表板，同样，创建完成后在目录上选中它，然后在右侧区域点击编辑按钮来进行编辑

![仪表板-1](/datart-docs/images/first-visualization/5-1-1.jpg)

在编辑界面，我们可以将刚刚创建的公共数据图表添加到仪表板中来，也可以直接在仪表板内创建数据图表

![仪表板-2](/datart-docs/images/first-visualization/5-1-2.jpg)

按照上个段落介绍的方式，再创建几个数据图表，这样一个简单的仪表板就制作出来了

![仪表板-3](/datart-docs/images/first-visualization/5-1-3.jpg)

我们可以在左侧目录上选择刚创建的数据图表和仪表板来预览，同时可以在右侧区域顶部点击页签来快速切换

![仪表板-4](/datart-docs/images/first-visualization/5-1-4.jpg)

## 创建故事板

接下来我们创建一个故事板，来演示刚刚创建的可视化作品。点击左上角的`演示`菜单，目录会切换为故事板列表，点击加号创建一个故事板，和仪表板一样，创建完成后在右侧点击编辑按钮来制作故事板

![故事板-1](/datart-docs/images/first-visualization/6-1-1.jpg)

点击左上角的加号按钮添加刚才创建的仪表板，并设置过渡动画，一个故事版就做好了。点击播放按钮，就可以播放刚才制作好的故事版了

![故事板-2](/datart-docs/images/first-visualization/6-1-2.jpg)
