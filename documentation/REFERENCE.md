# 配置参考

所有 metrics 插件的参数详解。完整插件列表见 [lowlighter/metrics 官方文档](https://github.com/lowlighter/metrics/blob/main/docs/plugins/README.md)。

---

## 基础参数

适用于所有插件。

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `filename` | 生成的 SVG 文件名 | - |
| `token` | GitHub Personal Access Token | 必填 |
| `base` | 基础样式模板，`header` 为带背景的完整卡片，`""` 为无背景 | `""` |
| `output_action` | 输出方式，`gist` 写入 Gist | 必填 |
| `committer_gist` | 目标 Gist ID | 必填 |

---

## isocalendar - 贡献日历

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_isocalendar` | 启用插件 | `no` |
| `plugin_isocalendar_delay` | 延迟加载（秒） | `0` |
| `plugin_isocalendar_limit` | 显示年份数 | `1` |
| `plugin_isocalendar_threshold` | 最小贡献数阈值 | `0` |
| `plugin_isocalendar_skipped` | 显示被排除的天数 | `no` |
| `plugin_isocalendar_details` | 显示每日详情 | `no` |
| `plugin_isocalendar_flags` | 显示标记 | `no` |
| `plugin_isocalendar_weeks` | 每周从哪天开始（0=周日，1=周一） | `0` |

---

## languages - 编程语言

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_languages` | 启用插件 | `no` |
| `plugin_languages_timeout` | 超时时间（毫秒） | `5000` |
| `plugin_languages_threshold` | 显示阈值（占总代码量百分比） | `0` |
| `plugin_languages_limit` | 最多显示语言数 | `0`（全部） |
| `plugin_languages_order` | 排序方式：`index`, `random`, `size` | `size` |
| `plugin_languages_ignored` | 忽略的语言（逗号分隔） | - |
| `plugin_languages_sections` | 显示分区 | `overview, breakdown` |
| `plugin_languages_aliases` | 语言别名映射 | - |
| `plugin_languages_skipped` | 显示被忽略的语言 | `no` |

---

## topics - 仓库主题

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_topics` | 启用插件 | `no` |
| `plugin_topics_limit` | 显示数量，0 为全部 | `0` |
| `plugin_topics_mode` | 显示模式：`icons`, `stars`, `default` | `default` |

---

## notable - 热门项目

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_notable` | 启用插件 | `no` |
| `plugin_notable_repositories` | 显示仓库数 | `6` |
| `plugin_notable_sections` | 显示分区：`repositories, stars` | `repositories` |

---

## discussions - 讨论

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_discussions` | 启用插件 | `no` |

---

## achievements - GitHub 成就

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_achievements` | 启用插件 | `no` |
| `plugin_achievements_limit` | 显示数量 | `0`（全部） |
| `plugin_achievements_order` | 排序方式：`random`, `recent` | `random` |
| `plugin_achievements_display` | 显示模式：`compact`, `separate` | `separate` |

---

## anilist - 动漫

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_anilist` | 启用插件 | `no` |
| `plugin_anilist_user` | AniList 用户名 | 必填 |
| `plugin_anilist_medias` | 媒体类型：`anime`, `manga` | `anime` |
| `plugin_anilist_sections` | 显示分区：`favorites, watching, planned, completed, dropped, paused, repeating` | - |
| `plugin_anilist_limit` | 每分区显示数量 | `5` |
| `plugin_anilist_limit_characters` | 收藏角色数量 | `5` |
| `plugin_anilist_strategy` | 排序策略：`score`, `progress`, `repeat`, `random` | `random` |
| `plugin_anilist_about` | 显示简介 | `no` |
| `plugin_anilist_stats` | 显示统计 | `no` |
| `plugin_anilist_information` | 显示账号信息 | `no` |
| `plugin_anilist_relations` | 显示关联作品 | `no` |
| `plugin_anilist_trends` | 显示趋势图 | `no` |

---

## steam - Steam 游戏

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `plugin_steam` | 启用插件 | `no` |
| `plugin_steam_user` | Steam ID（64位） | 必填 |
| `plugin_steam_token` | Steam Web API Key | 必填 |
| `plugin_steam_sections` | 显示分区：`player, most-played, recently-played, achievement` | - |
| `plugin_steam_games_limit` | 最常游戏显示数量 | `5` |
| `plugin_steam_games_recent_limit` | 最近游戏显示数量 | `5` |
| `plugin_steam_playtime_days` | 统计天数范围 | `14` |
| `plugin_steam_achievements_limit` | 成就显示数量 | `3` |
| `plugin_steam_player_details` | 显示玩家详情 | `no` |
| `plugin_steam_player_badges` | 显示玩家徽章 | `no` |
| `plugin_steam_player_xp` | 显示等级经验 | `no` |
| `plugin_steam_leaderboards` | 显示排行榜 | `no` |
| `plugin_steam_ignores` | 忽略的游戏（App ID 逗号分隔） | - |

---

## 常用配置模板

### 最小配置（仅 General）

```yaml
- name: General
  uses: lowlighter/metrics@latest
  with:
    filename: general.svg
    token: ${{ secrets.METRICS_TOKEN }}
    base: header
    output_action: gist
    committer_gist: ${{ secrets.GIST }}
    plugin_isocalendar: yes
    plugin_languages: yes
```

### 自定义更新频率

```yaml
# 每 6 小时
schedule: [{cron: "0 */6 * * *"}]

# 每周一
schedule: [{cron: "0 0 * * 1"}]

# 每天凌晨
schedule: [{cron: "0 0 * * *"}]
```

### 禁用不需要的插件

在 `config_order` 中移除对应项，或在 YAML 中将 `plugin_xxx: yes` 改为 `plugin_xxx: no`。
