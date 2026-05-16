# 常见问题

按问题类型分类的排查指南。

---

## Actions 运行失败

### Workflow 一直处于等待状态

**原因**：`production` environment 未配置审批规则。

**解决**：进入仓库 → **Settings** → **Environments** → 选择 `production` → 检查是否有待审批的规则，移除或审批。

### Token 相关错误

**原因**：Token 过期或权限不足。

**解决**：
1. 确认 `METRICS_TOKEN` Secrets 值正确
2. 确认 Token 未过期（在 [Developer Settings](https://github.com/settings/tokens) 检查）
3. 确认 Token 包含必要权限：`repo`、`read:user`、`user:email`、`read:org`、`workflow`

### Gist 写入失败

**原因**：Gist 未公开或 Gist ID 错误。

**解决**：
1. 确认 Gist 设为 **Public**
2. 确认 `GIST` Secrets 中的 ID 正确（是 Gist URL 中最后一串字符，不是完整 URL）
3. 确认 GitHub Token 对应的账号有该 Gist 的写入权限

---

## 模块显示异常

### 模块显示为空或 "Unexpected error"

**可能原因**：
- Secrets 配置错误
- API 调用频率限制
- 账号无相关数据

**排查步骤**：
1. 进入 **Actions** → 点击失败的 workflow run → 查看日志
2. 定位具体哪个 step 失败，查看错误信息
3. 根据错误信息针对性解决

### AniList 模块为空

**原因**：AniList 账号没有数据或用户名错误。

**解决**：
1. 确认 `plugin_anilist_user` 填写的是 AniList 用户名（不是 GitHub 用户名）
2. 在 AniList 添加收藏或正在看的动漫
3. 如使用 [MalSync](https://malsync.moe/) 同步，可从 Bilibili 等平台导入

### Steam 模块显示错误

**原因**：Steam 隐私设置阻止数据获取。

**解决**：
1. 登录 Steam → 点击右上角个人资料 → **Edit Profile**
2. 进入 **Privacy Settings**
3. 将「Game details」设为 **Public**
4. 将「Inventory」设为 **Public**（如需显示库存）

### GitHub 成就不显示

**原因**：成就未解锁或 GitHub 未同步。

**解决**：
1. 访问 [github.com/achievements](https://github.com/achievements) 查看已解锁成就
2. 成就需要手动刷新才会出现在 API 中，尝试完成新成就后等待几天

---

## 页面不更新

### SVG 图片不刷新

**原因**：浏览器或 CDN 缓存。

**解决**：
1. 清除浏览器缓存后刷新页面
2. 在 SVG URL 后手动添加时间戳：`?t=1234567890`
3. 手动触发 Actions 重新运行
4. 等待最长 24 小时（GitHub Gist 有 CDN 缓存）

### 图片显示红叉或 404

**原因**：Gist 内容被删除或不可访问。

**解决**：
1. 访问 Gist 确认文件存在
2. 确认 Gist 仍为公开状态
3. 手动触发 Actions 重新生成

---

## 样式问题

### 卡片宽度不统一

**原因**：README 中图片 `width` 设置不当。

**解决**：调整 `width` 值使两个卡片宽度之和不超过页面宽度，建议总宽度 ≤ 780px。

### 卡片之间有间隙

**原因**：`align="left"` 和 `align="right"` 的卡片默认会有行高间隙。

**解决**：在分隔线 `<img>` 中使用 `width="100%"` 和 `height="1"` 强制换行。

---

## 其他

### 如何禁用某个模块？

在 `metrics.yml` 中找到对应 step，将整个 step 删除或注释掉，同时从 `README.md` 中移除对应的 `<img>` 标签。

### 如何添加更多模块？

参考 [REFERENCE.md](REFERENCE.md) 中的参数说明，在 `metrics.yml` 中添加新的 step，启用相应插件。

### 可以使用自己的 Gist 存储生成的图片吗？

可以。将 `output_action` 改为 `gist`，`committer_gist` 填入你自己的 Gist ID。注意 Gist 必须为公开状态。

### metrics 更新频率怎么调？

在 `metrics.yml` 的 `on.schedule` 中修改 cron 表达式。默认每天 UTC 0:00 更新。

```yaml
on:
  schedule: [{cron: "0 */6 * * *"}]  # 每 6 小时
  workflow_dispatch:  # 允许手动触发
```
