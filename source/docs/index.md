---
title: 部署
---

# 0. 在线体验 Demo

- http://datart-demo.retech.cc
- 用户名：demo
- 密码：123456

# 1. Docker 部署

datart 在 dockerhub 中的公共镜像地址为 [datart/datart](https://hub.docker.com/r/datart/datart)。如果你本地已经安装了 docker，执行以下命令可以一键安装：

```shell
docker run -p 8080:8080 datart/datart
```

镜像启动成功后，在浏览器中访问 <http://docker_ip:8080> 进入登录页。镜像中提供初始账户，用户名`demo` 密码`123456`

## 1.1. 配置外部数据库

在没有外部数据库配置的情况下，Datart 使用 H2 作为应用程序数据库。 强烈建议您将自己的 Mysql 数据库配置为应用程序数据库。

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

在启动命令中增加参数 `-v your_path/files:/datart/files` 即可。以下是完整命令

`docker run -d --name datart -v your_path/datart.conf:/datart/config/datart.conf -v your_path/files:/datart/files -p 8080:8080 datart/datart`

# 2 本地部署

## 2.1 环境准备

- JDK 1.8+
- MySql5.7+
- datart 安装包（datart-server-\*-install.zip)
- Mail Server （可选）
- [ChromeWebDriver](https://chromedriver.chromium.org/) （可选）
- Redis （可选）

解压安装包

```bash
unzip datart-server-*-install.zip
```

## 2.2 以独立模式运行

安装包解压后，即可运行 ./bin/datart-server.sh start 来启动 datart,启动后默认访问地址是: <http://127.0.0.1:8080>,默认用户`demo/123456`

**_独立模式使用内置数据库作为应用数据库，目前无法保证数据迁移，建议配置外部数据库作为应用数据库_**

## 2.3 配置外部数据库

**_要求 Mysql5.7 及以上版本_**

- 创建数据库，指定数据库编码为 utf8

```bash
mysql> CREATE DATABASE `datart` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```

**_注意：1.0.0-beta.2 版本以前，需要手动执行`bin/datart.sql`来初始化数据库。此版本及以上版本，创建好数据库即可，在初次连接时会自动初始化数据库_**

**_首次连接数据库(或者版本升级)时,建议使用一个权限较高的数据库账号登录(建议 root 账号)。因为首次连接会执行数据库初始化脚本，如果使用的数据库账号权限太低，会导致数据库初始化失败_**

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

## 2.4. 完整配置（可选）

**_配置文件位于 config/profiles/application-config.yml_**

**_完整配置文件格式是 yml 格式,配置错误会导致程序无法启动。配置时一定要严格遵循 yml 格式。_**

**_application-config.yml 直接由 spring-boot 处理,其中的 oauth2,redis,mail 等配置项完全遵循 spring-boot-autoconfigure 配置_**

### 2.4.1 数据库连接信息

**可以在 `darart.conf` 文件中配置，也可直接修改这里的配置。推荐保留这里的默认配置，把数据库信息配置到 `darart.conf`文件中**

**_注：请务必保留连接串中的`allowMultiQueries=true`参数_**

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://${datasource.ip:null}:${datasource.port:3306}/${datasource.database:datart}?&allowMultiQueries=true&characterEncoding=utf-8
    username: ${datasource.username:root}
    password: ${datasource.password:123456}
```

### 2.4.2 服务端属性配置（可选）

- Web 服务绑定 IP 和端口

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
      send-mail: false # 注册用户时是否需要邮件验证，如果没配置邮箱，这里需要设置为false

  security:
    token:
      secret: "sHAS$as@fsdkKjd" #加密密钥
      timeout-min: 30 # 登录会话有效时长，单位：分钟。

  env:
    file-path: ${user.dir}/files # 服务端文件保存位置
```

_注意：加密密钥每个服务端部署前应该进行修改，且部署后不能再次修改。如果是集群部署，同一个集群内的 secret 要保持统一_

### 2.5 邮件服务配置（可选）

**_邮件服务用于定时任务发送，用户注册/激活/邀请等功能。要体验完整的 datart 功能，请务必正确的邮件服务。_**

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

**_配置说明_**

- `username`为邮箱地址，`password`邮箱服务密码，需要注意的是常见免费邮箱（如 163 邮箱、QQ 邮箱、gmail 等）这里应填客户端独立密码，可前往对应邮箱账号设置页面开启 SMTP
  服务，并申请客户端授权码（或独立密码，各邮箱提供商叫法不同）

下表为常见免费邮箱 SMTP 服务地址及端口：
![mail.png](/datart-docs/images/deployment/mail.png)

### 2.6 截图配置（可选）

- 截图配置用于定时任务中的发送图片功能。

```yaml
datart:
  screenshot:
    timeout-seconds: 60
    webdriver-type: CHROME
    webdriver-path: { Web Driver Path }
```

- 配置说明
  `timeout-seconds` 指定截图超时时间
  `webdriver-type` 截图浏览器类型。(目前仅支持`CHROME`)
  `webdriver-path` webdriver 地址。可执行文件的绝对地址，或远程 webdriver 的调用地址

*

注意：配置时请确保浏览器的版本和 webdriver 的版本匹配。推荐使用 docker 镜像 [selenium/standalone-chrome](https://registry.hub.docker.com/r/selenium/standalone-chrome)
来配置远程截图服务。\*

```
docker run -p 4444:4444 -d --name selenium-chrome --shm-size="2g" selenium/standalone-chrome
```

- 然后配置 `webdriver-path: "http://{IP}:4444/wd/hub"`

### 2.7 redis（可选）

- redis 用于查询结果缓存

```yaml
spring:
  redis:
    port: 6379
    host: { HOST }
```

### 2.8 单点登录配置（可选）

datart 支持配置基于 OAuth2 和 LDAP 的单点登录方式。

用户首次使用所配置的单点登录方式登录成功后，datart 会使用用户的邮箱注册一个新账户。在此之后的登录都视为该账户登录。

#### 2.8.1 OAuth2

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

#### 2.8.2 LDAP

启用 LDAP 登录需要在 `spring.ldap` 下配置客户端信息。下面为开启 LDAP 登录的配置示例：

```yaml
spring:
  ldap:
    urls: ldap://192.168.100.6:389
    base: dc=example,dc=com
    username: cn=admin,dc=example,dc=com
    password: 123456
```

| 配置项   | 说明                                                                                                          |
| -------- | ------------------------------------------------------------------------------------------------------------- |
| urls     | LDAP 服务器 URL；格式固定为 `ldap://服务IP或域名:端口`                                                        |
| base     | 基本专有名称（DN）                                                                                            |
| username | 使用 LDAP 服务器进行身份验证时使用的用户名（principal）；常规情况下为管理员用户的 DN（例如 cn=Administrator） |
| password | 使用 LDAP 服务器进行身份验证时使用的密码（credentials）                                                       |
