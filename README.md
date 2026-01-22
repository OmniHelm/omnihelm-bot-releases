# OmniHelm Bot - brand-gomami 版本

围绕 Emoji 状态管理与抽奖系统构建的 Telegram Bot，面向 Premium 群组提供自定义 Emoji 状态与抽奖活动的核心能力。

---

## 🚀 快速开始

### 一键部署（推荐）

```bash
# 部署最新版本
bash -c "$(curl -fsSL https://github.com/OmniHelm/omnihelm-bot-releases/releases/latest/download/deploy.sh)"
```

### 指定版本部署

```bash
# 查看所有版本: https://github.com/OmniHelm/omnihelm-bot-releases/releases
export OMNIHELM_VERSION=v2025.01.21.1
bash -c "$(curl -fsSL https://github.com/OmniHelm/omnihelm-bot-releases/releases/download/${OMNIHELM_VERSION}/deploy.sh)"
```

---

## 📋 环境要求

| 项目 | 要求 |
|------|------|
| Docker | 20.10+ |
| Docker Compose | V2 (2.0+) |
| 系统 | Linux / macOS / Windows (WSL2) |
| 端口 | 8000 (Web), 3306 (MySQL) |

---

## 📦 首次部署

### 1. 运行部署脚本

```bash
bash -c "$(curl -fsSL https://github.com/OmniHelm/omnihelm-bot-releases/releases/latest/download/deploy.sh)"
```

脚本会自动：
- ✅ 检查 Docker 环境
- ✅ 交互式配置环境变量
- ✅ 验证 Telegram Token
- ✅ 拉取 Docker 镜像
- ✅ 启动所有服务
- ✅ 初始化数据库

### 2. 配置项说明

部署过程中需要配置以下信息：

