# 🚀 Openworld Free IPv6 VPS 自动续期脚本
# 既然来了，就留下你的Star吧！

基于 GitHub Actions 的 **Openworld Free IPv6 VPS** 全自动续期工具。采用 Playwright 自动化技术 + 智能 Discord OAuth 授权 + 多帧 GIF 动态验证码解析，实现无须人工干预的永久续期。

---

## 🌟 功能特性

- 🔑 **Discord OAuth 免干预登录**：利用账号的 `DISCORD_TOKEN` 向 Discord API 提交直接授权，跳过复杂的网页交互。
- 🧩 **多帧 GIF 算式验证码识别**：
  - 自动在浏览器上下文中获取 `blob:` 类型的多帧 GIF 动态验证码。
  - **拆帧 + 分区切割**：提取 GIF 的所有帧，将画面切割为左半区（数字A）、中区（运算符）、右半区（数字B）。
  - **模糊映射与跨帧投票**：清洗字符并映射误识别符号，利用跨帧概率统计得出高准确度的算式并自动计算结果。
- ⏱️ **智能天数检测**：自动解析面板当前的剩余到期天数，仅当剩余时间 `<= 5 天` 时才触发续期，避免无谓请求。
- 🌟 **智能重启**：自动检测服务器运行状态，当服务器停止运行时，会自动触发启动按钮，运行正常后继续续期任务。
- 📢 **Telegram 结果通知**：可选配置 Telegram Bot，续期成功或失败时自动推送最新状态。
- 📸 **自动保存验证码GIF**：自动保存验证码GIF，在 GitHub Actions 中保存为 Artifacts 便于排查。

---

## 🔐 GitHub Secrets 配置说明

在 GitHub 仓库依次点击 **Settings** ➔ **Secrets and variables** ➔ **Actions** ➔ **New repository secret** 配置以下变量：

| Secret 名称 | 是否必填 | 说明 |
| :--- | :---: | :--- |
| `DISCORD_TOKEN` | **必填** | 你的 Discord 账号授权 Token（获取方式见下文） |
| `TG_BOT_TOKEN` | ❌ 可选 | Telegram Bot Token（用于接收续期结果通知） |
| `TG_CHAT_ID` | ❌ 可选 | Telegram Chat ID（接收通知的用户或群组 ID） |

> [!NOTE]
> 无需手动配置 VPS 地址，脚本登录后会**自动从面板检测**账号下所有 VPS 实例并逐一续期。

---

## 🛠️ GitHub Actions 部署指南

1. **Fork 本仓库** 到你自己的 GitHub 账号下。
2. **开启 Actions 权限**：在仓库的 **Actions** 标签页中点击按钮允许运行工作流。
3. **添加 Secrets**：在 **Settings ➔ Secrets and variables ➔ Actions** 中添加 `DISCORD_TOKEN`（如需 Telegram 通知，同时添加 `TG_BOT_TOKEN` 和 `TG_CHAT_ID`）。
4. **手动测试运行**：
   - 进入 **Actions** 标签页。
   - 选择左侧的 **Auto Renew Openworld VPS** 工作流。
   - 点击 **Run workflow** 按钮启动测试。
5. **定时自动运行**：工作流默认每 2 天自动触发运行一次，实现完全无人值守。

---

## 🔍 如何获取 Discord Token

1. 使用电脑浏览器打开 [Discord 网页版](https://discord.com/app) 并登录你的账号。
2. 按 `F12`（或 `Ctrl + Shift + I`）打开开发者工具。
3. 切换到 **网络 (Network)** 标签页。
4. 在 Discord 中点击任意频道或点击Openworld Inc.频道，触发 API 请求。
5. 在网络请求列表中点击任意 `discord.com/api/science` 开头的请求science。
6. 在右侧 **请求标头 (Request Headers)** 中找到 `Authorization` 字段，该字段对应的长字符串即为 **DISCORD_TOKEN**。

> ⚠️ **安全提示**：请妥善保管你的 Discord Token，切勿泄露给他人。

---

## ⚠️ 免责声明

- 本脚本仅供个人自动化运维及 Python 自动化学习交流使用。
- 请遵守 Openworld 平台的服务条款 (Terms of Service)，作者不对任何使用不当导致的账号问题负责。
