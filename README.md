# 🍗 Nodeseek-Checkin

自动签到 NodeSeek 领取每日 5 鸡腿，基于 GitHub Actions 每天定时运行。

## 🔐 Secrets 配置

请在公库 **Nodeseek-Checkin** 的 **Settings → Secrets and variables → Actions → New repository secret** 中添加以下变量：

| Secret 名 | 说明 | 示例 |
| :--- | :--- | :--- |
| `NS_ACCOUNT` | 🧾 NodeSeek 登录邮箱和密码，用英文逗号分隔 | `example@mail.com,mypassword` |
| `GOST_PROXY` | 🌐 Gost 代理地址 | `socks5://user:pass@1.2.3.4:1080` |
| `TG_BOT` | 📨 Telegram 推送，Chat ID 和 Bot Token 用英文逗号分隔 | `987654321,123456:AAFxxx` |
| `PRIVATE_REPO_TOKEN` | 🔑 有私库读取权限的 GitHub PAT | `github_pat_xxxxxx` |

## 📝 PRIVATE_REPO_TOKEN 生成方式

GitHub → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)** → **Generate new token (classic)** → 勾选 `repo` + `workflow` → **Generate token** → 复制并填入 Secret。

## 📁 项目结构说明

本方案采用“公私库分离”策略，以最大程度保护脚本逻辑与账户安全：

* **公库 (`Nodeseek-Checkin`)**: 仅存放 `.github/workflows/nodeseek_checkin.yml`。
* **私库 (`Nodeseek-Checkin-Private`)**: 存放核心执行脚本 `nodeseek_checkin.py`。

## 🚀 运行机制

1.  **环境模拟**：通过 `xvfb` 开启虚拟显示器，模拟 1920x1080 真人操作环境。
2.  **过盾算法**：内置神级坐标倒推算法，配合 `xdotool` 物理火力打击 Cloudflare Turnstile 验证框。
3.  **结果推送**：签到完成后，通过 Telegram Bot 实时推送收益结果（鸡腿 x 5）。