| 配置项 | 说明 | 获取方式 |
|--------|------|----------|
| `TELEGRAM_TOKEN` | Bot Token | [@BotFather](https://t.me/BotFather) |
| `TELEGRAM_API_ID` | API ID | [my.telegram.org/apps](https://my.telegram.org/apps) |
| `TELEGRAM_API_HASH` | API Hash | [my.telegram.org/apps](https://my.telegram.org/apps) |
| `GROUP_ID` | 群组 ID | [@getidsbot](https://t.me/getidsbot) |
| `ADMIN_IDS` | 管理员 ID | [@getidsbot](https://t.me/getidsbot) |
| `WEB_BASE_URL` | HTTPS 域名 | 自定义（生产环境必须） |
| `DB_PASSWORD` | 数据库密码 | 自定义强密码 |

### 3. 首次认证

服务启动后，在 Telegram 中与 Bot 私聊：

```
/session_auth
```

按提示输入手机号和验证码完成 Telethon 认证。

### 4. 验证部署

```bash
# 查看服务状态
docker compose ps

# 查看日志
docker compose logs -f

# 健康检查
curl http://localhost:8000/health
```

---

## 🔄 升级与回滚

### 升级到最新版本

```bash
# 方式 1: 重新运行部署脚本（推荐）
bash -c "$(curl -fsSL https://github.com/OmniHelm/omnihelm-bot-releases/releases/latest/download/deploy.sh)"

# 方式 2: 手动拉取镜像
docker compose pull
docker compose up -d
```

### 回滚到指定版本

```bash
# 1. 下载指定版本部署包
export VERSION=v2025.01.21.1
curl -LO https://github.com/OmniHelm/omnihelm-bot-releases/releases/download/${VERSION}/omnihelm-bot-deploy-${VERSION}.tar.gz

# 2. 解压并部署
tar -xzf omnihelm-bot-deploy-${VERSION}.tar.gz
cd omnihelm-bot-deploy
./deploy.sh
```

### 版本号规则

| 格式 | 说明 | 示例 |
|------|------|------|
| `v{YYYY.MM.DD}.{N}` | 自动发布（每日构建） | `v2025.01.21.1` |
| `v{X.Y.Z}` | 手动发布（里程碑版本） | `v1.0.0` |

---

## 🛠️ 服务管理

### 常用命令速查

| 操作 | 命令 |
|------|------|
| 查看服务状态 | `docker compose ps` |
| 查看实时日志 | `docker compose logs -f` |
| 查看特定服务日志 | `docker compose logs -f bot` |
| 重启服务 | `docker compose restart` |
| 停止服务 | `docker compose down` |
| 启动服务 | `docker compose up -d` |
| 更新镜像 | `docker compose pull` |
| 备份数据库 | `docker compose exec mysql mysqldump -u root -p omnihelm_bot > backup.sql` |

### 配置管理

```bash
# 查看当前配置
cat .env

# 修改配置后重启
vim .env
docker compose restart bot

# 重新加载 Bot 配置（无需重启）
# 在 Telegram 中发送：/config_reload
```

---

## ⚙️ 配置说明

### 必需环境变量

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `TELEGRAM_TOKEN` | Telegram Bot Token | - |
| `TELEGRAM_API_ID` | Telegram API ID | - |
| `TELEGRAM_API_HASH` | Telegram API Hash | - |
| `GROUP_ID` | 群组 ID | - |
| `ADMIN_IDS` | 管理员 ID（逗号分隔） | - |
| `WEB_BASE_URL` | Web 域名（HTTPS） | - |
| `MINI_APP_URL` | Mini App URL | - |
| `DB_PASSWORD` | 数据库密码 | - |

### 可选环境变量

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `LOG_LEVEL` | 日志级别 | `INFO` |
| `DB_HOST` | 数据库主机 | `mysql` |
| `DB_PORT` | 数据库端口 | `3306` |
| `DB_NAME` | 数据库名 | `omnihelm_bot` |

详细配置参考：[.env.example](https://github.com/OmniHelm/omnihelm-bot-releases/releases/latest/download/.env.example)

---

## 🐛 故障排查

### 问题 1: 容器启动失败

```bash
# 查看详细日志
docker compose logs bot

# 检查环境变量
docker compose config

# 常见原因：
# - Token 配置错误
# - 端口被占用（8000/3306）
# - 数据库连接失败
```

### 问题 2: Session 认证失效

```bash
# 症状：Telethon 功能失效
# 解决：重新认证
# 在 Telegram 中发送: /session_auth

# 或手动删除 Session 文件
docker compose exec bot rm -f /app/backend/*.session
docker compose restart bot
```

### 问题 3: 端口冲突

```bash
# 查看占用端口的进程
sudo lsof -i :8000
sudo lsof -i :3306

# 修改 docker-compose.yml 端口映射
# 例如: "8080:8000" 或 "3307:3306"
```

### 问题 4: 数据库连接失败

```bash
# 测试数据库连接
docker compose exec mysql mysql -u root -p

# 检查 MySQL 容器状态
docker compose ps mysql
docker compose logs mysql

# 验证数据库初始化
docker compose exec mysql mysql -u root -p -e "SHOW DATABASES;"
```

### 问题 5: 镜像拉取失败

```bash
# 国内用户可能需要配置镜像加速
# 参考: https://docs.docker.com/registry/recipes/mirror/

# 或手动下载离线包部署
curl -LO https://github.com/OmniHelm/omnihelm-bot-releases/releases/latest/download/omnihelm-bot-deploy-latest.tar.gz
tar -xzf omnihelm-bot-deploy-latest.tar.gz
cd omnihelm-bot-deploy
./deploy.sh
```

---

## 📚 更多资源

- **完整文档**: [主仓库 README](https://github.com/OmniHelm/OmniHelm-bot)
- **版本历史**: [Releases](https://github.com/OmniHelm/omnihelm-bot-releases/releases)
- **更新日志**: [CHANGELOG.md](./CHANGELOG.md)
- **问题反馈**: [GitHub Issues](https://github.com/OmniHelm/omnihelm-bot-releases/issues)

---

## 📄 许可证

MIT License

---

**最后更新**: 2026-01-22
