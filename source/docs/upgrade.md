---
title: 升级指南
---

# Docker 部署升级

```shell
# 更新镜像
docker pull datart/datart
# 停止与删除旧版本容器
docker stop datart_container_name
docker rm datart_container_name
# 使用新版本镜像启动容器
docker run -d --name datart -v your_path/datart.conf:/datart/config/datart.conf -v your_path/files:/datart/files -p 8080:8080 datart/datart
```

# 本地部署升级

1. 从 [Github](https://github.com/running-elephant/datart/releases) 或 [Gitee](https://gitee.com/running-elephant/datart/releases) 下载新版本安装包
2. 解压安装包到新的目录，不要覆盖旧版本 datart 服务路径
3. 拷贝旧版本 datart 服务路径下的 `logs` 和 `files` 文件夹到新目录中
4. 重新配置新版本 datart 服务，参考[部署](index)章节的说明
5. 启动服务，完成升级

# 注意事项

- 每次升级之前，请备份 datart 应用数据库，有备无患
- 每次升级之前，请阅读新版本 release note 中的不兼容变更，以免造成生产事故
- 正常情况下升级失败时程序会自动回滚数据库到当前使用版本；如遇到异常情况，可以使用备份的数据库脚本回滚数据库

# alpha 版本升级指南

由于 alpha 版本没有提供自动升级程序，因此使用 alpha 版本的用户需要先手动升级到 beta.0 版本，然后按照[本地部署升级](#本地部署升级)章节的步骤升级到最新版本

在 [Github](https://github.com/running-elephant/datart/releases) 和 [Gitee](https://gitee.com/running-elephant/datart/releases) 中， 每一个 alpha 版本的 release note 里都提供了数据库升级须知，供参考
