# 🍗 NodeSeek-Checkin

自动签到 NodeSeek，每天定时运行，支持多账号批量签到并推送 Telegram 通知。

## 🔐 Secrets 配置

**Settings → Secrets and variables → Actions → New repository secret** 添加以下变量：

| Secret 名 | 说明 | 示例 |
|---|---|---|
| `NS_ACCOUNT` | 🧾 账号列表与密码，JSON 格式，见下方说明 | — |
| `GOST_PROXY` | 🌐 Gost 代理地址 | `socks5://user:pass@1.2.3.4:1080` |
| `TG_BOT` | 📨 Telegram 推送，Chat ID 和 Bot Token 用英文逗号分隔 | `987654321,123456:AAFxxx` |
| `PRIVATE_REPO_TOKEN` | 🔑 有私库读取权限的 GitHub PAT | `github_pat_xxxxxx` |

### NS_ACCOUNT 格式

**格式一：统一密码**（所有账号同一个密码）
```json
{
  "password": "统一密码",
  "accounts": [
    "email1@gmail.com",
    "email2@gmail.com"
  ]
}
```

**格式二：独立密码**（每个账号单独密码）
```json
{
  "accounts": [
    {"email": "email1@gmail.com", "password": "pass1"},
    {"email": "email2@gmail.com", "password": "pass2"}
  ]
}
```

**格式三：混用**（顶层密码作为默认值，单个账号可单独覆盖）
```json
{
  "password": "默认密码",
  "accounts": [
    "email1@gmail.com",
    {"email": "email2@gmail.com", "password": "独立密码"}
  ]
}
```

> 填入 Secret 时请将 JSON 压缩为一行。

## 📝 PRIVATE_REPO_TOKEN 生成方式

GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token (classic) → 勾选 `repo` + `workflow` → Generate token → 填入 Secret。

## 📝 TG_BOT 获取方式

1. Telegram 搜索 `@BotFather` → `/newbot` → 创建机器人 → 获取 Bot Token
2. 搜索 `@userinfobot` → 发送任意消息 → 获取你的 Chat ID
3. 将两者用英文逗号拼接填入 Secret，格式：`chatid,token`

## 📨 推送示例
```
🍗 NodeSeek 多账号签到汇报
🕐 运行时间: 2025-01-01 08:00:00
📊 共21个账号  ✅19成功  ☑️2已签  ❌0失败

✅ 本次签到成功：
  · email1@gmail.com  +5 🍗
  · email2@gmail.com  +5 🍗

☑️ 今日已签到：
  · email3@aol.com  +5 🍗
```

## ⚙️ 运行说明

- 每天自动定时触发，无需手动操作
- 多账号依次签到，自动切换会话
- 已签到账号自动跳过，不重复提交
- 登录或签到失败的账号会在推送中单独列出
