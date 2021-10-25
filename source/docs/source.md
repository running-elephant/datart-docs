---
title: 数据源
---

数据源用于管理数据连接、订阅与资源信息，支持通过数据源、文件、`REST API` 等多种方式获取数据。datart 内置了 `JDBC`、文件和 `http` 3种类型的数据源，在内置数据源不满足使用时，用户也可以扩展自己的数据源

## 1. JDBC

创建一个 `JDBC` 数据源通常需要配置以下内容：
- 名称：数据源名称，要求唯一
- dbType：要连接的数据库类型。默认支持30+种JDBC数据库。如果你的数据库不在选项中，请参照[扩展JDBC数据源](jdbc)
- url：必选项，通常格式为 `jdbc:<数据源名称>://<数据源域名或IP>:(<端口>)/<数据源实例>(?<连接参数>)`
- username：用户名
- password：密码
- driverClass：驱动类；datart 没有内置所有数据库的驱动类，所以当测试连接提示缺少驱动类时，用户需要手动填写驱动类名称
- serverAggregate：服务端聚合；开启后，在执行SQL时，会将数据视图中 SQL 查询的结果集全量拉取到服务端，然后进行本地分组聚合计算。这个选项适合数据源端计算效率低，或者计算能力弱的数据源进行开启。如果源端本身具有较强的计算能力，则不需要开启这个选项
- properties：连接参数，键值对形式


## 2. 文件

datart 支持上传 `Excel` 和 `csv` 文件作为数据源，文件存储在 datart 服务端；datart 使用内置的 H2 数据库对文件中的二维表进行计算，支持在数据视图中编写 SQL 对一个数据源下的多个文件进行关联与聚合操作

创建一个文件数据源的步骤如下：

1. 填写名称，选择类型，保存数据源

![文件-1](/datart-docs/images/source/2-1-1.png)

2. 保存成功之后，点击`新增配置`添加文件

![文件-2](/datart-docs/images/source/2-1-2.png)

3. 填写表名，上传文件

![文件-3](/datart-docs/images/source/2-1-3.png)

4. 上传成功之后，表格中会展示字段信息和文件内容，点击表头头部编辑字段类型

![文件-4](/datart-docs/images/source/2-1-4.png)

5. 保存文件信息，一个数据源下可以添加多个文件

6. 保存数据源

## 3. http

`http` 数据源允许用户使用 `REST API` 响应数据作为数据来源；datart 使用内置的 H2 数据库对响应数据进行计算，支持在数据视图中编写 SQL 对一个数据源下的多个 `REST API` 响应数据进行关联与聚合操作

创建一个 `http` 数据源的步骤如下：

1. 填写名称，选择类型，点击`新增配置`添加配置信息

![http-1](/datart-docs/images/source/3-1-1.png)

2. 通常需要配置以下内容：

- 表名：一个数据源下唯一
- url：`REST API` 地址
- method：请求方式，GET/POST/PUT/DELETE
- property：返回数据中需要解析的属性，指定的属性必须是一个list结构。多层嵌套用 `.` 隔开。如返回数据结构是数组，这个属性不填。 举例如下：

回数据格式如下, 则property不用填写

```json

[
  {
    "id": 1,
    "name": "zs"
  },
  {
    "id": 2,
    "name": "ls"
  }
]

返回数据格式如下，property 值为 `data.users`

```json
{
  "success": true,
  "data": {
    "users": [
      {
        "id": 1,
        "name": "zs"
      },
      {
        "id": 2,
        "name": "ls"
      }
    ]
  }
}
```

- username：用户名，可选，支持 Basic access authentication 身份验证方式
- password：密码，可选
- ResponseParser：默认为空。如果返回数据格式特殊，可通过实现 `datart.data.provider.HttpResponseParser`，然后通过这个参数指定具体的Parser实现类
- headers：请求头，键值对形式
- queryParam：请求参数，键值对形式
- body：请求体，文本
- contentType：默认值 `application/json`

![http-2](/datart-docs/images/source/3-1-2.png)

3. 填写完配置之后，点击 `url` 输入框上的`测试连接`按钮，测试通过之后，下方表格中会展示字段信息和响应数据，点击表头头部编辑字段类型

![http-3](/datart-docs/images/source/3-1-3.png)

4. 保存 `REST API` 连接信息，一个数据源下可以添加多个连接信息

5. 保存数据源

## 4. 基础功能

数据源支持以下通用操作

### 4.1 新建

点击数据源列表顶部的加号按钮，右侧区域会展示新建表单

![新建数据源-1](/datart-docs/images/source/4-1-1.png)

填写完表单信息后，点击右上角的`保存`按钮保存数据源

![新建数据源-2](/datart-docs/images/source/4-1-2.png)

### 4.2 测试连接

`JDBC` 数据源和 `http` 数据源在配置阶段可以测试连接是否有效，在填写完所有必填项之后，点击 `url` 输入框上的`测试连接`按钮即可进行测试

`JDBC` 数据源测试成功之后会在顶部提示信息

![测试连接-1](/datart-docs/images/source/4-2-1.png)

`http` 数据源测试成功之后在 `columns` 表格中展示获取到的字段信息和数据

![测试连接-2](/datart-docs/images/source/4-2-2.png)

### 4.3 编辑

点击选择数据源列表中的任意数据源，右侧区域会展示该数据源的配置信息，编辑完成后点击右上角`保存`按钮保存数据源

![编辑数据源-1](/datart-docs/images/source/4-3-1.png)

### 4.4 移至回收站

编辑数据源时，点击右上角的`移植回收站`按钮，确认对话框之后，该数据源会被移动到回收站归档，无法再被使用

![移至回收站-1](/datart-docs/images/source/4-4-1.png)

### 4.5 还原

点击数据源列表顶部的`扩展`按钮，进入回收站

![还原-1](/datart-docs/images/source/4-5-1.png)

系统会将移至回收站的数据源重命名，规则为`原名称.时间戳`；点击回收站列表的任意数据源，右侧区域会展示该数据源的信息

![还原-1](/datart-docs/images/source/4-5-2.png)

点击右上角`还原按钮`，确认对话框之后，该数据源会还原到数据源列表，恢复可用状态

### 4.6 删除

查看回收站列表里的数据源信息时，点击右上角`删除`按钮，确认对话框之后，将会永久删除该数据源

![删除-1](/datart-docs/images/source/4-6-1.png)

### 4.7 搜索

点击数据源列表顶部的`搜索`按钮，会弹出关键字搜索框，可以按照名称检索数据源

![搜索-1](/datart-docs/images/source/4-7-1.png)