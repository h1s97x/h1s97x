# 快速开始

从零搭建 GitHub Profile 展示页面。

## 第一步：创建仓库

创建一个与 GitHub 用户名同名的仓库。

例如用户名为 `yourname`，则创建 `yourname/yourname` 仓库。该仓库的 `README.md` 会自动显示在你的个人主页上。

## 第二步：创建 Gist

1. 访问 [gist.github.com](https://gist.github.com/)
2. 创建一个新的 Gist（文件名随意，内容随意）
3. **重要**：将 Gist 设为 **Public（公开）**，否则 metrics 无法写入
4. 复制 Gist URL，格式为 `https://gist.github.com/yourname/<GIST_ID>`
5. 记录 `<GIST_ID>`（URL 中最后一串字符）

## 第三步：获取 GitHub Token

1. 访问 [GitHub Developer Settings](https://github.com/settings/tokens)
2. 点击 **Generate new token (classic)**
3. 配置：
   - **Note**：填 `metrics-token`
   - **Expiration**：建议 `No expiration` 或设置到期时间
   - **Scopes**：勾选 `repo`、`read:user`、`user:email`、`read:org`、`workflow`
4. 点击 **Generate token**
5. **立即复制保存**，离开页面后无法再次查看

## 第四步：（可选）获取 Steam 凭证

如果不需要 Steam 模块可跳过。

### 获取 Steam ID

访问 Steam 个人资料，URL 格式为：

```
https://steamcommunity.com/profiles/76561198xxxxxxxxx
```

数字部分即 64 位 Steam ID。

或使用 [steamid.io](https://steamid.io/) 查询。

### 获取 Steam API Key

1. 访问 [Steam API Key 申请](https://steamcommunity.com/dev/apikey)
2. 填写域名（可填 `localhost`）
3. 提交后获取 API Key

## 第五步：配置仓库 Secrets

进入仓库 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

| Secret 名称 | 值 | 必填 |
|-------------|-----|------|
| `METRICS_TOKEN` | GitHub Personal Access Token | 是 |
| `GIST` | Gist ID（第二步中记录的那串字符） | 是 |
| `STEAM_ID` | Steam 64 位 ID | 否 |
| `STEAM_TOKEN` | Steam Web API Key | 否 |

## 第六步：上传配置文件

将以下文件添加到仓库：

**`.github/workflows/metrics.yml`** - GitHub Actions 工作流（见项目根目录）

**`README.md`** - 个人主页

```markdown
[<img align="left" width="390" alt="🦑" src="https://gist.githubusercontent.com/YOUR_USERNAME/GIST_ID/raw/general.svg">](#)
[<img align="right" width="390" alt="🦑" src="https://gist.githubusercontent.com/YOUR_USERNAME/GIST_ID/raw/steam.svg">](#)

[<img width="100%" height="1" alt="🦑" src="https://gist.githubusercontent.com/lowlighter/3c6eaedf50273adfb7a510822672f570/raw/placeholder.svg">](#)

[<img align="left" width="390" alt="🦑" src="https://gist.githubusercontent.com/YOUR_USERNAME/GIST_ID/raw/anilist.svg">](#)
[<img align="right" width="390" alt="🦑" src="https://gist.githubusercontent.com/YOUR_USERNAME/GIST_ID/raw/achievements.svg">](#)
```

> 将 `YOUR_USERNAME` 和 `GIST_ID` 替换为你的实际值。

## 第七步：运行并验证

1. 进入仓库 → **Actions** 标签页
2. 选择 **Metrics** workflow
3. 点击 **Run workflow** → **Run workflow**
4. 等待 1-2 分钟运行完成
5. 访问你的 GitHub Profile 主页查看效果

如果页面没有更新，尝试：
- 手动触发 Actions 重新运行
- 清除浏览器缓存
- 在 SVG URL 后加 `?t=时间戳` 强制刷新
