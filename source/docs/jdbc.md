---
title: 扩展JDBC数据源
---

## 1. 添加驱动文件

datart release package 中默认只提供了 `MySQL 8.0` 的 JDBC 驱动文件，如果用户需要使用其他数据库作为数据源，需要自行手动添加驱动文件

准备对应数据库的 JDBC 驱动 jar 包，放到服务端 `lib/` 路径下，重启服务即可生效

## 2. 扩展JDBC数据源

datart并没有穷举所有的JDBC数据库类型，这个做法也有一定难度。对于不在默认支持列表中的数据库，可通过简单几步配置就可以支持。

+ 找到 `conf/jdbc-driver-ext.yml` 文件，添加关键配置，然后重启程序。
+ 以下以添加 `impala` 为例，在`conf/jdbc-driver-ext.yml`中添加以下配置。

```yaml
IMPALA:
  db-type: "impala"
  name: "impala"
  literal-quote: "'"
  identifier-quote: "`"
#  literal-end-quote: "`"
#  identifier-end-quote: "'"
#  driver-class: "com.mysql.cj.jdbc.Driver" 
#  url-prefix:
```

配置说明

- **`db-type` : 必填,数据库类型，唯一**
- **`name` : 必填,数据库名称，唯一**
- **`literal-quote`: 必填,字符型参数引号。**
- **`identifier-quote`: 必填，SQL字段/列名引号。**
- `literal-end-quote` : 非必填，当字符型参数引号左右不一样时需要填写。
- `identifier-end-quote`: 非必填，当SQL字段/列名引号左右不一致时，需要填写。如Sql Server使用的 `[`,`]`
- `driver-class` : 非必填，驱动类名称。可在数据源界面指定。
- `url-prefix`: 非必填，url连接串前缀。可在数据源界面配置。

配置完成后，按照第一段描述添加驱动文件，重启服务端。刷新页面，就可以在JDBC数据源下找到刚才添加的 `impala` 数据库