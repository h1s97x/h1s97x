# GitHub 个人主页配置指南

本项目使用 [lowlighter/metrics](https://github.com/lowlighter/metrics) 生成动态的 GitHub 个人资料统计卡片。

---

## 📋 项目结构

```
h1s97x/
├── .github/
│   └── workflows/
│       └── metrics.yml      # GitHub Actions 工作流配置
├── README.md                # 个人主页展示文件
└── documentation/           # 文档目录
```

---

## 🎨 展示模块

本项目包含 **4 个展示模块**：

| 模块             | 文件               | 显示内容                                                 |
| ---------------- | ------------------ | -------------------------------------------------------- |
| **General**      | `general.svg`      | 头像、日历贡献图、编程语言统计、热门项目、讨论、主题标签 |
| **Steam**        | `steam.svg`        | Steam 玩家信息、最常玩游戏、最近玩游戏、游戏成就         |
| **Medias**       | `medias.svg`       | AniList 动漫追番、收藏角色                               |
| **Achievements** | `achievements.svg` | GitHub 成就徽章                                          |

### 页面布局

```
┌─────────────────────┬─────────────────────┐
│       General       │        Steam        │
├─────────────────────┴─────────────────────┤
│                  分隔线                    │
├─────────────────────┬─────────────────────┤
│       Medias        │    Achievements     │
└─────────────────────┴─────────────────────┘
```

---

## ⚙️ 配置步骤

### 第一步：创建个人主页仓库

1. 创建一个与你的 GitHub 用户名同名的仓库（如 `h1s97x/h1s97x`）
2. 该仓库的 `README.md` 会自动显示在你的个人主页上

### 第二步：创建 Gist（用于存储生成的 SVG）

1. 访问 [GitHub Gist](https://gist.github.com/)
2. 创建一个新的 Gist（内容随意）
3. 记录 Gist URL 中的 ID（格式：`https://gist.github.com/h1s97x/<GIST_ID>`）

### 第三步：创建 GitHub Personal Access Token

1. 访问：[GitHub Developer Settings](https://github.com/settings/tokens)
2. 点击 **Generate new token (classic)**
3. 填写信息：
   - **Note**: `metrics-token`
   - **Expiration**: `No expiration`（或选择一个期限）
   - **Scopes**: 勾选以下权限
     - ✅ `repo` (Full control of private repositories)
     - ✅ `read:user` (Read user profile data)
     - ✅ `user:email` (Access user email addresses)
     - ✅ `read:org` (Read org and team membership)
4. 点击 **Generate token**
5. **立即复制并保存** Token（离开页面后无法再次查看）

### 第四步：获取 Steam API 凭证

#### 4.1 获取 Steam ID（64位ID）

**方法一：从个人资料 URL 获取**
- 访问你的 Steam 个人资料页面
- URL 格式：`https://steamcommunity.com/profiles/76561198xxxxxxxxx`
- 数字部分就是你的 Steam ID

**方法二：使用工具查询**
- 访问：[steamid.io](https://steamid.io/)
- 输入你的 Steam 自定义昵称
- 查找 **steamID64** 字段

#### 4.2 获取 Steam Web API Key

1. 访问：[Steam API Key](https://steamcommunity.com/dev/apikey)
2. 登录你的 Steam 账号
3. 填写域名（可填 `localhost` 或任意域名）
4. 点击注册，获取 API Key

### 第五步：配置仓库 Secrets

进入你的仓库 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

添加以下 Secrets：

| Secret 名称     | 值                     | 说明                         |
| --------------- | ---------------------- | ---------------------------- |
| `METRICS_TOKEN` | `ghp_xxxxxxxxxxxx`     | GitHub Personal Access Token |
| `GIST`          | `xxxxxxxxxxxxxxxxxxxx` | Gist ID（URL 中那串字符）    |
| `STEAM_ID`      | `76561198xxxxxxxxx`    | Steam 64位ID                 |
| `STEAM_TOKEN`   | `xxxxxxxxxxxxxxxx`     | Steam Web API Key            |

### 第六步：上传配置文件

将以下两个文件上传到仓库：

**`.github/workflows/metrics.yml`**

```yaml
name: Metrics
on:
  schedule: [{cron: "0 0 * * *"}]
  workflow_dispatch:
jobs:
  metrics:
    runs-on: ubuntu-latest
    environment:
      name: production
    permissions:
      contents: write
    steps:
      - name: 🦑 General
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: general.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: header
          base_indepth: yes
          config_order: base.header, isocalendar, languages, notable, discussions, topics
          output_action: gist
          committer_gist: ${{ secrets.GIST }} 
          plugin_isocalendar: yes
          plugin_languages: yes
          plugin_topics: yes
          plugin_topics_limit: 0
          plugin_topics_mode: icons
          plugin_notable: yes
          plugin_discussions: yes
          
      - name: 🦑 Medias
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: medias.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.GIST }}
          plugin_anilist: yes
          plugin_anilist_user: h1s97x
          plugin_anilist_medias: anime
          plugin_anilist_sections: watching, characters
          plugin_anilist_limit: 4
          plugin_anilist_limit_characters: 8
          
      - name: 🦑 Achievements
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: achievements.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.GIST }}
          plugin_achievements: yes
          plugin_achievements_display: compact

      - name: 🦑 Steam
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: steam.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.GIST }}
          plugin_steam: yes
          plugin_steam_user: ${{ secrets.STEAM_ID }}
          plugin_steam_token: ${{ secrets.STEAM_TOKEN }}
          plugin_steam_sections: player, most-played, recently-played
          plugin_steam_games_limit: 8
          plugin_steam_playtime_days: 30
          plugin_steam_achievements_limit: 2
```

**`README.md`**

```markdown
[<img align="left" width="390" alt="🦑" src="https://gist.githubusercontent.com/h1s97x/1b64184fcfdbc564141eb8fe11433154/raw/general.svg">](#)
[<img align="right" width="390" alt="🦑" src="https://gist.githubusercontent.com/h1s97x/1b64184fcfdbc564141eb8fe11433154/raw/steam.svg">](#)

[<img width="100%" height="1" alt="🦑" src="https://gist.githubusercontent.com/lowlighter/3c6eaedf50273adfb7a510822672f570/raw/placeholder.svg">](#)

[<img align="left" width="390" alt="🦑" src="https://gist.githubusercontent.com/h1s97x/1b64184fcfdbc564141eb8fe11433154/raw/medias.svg">](#)
[<img align="right" width="390" alt="🦑" src="https://gist.githubusercontent.com/h1s97x/1b64184fcfdbc564141eb8fe11433154/raw/achievements.svg">](#)
```

> ⚠️ **注意**：将 README.md 中的 Gist ID 替换为你自己的

### 第七步：运行 Workflow

1. 进入仓库的 **Actions** 标签页
2. 选择 **Metrics** workflow
3. 点击 **Run workflow** → **Run workflow**
4. 等待运行完成（约 1-2 分钟）
5. 刷新你的个人主页查看效果

---

## 🔧 自定义配置

### 修改 AniList 用户名

在 `metrics.yml` 中找到 Medias 部分，修改：

```yaml
plugin_anilist_user: your_username  # 改为你的 AniList 用户名
```

### 修改显示的动漫内容

```yaml
plugin_anilist_sections: watching, characters
# 可选值：
# - favorites     收藏的动漫
# - watching      正在看
# - characters    收藏角色
```

### 修改 Steam 显示内容

```yaml
plugin_steam_sections: player, most-played, recently-played
# 可选值：
# - player           玩家信息
# - most-played      最常玩游戏
# - recently-played  最近玩游戏
```

### 修改显示数量

```yaml
plugin_steam_games_limit: 8        # 显示的游戏数量
plugin_steam_achievements_limit: 2 # 显示的成就数量
```

### 修改更新频率

```yaml
schedule: [{cron: "0 0 * * *"}]  # 每天 UTC 00:00
# 其他示例：
# schedule: [{cron: "0 */6 * * *"}]  # 每 6 小时
# schedule: [{cron: "0 0 * * 1"}]    # 每周一
```

---

## ❓ 常见问题

### Q1: 模块显示为空或 "Unexpected error"

**可能原因：**
1. Secrets 配置错误
2. Token 权限不足
3. API 调用限制

**解决方法：**
1. 检查所有 Secrets 是否正确配置
2. 确保 `METRICS_TOKEN` 有 `repo` 和 `read:user` 权限
3. 查看 Actions 运行日志定位具体错误

### Q2: AniList/Medias 模块为空

**原因：** AniList 账号没有相关数据

**解决方法：**
1. 在 AniList 添加收藏或正在看的动漫
2. 或使用 [MalSync](https://malsync.moe/) 从 Bilibili 同步数据

### Q3: Steam 模块显示错误

**可能原因：**
1. Steam 个人资料隐私设置

**解决方法：**
1. 访问 Steam 设置 → 隐私设置
2. 将「游戏详情」设为「公开」

### Q4: SVG 图片不更新

**解决方法：**
1. 手动触发 Actions 运行
2. 清除浏览器缓存
3. 在 URL 后添加 `?t=timestamp` 强制刷新

### Q5: 如何查看 Gist ID？

Gist URL 格式：`https://gist.github.com/用户名/GIST_ID`

例如：`https://gist.github.com/h1s97x/1b64184fcfdbc564141eb8fe11433154`

Gist ID = `1b64184fcfdbc564141eb8fe11433154`

---

## 📚 参考资源

- [lowlighter/metrics 官方文档](https://github.com/lowlighter/metrics)
- [metrics 插件列表](https://github.com/lowlighter/metrics/blob/master/source/plugins/README.md)
- [GitHub Personal Access Tokens](https://github.com/settings/tokens)
- [Steam API Key](https://steamcommunity.com/dev/apikey)
- [AniList](https://anilist.co/)
- [MalSync（Bilibili → AniList 同步）](https://malsync.moe/)

---

## 📝 更新日志

- **2026-03**：添加 Steam 游戏模块
- **2026-03**：修复 Medias 和 Achievements 显示问题
- **初始版本**：使用 lowlighter/metrics 创建个人主页
