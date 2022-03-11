---
title: 部署
---

# 0. 在线体验 Demo

- http://datart-demo.retech.cc
- 用户名：demo
- 密码：123456

# 1. Docker 部署

```shell
docker run -p 8080:8080 datart/datart 
```
启动后可访问 <http://docker_ip:8080>  
默认账户:用户名`demo`,密码`123456`

## 1.1. 配置外部数据库
在没有外部数据库配置的情况下，Datart使用H2作为应用程序数据库。 强烈建议您将自己的Mysql数据库配置为应用程序数据库。  

创建空文件 `datart.conf` ,将以下内容粘贴到到文件中。并配置数据库连接信息。

```shell
# 数据库连接配置
datasource.ip=   
datasource.port=
datasource.database=
datasource.username=
datasource.password=

# server
server.port=8080
server.address=0.0.0.0

# datart config
datart.address=http://127.0.0.1
datart.send-mail=false
datart.webdriver-path=http://127.0.0.1:4444/wd/hub
```

运行 `docker run -d --name datart -v your_path/datart.conf:/datart/config/datart.conf -p 8080:8080 datart/datart`

## 1.2. 将用户文件挂载到外部

默认配置下，用户文件（头像，文件数据源等）保存在 `files` 文件夹下，将这个路径挂载到外部，以在进行应用升级时，能够保留这些文件。

在配置文件中增加参数 `-v your_path/files:/datart/files` 即可。以下是完整命令

`docker run -d --name datart -v your_path/datart.conf:/datart/config/datart.conf -v your_path/files:/datart/files -p 8080:8080 datart/datart`


# 2 本地部署 
## 2.1 环境准备

