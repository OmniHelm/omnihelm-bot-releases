# Changelog

## 说明

本文档记录所有版本的更新历史。

## [v2025.10.21.16](https://github.com/OmniHelm/omnihelm-bot-releases/releases/tag/v2025.10.21.16) (2025-10-21)

- fix: 修复 deploy.sh 自动检测下载模式的逻辑错误 (9090e79)


## [v2025.10.21.15](https://github.com/OmniHelm/omnihelm-bot-releases/releases/tag/v2025.10.21.15) (2025-10-21)

- fix: 修复 deploy.sh 的 latest 版本解析问题 (f9dc162)


## [v2025.10.21.14](https://github.com/OmniHelm/omnihelm-bot-releases/releases/tag/v2025.10.21.14) (2025-10-21)

- feat: 添加 latest 关键字支持，优化用户体验 (6bcbd38)


## [v2025.10.21.13](https://github.com/OmniHelm/omnihelm-bot-releases/releases/tag/v2025.10.21.13) (2025-10-21)

- fix: 修复 deploy.sh 下载模式检测输出污染问题 (c038d13)


## [v2025.10.21.12](https://github.com/OmniHelm/omnihelm-bot-releases/releases/tag/v2025.10.21.12) (2025-10-21)

- fix: 使用远程分支状态判断，消除对本地目录的依赖 (3778bf4)
- fix: 在创建 Release 前强制验证并等待默认分支设置生效 (85eef9e)
- fix: 修复空仓库创建 Release 失败 - 自动设置默认分支 (1dbd41a)
- fix: 修复空仓库无法自动初始化分支的问题 (78d4020)
- refactor: 优化公开仓库命名为 omnihelm-bot-releases (f19f112)
- feat: 实现多品牌 Release 自动同步到公开仓库 (81f9260)
- trigger: 触发首次 Release 发布 (718086b)
- fix: 修复 workflow 触发条件配置错误 (6f3718f)
- docs: 将 YOUR_ORG 占位符替换为实际组织名 OmniHelm (b908146)
- feat: 推送到 main 分支自动发布 Release (8f9a4f6)


---

