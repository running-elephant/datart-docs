---
title: 故事板
---

好的可视化作品可以传达精准有效的信息，但在向观众展示或讲述一组有相关性的可视化作品时，往往需要通过合理的方式来组织和呈现，用于描述上下文、以及表达可视化作品之间的关系。

故事板是一组可视化作品的集合，制作者通过编排顺序、播放模式、动画效果来更好地呈现这组可视化作品，帮助观众更加容易地理解数据。

故事板通常被用于以下场景：

- 将可视化作品组织成一个演示文稿，帮助“叙述故事”；
- 制作一个轮播大屏，用于静态展示。

![story](/datart-docs/images/storyboard/story.gif)

## 1. 新建故事板

新建一个故事板（storyBoard）

![新建](/datart-docs/images/storyboard/create.jpg)

找到 `演示` 菜单入口，然后点击下方`+`按钮 弹出 新建故事版对话框，输入一个名字，点击保存，就得到一个空白的故事版。如下图。

![toEdit](/datart-docs/images/storyboard/toEdit.jpg)

点击编辑按钮进入故事板编辑器

![block](/datart-docs/images/storyboard/block.png)

可以看到一个空白的故事板如上图所示

1. 故事板名称
2. 添加故事页按钮
3. 故事页切入动画效果
4. 故事页切出动画效果
5. 切换效果的快慢设置
6. 是否开启自动播放
7. 自动播放时每页的停留时间（秒）
8. 编辑好，点击完成，进行保存。

## 2. 添加一个故事页

点击 添加故事页按钮 会弹出选择 仪表板对话框，选择其中你想加入的仪表板。单击确定。

![add](/datart-docs/images/storyboard/addpage.png)

添加完成后 如下图，每个仪表板将作为 故事板的一个故事页（storypage）

![addok](/datart-docs/images/storyboard/addok.png)

## 3. 删除一个故事页

在编辑模式下，选中一个故事页，点击键盘上`back` 键 弹出删除确认对话框,点击确定即可是删除。

![](/datart-docs/images/storyboard/delete.jpg)

## 4. 设计切换效果

你可以给每页设计切换效果

![](/datart-docs/images/storyboard/effact.jpg)

点击观看切换效果

![qiehuan](/datart-docs/images/storyboard/qiehuan.gif)

## 5. 设置自动播放

如图我们开启了自动播放，并且设置每页的停放留时间 2 秒.并点击完成。

![auto](/datart-docs/images/storyboard/makeAuto.png)

我们可以在预览页面看到如下按钮

![](/datart-docs/images/storyboard/playBtn.png)

点击播放按钮，会在新窗口打开播放页面。你可以点击左下角的按钮，进行手动控制，让它暂停继续等。

![](/datart-docs/images/storyboard/showAuto.gif)

## 6. 基础功能

### 6.1 分享管理

分享管理功能用于管理当前故事板的分享链接。可以在工具栏右侧的扩展菜单中找到“分享管理”选项

![分享管理-1](/datart-docs/images/storyboard/6-1-1.png)

点击之后，在弹出的的面板中新建和编辑分享链接

![分享管理-2](/datart-docs/images/storyboard/6-1-2.png)

点击右上角的“添加链接”按钮新建一个分享链接，需要输入以下选项

- 截止日期：指定当前分享链接的过期时间；可选项，当不填写时链接永久有效。**需要注意的是，由于截止日期在应用数据库中以 timestamp 类型存储，因此不支持设置 2038 年以后的日期**

- 认证方式：

  - 无：公共分享，任何人都可以访问。数据权限与分享者相同

    ![分享管理-3](/datart-docs/images/datachart/3-7-3.png)

  - 口令：会随链接一同生成一个口令，访问分享链接时需要正确输入口令才能查看内容。数据权限与分享者相同

    ![分享管理-4](/datart-docs/images/datachart/3-7-4.png)

  - 登录：访问分享链接时需要登录查看内容。该认证方式会多出 2 个额外选项

    - 数据权限：可以选择访问链接时所查看数据的权限。当选择“登录者”时，会依照不同的登录者展示符合其自身权限的数据；当选择“分享者”时，所有人访问链接查看到的数据与分享者相同
    - 指定角色/成员：指定能够访问当前分享链接的角色和成员；可选项，当不填写时按照访问用户自身的[权限设定](permission)来展示内容。

    ![分享管理-5](/datart-docs/images/datachart/3-7-5.png)

点击确认之后，生成的分享链接会展示在面板上。可以在面板上快捷复制链接地址、编辑和删除链接

![分享管理-6](/datart-docs/images/storyboard/6-1-3.png)

### 6.2 移至回收站

在故事板列表和右侧面板工具栏的扩展菜单中都可以将当前故事板移至回收站

![移至回收站-1](/datart-docs/images/storyboard/6-2-1.png)

点击并确认之后，该故事板会被移动到回收站归档，无法再被使用

### 6.3 还原

点击故事板列表顶部的扩展按钮，进入回收站

![还原-1](/datart-docs/images/storyboard/6-3-1.png)

系统会将移至回收站的故事板重命名，规则为`原名称.时间戳`；点击回收站列表的任意故事板，右侧区域会展示该故事板的详细信息，为只读状态

![还原-2](/datart-docs/images/storyboard/6-3-2.png)

点击列表右侧扩展按钮，选择`还原`，在弹窗表单中需要重新填写还原后的名称和目录，填写完成后，点击保存按钮完成还原

![还原-3](/datart-docs/images/storyboard/6-3-3.png)

### 6.4 删除

查看回收站列表里的故事板信息时，点击列表右侧扩展按钮，选择`删除`，确认对话框之后，将会永久删除该故事板

![删除](/datart-docs/images/storyboard/6-4-1.png)

### 6.5 搜索

点击目录树顶部的`搜索`按钮，会弹出关键字搜索框，可以按照名称检索故事板

![搜索](/datart-docs/images/storyboard/6-5-1.png)

