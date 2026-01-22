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

## 🎨 品牌定制

OmniHelm Bot 支持完整的品牌定制，无需修改代码即可适配不同群组。

### 可配置项一览

| 配置项 | 默认值 | 说明 |
|--------|--------|------|
| `group_name` | `OmniHelm` | 群组简称（拼接用，如 "OmniHelm标"） |
| `group_display_name` | `OmniHelm 家族` | 群组完整名称（显示用） |
| `group_member_title` | `家人` | 群组成员称呼（如 "家人", "伙伴"） |
| `emoji_suffix` | `标` | Emoji 标识后缀（如 "标", "徽章"） |
| `system_name` | `OmniHelm Bot` | Bot 系统名称 |
| `system_title` | `OmniHelm 抽奖系统` | 系统标题（网页使用） |
| `brand_logo_url` | 默认 Logo URL | Logo 图片 URL |
| `brand_slogan` | `让每一次抽奖都充满惊喜` | 品牌 Slogan |

### 配置方法

#### 方法 1: 使用管理员命令（推荐）

在 Telegram 中与 Bot 私聊，发送以下命令：

```bash
# 查看所有配置
/config_get

# 修改单个配置
/config_set group_name "ABC"
/config_set group_display_name "ABC 联盟"
/config_set group_member_title "伙伴"
/config_set emoji_suffix "徽章"

# 刷新配置缓存（使新配置生效）
/config_reload
```

#### 方法 2: 直接修改数据库

```bash
# 连接到数据库容器
docker compose exec mysql mysql -u root -p

# 执行 SQL 语句
USE omnihelm_bot;

-- 修改群组名称
UPDATE bot_settings SET setting_value = 'ABC' WHERE setting_key = 'group_name';

-- 修改 Emoji 后缀
UPDATE bot_settings SET setting_value = '徽章' WHERE setting_key = 'emoji_suffix';

-- 修改系统名称
UPDATE bot_settings SET setting_value = 'ABC Bot' WHERE setting_key = 'system_name';

-- 查看所有配置
SELECT * FROM bot_settings;
```

**修改后需要在 Telegram 中发送 `/config_reload` 刷新缓存。**

### 定制示例

#### 场景 1: 迁移到新品牌 "ABC 联盟"

```bash
# 在 Telegram 中发送：
/config_set group_name "ABC"
/config_set group_display_name "ABC 联盟"
/config_set group_member_title "伙伴"
/config_set emoji_suffix "徽章"
/config_set system_name "ABC Bot"
/config_set system_title "ABC 抽奖系统"
/config_reload
```

**定制效果：**
- Emoji 标识名称：`OmniHelm标` → `ABC徽章`
- 群组称呼：`OmniHelm 家族` → `ABC 联盟`
- 成员称呼：`家人` → `伙伴`
- 系统名称：`OmniHelm Bot` → `ABC Bot`

#### 场景 2: 仅修改 Emoji 后缀

```bash
# 将 "OmniHelm标" 改为 "OmniHelm徽章"
/config_set emoji_suffix "徽章"
/config_reload
```

#### 场景 3: 自定义 Logo 和 Slogan

```bash
/config_set brand_logo_url "https://yourdomain.com/logo.png"
/config_set brand_slogan "您的品牌标语"
/config_reload
```

### 配置管理命令

| 命令 | 说明 | 示例 |
|------|------|------|
| `/config_get` | 查看所有配置 | `/config_get` |
| `/config_get <key>` | 查看指定配置 | `/config_get group_name` |
| `/config_set <key> <value>` | 修改配置 | `/config_set group_name "ABC"` |
| `/config_reload` | 刷新配置缓存 | `/config_reload` |

**注意事项：**
1. 修改配置后必须执行 `/config_reload` 才能生效
2. 所有配置命令仅限管理员使用
3. 配置修改后，用户看到的所有文案会自动更新

### 文案自动适配

品牌定制后，以下所有文案会自动适配新配置：

- ✅ Emoji 状态名称（如 "OmniHelm标" → "ABC徽章"）
- ✅ 群组称呼（如 "OmniHelm 家族" → "ABC 联盟"）
- ✅ 成员称呼（如 "家人" → "伙伴"）
- ✅ 抽奖条件标签（如 "需挂 OmniHelm标" → "需挂 ABC徽章"）
- ✅ 网页标题和 Logo
- ✅ 所有 Bot 命令回复文案

**无需重启服务，无需修改代码，真正的零成本品牌切换！**

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

**最后更新**: $(date '+%Y-%m-%d')