- JDK 1.8+
- MySql5.7+
- datart安装包（datart-server-1.0.0-alpha.0-install.zip)
- Mail Server （可选）
- [ChromeWebDriver](https://chromedriver.chromium.org/) （可选）
- Redis （可选）

 解压安装包
```bash
unzip datart-server-*-install.zip
```

## 2.2 以独立模式运行

安装包解压后，即可运行 ./bin/datart-server.sh start 来启动datart,启动后默认访问地址是: <http://127.0.0.1:8080>,默认用户`demo/123456`

***独立模式使用内置数据库作为应用数据库，目前无法保证数据迁移，建议配置外部数据库作为应用数据库***

## 2.3 配置外部数据库，要求Mysql5.7及以上版本

- 创建数据库，指定数据库编码为utf8

```bash
mysql> CREATE DATABASE `datart` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```

***注意：1.0.0-beta.2版本以前，需要手动执行`bin/datart.sql`来初始化数据库。此版本及以上版本，创建好数据库即可，在初次连接时会自动初始化数据库***

***首次连接数据库(或者版本升级)时,建议使用一个权限较高的数据库账号登录(建议root账号)。因为首次连接会执行数据库初始化脚本，如果使用的数据库账号权限太低，会导致数据库初始化失败***

- 基础配置：配置文件位于 config/datart.conf

```bash
   数据库配置(必填):
    1. datasource.ip(数据库IP地址)
    2. datasource.port(数据库端口数据库端口)
    3. datasource.database(指定数据库)
    4. datasource.username(用户名)
    5. datasource.password(密码)
    
   其它配置(选填):
    1. server.port(应用绑定端口地址,默认8080)
    2. server.address(应用绑定IP地址,默认 0.0.0.0)
    3. datart.address(datart 外部可访问地址,默认http://127.0.0.1)
    4. datart.send-mail(用户注册是否使用邮件激活,默认 false )
    5. datart.webdriver-path(截图驱动)
```

## 2.4. 完整配置 (可选) : 配置文件位于 config/profiles/application-config.yml

***完整配置文件格式是yml格式,配置错误会导致程序无法启动。配置时一定要严格遵循yml格式。***

***application-config.yml直接由spring-boot处理,其中的oauth2,redis,mail等配置项完全遵循spring-boot-autoconfigure配置***

### 2.4.1 数据库连接信息，可以在 `darart.conf` 文件中配置，也可直接修改这里的配置。推荐保留这里的默认配置，把数据库信息配置到 `darart.conf`文件中。

***注：请务必保留连接串中的`allowMultiQueries=true`参数***

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://${datasource.ip:null}:${datasource.port:3306}/${datasource.database:datart}?&allowMultiQueries=true&characterEncoding=utf-8
    username: ${datasource.username:root}
    password: ${datasource.password:123456}
```

### 2.4.2 服务端属性配置

- Web服务绑定IP和端口

```yaml
server:
  port: ${server.port:8080}
  address: ${server.ip:0.0.0.0}
```

- 配置服务端访问地址，创建分享，激活/邀请用户时，将使用这个地址作为服务端访问地址。

```yaml
datart:
  server:
    address: ${datart.address:http://127.0.0.1:8080}

```

- 其它可选配置项

```yaml
datart:
  user:
    active:
      send-mail: false  # 注册用户时是否需要邮件验证，如果没配置邮箱，这里需要设置为false

  security:
    token:
      secret: "sHAS$as@fsdkKjd" #加密密钥
      timeout-min: 30  # 登录会话有效时长，单位：分钟。

  env:
    file-path: ${user.dir}/files # 服务端文件保存位置 

```

*注意：加密密钥每个服务端部署前应该进行修改，且部署后不能再次修改。如果是集群部署，同一个集群内的secret要保持统一*

### 2.5 邮件服务配置 (可选)

***邮件服务用于定时任务发送，用户注册/激活/邀请等功能。要体验完整的datart功能，请务必正确的邮件服务。***

- 以下为完整配置

```yaml
spring:
  mail:
    host: { 邮箱服务地址 }
    port: { 端口号 }
    username: { 邮箱地址 }
    fromAddress:
    password: { 邮箱服务密码 }
    senderName: { 发送者昵称 }

    properties:
      smtp:
        starttls:
          enable: true
          required: true
        auth: true
      mail:
        smtp:
          ssl:
            enable: true
```

***配置说明***

- `username`为邮箱地址，`password`邮箱服务密码，需要注意的是常见免费邮箱（如 163 邮箱、QQ 邮箱、gmail 等）这里应填客户端独立密码，可前往对应邮箱账号设置页面开启 SMTP
  服务，并申请客户端授权码（或独立密码，各邮箱提供商叫法不同）

下表为常见免费邮箱 SMTP 服务地址及端口：
![mail.png](/datart-docs/images/deployment/mail.png)

### 2.6截图配置 (可选)

- 截图配置用于定时任务中的发送图片功能。

```yaml
datart:
  screenshot:
    timeout-seconds: 60
    webdriver-type: CHROME
    webdriver-path: {Web Driver Path}
```

- 配置说明
  `timeout-seconds` 指定截图超时时间
  `webdriver-type` 截图浏览器类型。(目前仅支持`CHROME`)
  `webdriver-path` webdriver地址。可执行文件的绝对地址，或远程webdriver的调用地址

*

注意：配置时请确保浏览器的版本和webdriver的版本匹配。推荐使用docker镜像 [selenium/standalone-chrome](https://registry.hub.docker.com/r/selenium/standalone-chrome)
来配置远程截图服务。*

```
docker run -p 4444:4444 -d --name selenium-chrome --shm-size="2g" selenium/standalone-chrome
```

- 然后配置 `webdriver-path: "http://{IP}:4444/wd/hub"`

### 2.7 redis (可选)
- redis用于查询结果缓存
```yaml
  redis:
    port: 6379
    host: { HOST }
```

### 2.8 oauth2 

```ymal
# security:
#   oauth2:
#     client:
#       registration:
#         cas:
#           provider: cas
#           client-id: "xxxxx"
#           client-name: "Sign in with CAS"
#           client-secret: "xxx"
#           authorization-grant-type: authorization_code
#           client-authentication-method: post
#           redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
#           scope: userinfo
#       provider:
#         cas:
#           authorization-uri: https://cas.xxx.com/cas/oauth2.0/authorize
#           token-uri: https://cas.xxx.com/cas/oauth2.0/accessToken
#           user-info-uri: https://cas.xxx.com/cas/oauth2.0/profile
#           user-name-attribute: id
#           userMapping:
#             email: "attributes.email"
#             name: "attributes.name"
#             avatar: "attributes.avatar"
```
