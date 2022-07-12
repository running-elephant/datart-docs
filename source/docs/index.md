---
title: 部署
---

# 0. 在线体验

- http://datart-demo.retech.cc
- 用户名：demo
- 密码：123456

# 1. Docker 部署

datart 在 dockerhub 中的公共镜像地址为 [datart/datart](https://hub.docker.com/r/datart/datart)。如果你本地已经安装了 docker，执行以下命令可以一键安装：

```shell
docker run -p 8080:8080 datart/datart
```

镜像启动成功后，在浏览器中访问 <http://docker_ip:8080> 进入登录页。镜像中提供初始账户，用户名 `demo` 密码 `123456`

## 1.1 配置应用数据库

在默认情况下，Datart 使用内置的 H2 作为应用程序数据库。 如果将 datart 用于生产环境，建议使用 MySQL 作为应用程序数据库。配置步骤如下：

1. 新建一个名为 `datart.conf` 的空文件，将以下内容填写完整，然后粘贴到到文件中

```shell
# 应用数据库配置
datasource.ip=localhost       # 数据库IP或域名
datasource.port=3306          # 数据库端口
datasource.database=datart    # 数据库名称
datasource.username=root      # 用户名
datasource.password=root      # 密码

# 应用服务器配置
server.port=8080              # 服务器端口
server.address=0.0.0.0        # 服务器地址（内网地址）

# datart 全局配置
datart.address=http://127.0.0.1:8080                # 应用主页地址（公网地址）
datart.send-mail=false                              # 注册账户时是否需要邮件激活
datart.webdriver-path=http://127.0.0.1:4444/wd/hub  # ChromeDriver 地址（用于截图）
```

2. 运行以下命令，使用新建的 `datart.conf` 配置启动镜像

```shell
docker run -d --name datart -v your_path/datart.conf:/datart/config/datart.conf -p 8080:8080 datart/datart
```

## 1.2 文件挂载

在默认情况下，用户在应用中生成文件（头像、文件数据源等）保存在 `files` 路径下。为保证在应用升级时这些文件得以保留，可以将这个路径挂载到容器外部；在启动命令中增加参数 `-v your_path/files:/datart/files` 即可。以下是完整命令：

```shell
docker run -d --name datart -v your_path/datart.conf:/datart/config/datart.conf -v your_path/files:/datart/files -p 8080:8080 datart/datart
```

# 2. 本地部署

## 2.1 环境准备

- JDK 1.8+
- MySql5.7+
- datart 安装包（datart-server-\*-install.zip)
- [Chrome](https://www.google.com/chrome/) 和 [WebDriver](https://chromedriver.chromium.org/) （可选）
- Redis （可选）

## 2.2 文件结构

首先解压安装包

```bash
unzip datart-server-*-install.zip
```

解压之后的文件结构如下

```yml
├── bin               # 执行脚本目录
├── config            # 配置文件目录
├── (files)           # 应用生成文件目录；应用运行后生成
├── lib               # 项目依赖目录
├── (logs)            # 日志目录；应用运行后生成
├── static            # 静态资源目录
├── nohup.out         # 缺省日志输出文件
├── Deployment.md     # 部署说明
├── Dockerfile
└── LICENSE
```

## 2.3 启动应用

运行 `bin` 目录下的脚本来启动应用，Linux 用户使用 `bin/datart-server.sh`，Windows 用户使用 `bin/datart-server.cmd`。以 Linux 系统举例，命令列表如下：

```bash
${DATART_HOME}/bin/datart-server.sh start       # 启动
${DATART_HOME}/bin/datart-server.sh stop        # 停止
${DATART_HOME}/bin/datart-server.sh status      # 查看状态
${DATART_HOME}/bin/datart-server.sh restart     # 重启
```

### 2.3.1 直接运行

安装包解压后，即可直接运行脚本启动应用。**需要注意的是，直接启动时使用的是内置的 H2 数据库作为应用数据库，升级应用时无法迁移数据，不建议在生产环境使用**

启动之后通过 <http://127.0.0.1:8080> 地址访问应用主页，内置初始账户，用户名 `demo` 密码 `123456`

### 2.3.2 配置应用数据库

datart 目前支持配置 MySQL 作为应用数据库；**需要 MySQL 5.7 及以上版本**。配置步骤如下：

1. 创建数据库，指定数据库编码为 utf8

```bash
mysql> CREATE DATABASE `datart` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```

**_注意：1.0.0-beta.2 版本以前，需要手动执行`bin/datart.sql`来初始化数据库。此版本及以上版本，创建好数据库即可，在初次连接时会自动初始化数据库_**

**_首次连接数据库(或者版本升级)时,建议使用一个权限较高的数据库账号登录(建议 root 账号)。因为首次连接会执行数据库初始化脚本，如果使用的数据库账号权限太低，会导致数据库初始化失败_**

2. 编辑 `config/datart.conf` 文件完成配置

```bash
datasource.ip=localhost           # 数据库IP或域名
datasource.port=3306              # 数据库端口
datasource.database=datart        # 数据库名称
datasource.username=root          # 用户名
datasource.password=root          # 密码
```

# 3. 详细配置

datart 的所有应用配置文件在 `config` 目录下。如果需要使用 datart 的全部功能，在这之前先了解一下如何进行配置

`config` 目录结构如下：

```yml
├── datart.conf
├── jdbc-driver-ext.yml
├── logback.xml
└── profiles
└── application-config.yml
```

- `datart.conf` 为快捷配置文件；如果你只想快速体验 datart 的功能，配置它就足够了。`datart.conf` 本质上是 `application-config.yml` 中常用配置的快捷方式
- `jdbc-driver-ext.yml` 为 JDBC 数据源扩展文件；详细内容请参考[数据源](source)和[扩展 JDBC 数据源](jdbc)
- `logback.xml` 为日志配置文件
- `application-config.yml` 为应用配置文件，包含所有的应用配置。建议将所有的应用配置文件拷贝都放置到 `profiles` 目录下

## 3.1 快捷配置

`datart.conf` 为快捷配置文件；所有配置参数如下：

```bash
# ====== 应用数据库配置 ======

# 数据库IP或域名
datasource.ip=localhost

# 数据库端口
datasource.port=3306

# 数据库名称
datasource.database=datart

# 用户名
datasource.username=root

# 密码
datasource.password=root

# ====== 应用服务器配置 ======

# 服务器端口
server.port=8080

# 服务器地址
#   Web 服务所绑定的本机网卡地址，一般为内网地址
server.address=0.0.0.0

# ====== datart 全局配置 ======

# 应用主页地址
#   浏览器访问应用主页输入的地址，一般为公网地址
datart.address=http://127.0.0.1:8080

# Chrome WebDriver 地址
datart.webdriver-path=http://127.0.0.1:4444/wd/hub

# 是否允许注册账户
datart.user.register=true
# 注册账户时，是否需要邮件激活
datart.send-mail=false

# 注册邮件有效期/小时, 默认48小时
datart.register.expire-hours=
# 邀请邮件有效期/小时, 默认48小时
datart.invite.expire-hours=

# 租户管理模式：platform-平台(默认)，team-团队
datart.tenant-management-mode=platform
```

## 3.2 应用配置

`application-config.yml` 为应用配置文件，里面包含 datart 应用的所有配置。`datart.conf` 中的内容实际上是 `application-config.yml` 部分配置项的快捷方式。

在编辑配置时需要注意以下事项：

1. **一定要严格遵循 yml 格式，注意空格与缩进，错误配置会导致程序无法正常启动**
2. `application-config.yml` 直接由 spring-boot 处理，其中的 oauth2, redis, mail 等配置项完全遵循 spring-boot-autoconfigure 配置

### 3.2.1 应用数据库配置

`spring.datasource` 为应用数据库配置

```yaml
spring:
  datasource:
    # 驱动类名称
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 数据源类型
    type: com.alibaba.druid.pool.DruidDataSource
    # 连接字符串
    url: jdbc:mysql://${datasource.ip:null}:${datasource.port:3306}/${datasource.database:datart}?&allowMultiQueries=true&characterEncoding=utf-8
    # 用户名
    username: ${datasource.username:root}
    # 密码
    password: ${datasource.password:123456}
```

**注意：请务必保留 url 中的`allowMultiQueries=true`参数**

默认情况下，会读取 `datart.conf` 中的应用数据库配置项填充到模板中

### 3.2.2 应用服务器配置

```yaml
server:
  # Web 服务绑定端口
  port: ${server.port:8080}
  # Web 服务绑定地址
  address: ${server.ip:0.0.0.0}
```

需要注意，所绑定的地址为本地网卡 IP 地址，一般为内网地址

同样在默认情况下，会读取 `datart.conf` 中的应用服务器配置项填充到模板中

### 3.2.3 datart 全局配置

- 配置服务端访问地址，创建分享，激活/邀请用户时，将使用这个地址作为服务端访问地址。

```yaml
datart:
  server:
    # 应用主页地址；一般为公网地址
    address: ${datart.address:http://127.0.0.1:8080}

  # 租户管理模式
  #   platform: 平台模式
  #   team: 团队模式
  tenant-management-mode: platform

  user:
    # 是否允许注册用户
    register: true
    active:
      # 注册用户时是否需要邮件激活
      send-mail: false
      # 激活邮件有效期（小时）
      expire-hours: 48
    invite:
      # 邀请加入组织邮件有效期（小时）
      expire-hours: 48

  security:
    token:
      # 加密密钥
      secret: "sHAS$as@fsdkKjd"
      # 登录会话有效时长（分钟）
      timeout-min: 30

  env:
    # 应用生成文件保存路径
    file-path: ${user.dir}/files

  migration:
    # 是否自动执行数据库升级脚本
    enable: true
```

- 应用主页地址：应用程序新建分享链接、截图、定时任务、激活账户、邀请成员时会使用到该地址
- 租户管理模式：参考[租户管理模式](tenant-management-mode)中的详细介绍
- 邮件激活：如果没有配置[邮件服务](#3-2-4-邮件服务配置)，请把它设为 false
- 加密密钥：
  - 密钥被用于应用鉴权 token 和 [JDBC 数据源密码](source#1-JDBC)加密
  - 密钥在应用投入使用之后不建议修改，会导致解密错误
  - 如果是集群部署 datart，同一个集群内的密钥要保持统一

### 3.2.4 邮件服务配置

邮件服务用于定时任务邮件发送，用户激活，组织成员邀请，如果需要使用这些功能，请务必正确地配置邮件服务

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

`username` 为邮箱地址，`password` 为邮箱服务密码，需要注意的是常见免费邮箱（如 163 邮箱、QQ 邮箱、gmail 等）这里应填客户端独立密码，可前往对应邮箱账号设置页面开启 SMTP 服务，并申请客户端授权码（或独立密码，各邮箱提供商叫法不同）

以下为常见免费邮箱 SMTP 服务地址及端口：

![mail.png](/datart-docs/images/deployment/mail.png)

### 3.2.5 截图配置

在 datart 中，导出截图、导出 PDF、定时邮件发送截图，都需要完成截图配置后才能正常使用

```yaml
datart:
  screenshot:
    # 截图超时时间（秒）
    timeout-seconds: 60
    # 工具类型；目前仅支持 CHROME
    webdriver-type: CHROME
    # WebDriver 可执行文件地址；填写服务器路径或远程地址都可以
    webdriver-path: { Web Driver Path }
```

- datart 没有内置 Chrome 浏览器，需要自行安装
- 请确保 Chrome 版本和 WebDriver 版本匹配。推荐使用 docker 镜像 [selenium/standalone-chrome](https://registry.hub.docker.com/r/selenium/standalone-chrome) 来配置远程截图服务，参考以下步骤：

  1. 执行命令 `docker run -p 4444:4444 -d --name selenium-chrome --shm-size="2g" selenium/standalone-chrome`

  2. 配置 `webdriver-path: "http://{IP}:4444/wd/hub"`

### 3.2.6 缓存（Redis）配置

配置了 Redis 之后，可以在[数据视图的高级配置中中开启缓存](view#7-高级配置)

```yaml
spring:
  redis:
    port: 6379
    host: { HOST }
```

### 3.2.7 单点登录配置

datart 支持配置基于 OAuth2 和 LDAP 的单点登录方式。

用户首次使用所配置的单点登录方式登录成功后，datart 会使用用户的邮箱注册一个新账户。在此之后的登录都视为该账户登录。

#### 3.2.7.1 OAuth2

启用 OAuth2 登录需要在 `spring.security.oauth2.client` 下配置客户端信息，可以配置多个 OAuth2 客户端。在配置之前，需要到提供 OAuth2 服务的网站获取 `Client ID` 和 `Client Secret`

下面为一个 OAuth2 客户端的配置示例：

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          cas:
            provider: cas
            client-id: "xxxxx"
            client-name: "Sign in with CAS"
            client-secret: "xxx"
            authorization-grant-type: authorization_code
            client-authentication-method: post
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            scope: userinfo
        provider:
          cas:
            authorization-uri: https://cas.xxx.com/cas/oauth2.0/authorize
            token-uri: https://cas.xxx.com/cas/oauth2.0/accessToken
            user-info-uri: https://cas.xxx.com/cas/oauth2.0/profile
            user-name-attribute: id
            userMapping:
              email: "attributes.email"
              name: "attributes.name"
              avatar: "attributes.avatar"
```

在 `spring.security.oauth2.client` 底下有 `registration` 和 `provider` 两个配置项，`registration` 包含所有客户端的注册信息，`provider` 包含所有客户端所访问的 OAuth2 服务提供者的相关信息。下方表格将参照示例说明每一个配置项的作用：

| 配置项                                                     | 说明                                                                                                                                                                                     |
| ---------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| registration.[registrationId]                              | 注册 ID；自定义名称，示例中命名为 cas                                                                                                                                                    |
| registration.[registrationId].provider                     | 关联服务提供者 ID；通过此项来关联到 provider 下配置的具体服务提供者                                                                                                                      |
| registration.[registrationId].client-id                    | 客户端 ID；从 OAuth2 服务网站获取的 Client ID                                                                                                                                            |
| registration.[registrationId].client-name                  | 客户端名称；内容自定义                                                                                                                                                                   |
| registration.[registrationId].client-secret                | 客户端密钥；从 OAuth2 服务网站获取的 Client Secret                                                                                                                                       |
| registration.[registrationId].authorization-grant-type     | 授权许可类型；示例中为 `authorization_code`，更多信息请参考 [The OAuth 2.0 Authorization Framework Authorization Grant types](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3) |
| registration.[registrationId].client-authentication-method | 客户端认证方法；http method，示例中为 `post`                                                                                                                                             |
| registration.[registrationId].redirect-uri                 | 认证成功回调地址；支持模板语法                                                                                                                                                           |
| registration.[registrationId].scope                        | 请求范围；指从授权请求获取的内容范围，例如 openid、email 或 profile。示例中从授权请求获取了 userinfo                                                                                     |
| provider.[providerId]                                      | 服务提供者 ID；自定义名称，示例中命名为 cas                                                                                                                                              |
| provider.[providerId].authorization-uri                    | 授权请求的 URL                                                                                                                                                                           |
| provider.[providerId].token-uri                            | 获取 Token 请求的 URL                                                                                                                                                                    |
| provider.[providerId].user-info-uri                        | 获取用户信息请求的 URL                                                                                                                                                                   |
| provider.[providerId].user-name-attribute                  | 用户信息中用于标记用户名称或 ID 的属性。示例中为 id                                                                                                                                      |
| provider.[providerId].userMapping                          | 用户信息与 datart 用户的属性映射设置。datart 通过该映射配置来定位到系统中的具体用户                                                                                                      |

如想了解更多信息，可以参考 [spring security oauth2](https://docs.spring.io/spring-security/site/docs/5.2.12.RELEASE/reference/html/oauth2.html) 官方文档。

#### 3.2.7.2 LDAP

启用 LDAP 登录需要在 `spring.ldap` 下配置客户端信息。下面为开启 LDAP 登录的配置示例：

```yaml
spring:
  ldap:
    urls: ldap://192.168.100.6:389
    base: dc=example,dc=com
    username: cn=admin,dc=example,dc=com
    password: 123456
    attribute-mapping:
      username: cn
```

| 配置项                     | 说明                                                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| urls                       | LDAP 服务器 URL；格式固定为 `ldap://服务IP或域名:端口`                                                        |
| base                       | 基本专有名称（DN）                                                                                            |
| username                   | 使用 LDAP 服务器进行身份验证时使用的用户名（principal）；常规情况下为管理员用户的 DN（例如 cn=Administrator） |
| password                   | 使用 LDAP 服务器进行身份验证时使用的密码（credentials）                                                       |
| attribute-mapping          | 自定义属性映射                                                                                                |
| attribute-mapping.username | 用户名对应的属性名称                                                                                          |

## 3.3 日志配置

`logback.xml` 为日志配置文件。在此仅对必要配置项做简单介绍，进一步了解请查看 [logback 官方文档](https://logback.qos.ch/manual/configuration.html)

```xml
<configuration>
    <!-- 日志文件路径 -->
    <property name="LOG_HOME" value="./logs"/>
    <!-- SQL 日志等级 -->
    <property name ="SQL_LEVEL" value="INFO"/>
</configuration>
```

- 如果需要查看所有的查询 SQL 日志，请将 `SQL_LEVEL` 设置 `DEBUG`
