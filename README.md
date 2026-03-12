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

**格式一：统一密码**

```json
{
  "password": "统一密码",
  "accounts": [
    {"email": "email1@gmail.com", "name": "昵称1"},
    {"email": "email2@gmail.com", "name": "昵称2"}
  ]
}
```

**格式二：独立密码**

```json
{
  "accounts": [
    {"email": "email1@gmail.com", "password": "pass1", "name": "昵称1"},
    {"email": "email2@gmail.com", "password": "pass2", "name": "昵称2"}
  ]
}
```

> `name` 为可选字段，不填则自动使用邮箱前缀作为名字。填入 Secret 时请将 JSON 压缩为一行。

## 📝 PRIVATE_REPO_TOKEN 生成方式

GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token (classic) → 勾选 `repo` + `workflow` → Generate token → 填入 Secret。

## ⚙️ 运行说明

- 每天自动定时触发，无需手动操作
- 多账号依次签到，自动切换会话
- 已签到账号自动跳过，不重复提交
- 登录或签到失败的账号会在推送中单独列出
