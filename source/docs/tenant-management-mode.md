---
title: 租户管理模式
---

# 场景

不同的企业对数据可视化平台的定位不一样，datart 希望尽可能满足更多企业对数据可视化平台的使用和管理需求，在对租户的管理方面提供了两种模式

## 平台模式

平台模式的租户管理规则如下：

- 每个用户都可以新建组织，数量不限
- 如果希望其他用户加入自己的组织，只能通过邀请的方式；不支持组织拥有者直接新建或删除用户

平台模式适用于多部门、部门内数据需求自治的企业。在这种场景下，由每个部门的管理者负责创建组织、划分角色与权限、邀请其他成员参与制作和浏览可视化作品。如果有跨部门协作或浏览的用户，可以同时加入多个组织

## 团队模式

团队模式的租户管理规则如下：

- 整个应用仅有一个组织，所有用户都在该组织下；不支持新建其他组织
- 组织拥有者可以直接新建和删除用户

团队模式适用于有独立数据（技术）部门的企业。在这种场景下，需要对访问应用的每个用户做更精细化的管理

# 配置

租户管理模式在 `datart.conf` 和 `application-config.yml` 中都可以设置

`datart.conf`

```
datart.tenant-management-mode=platform
```

`application-config.yml`

```yaml
tenant-management-mode: platform
```

可填写的值及其含义：

- platform: 平台模式
- team: 团队模式

# 使用

两种模式在功能上也有一些差异

1. 在主导航栏上，平台模式下有组织列表按钮，团队模式下没有

<img width="350" src="/datart-docs/images/tenant-management-mode/1.png" alt="1" />
<img width="350" src="/datart-docs/images/tenant-management-mode/2.png" alt="2" />

2. 在成员与角色功能中

平台模式下添加成员的方式为邀请成员，点击后弹出邀请成员对话框

<img width="270" src="/datart-docs/images/tenant-management-mode/3.png" alt="3" />
<img width="430" src="/datart-docs/images/tenant-management-mode/4.png" alt="4" />

团队模式下添加成员方式为新建成员，点击后右侧会显示新建表单

<img width="270" src="/datart-docs/images/tenant-management-mode/5.png" alt="5" />
<img width="430" src="/datart-docs/images/tenant-management-mode/6.png" alt="6" />

# 注意事项

- 在选择租户管理模式之前请考虑好自身企业的使用场景
- 从团队模式改变为平台模式，依然能够正常使用；但从平台模式改变为团队模式时，如果应用数据库中已有多个组织，会导致应用无法正常启动
- 如果没想好的话，请选择团队模式
