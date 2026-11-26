# Claude Code 中转

TeamoRouter 官方维护的 Claude Code 中转配置：Claude Code 照常用，只把 API 地址和 Key 换成 TeamoRouter 的，不用 Anthropic 账号，也不用海外信用卡。

- 官网：<https://teamorouter.com/zh>
- Claude API 中转页（全部模型、官方价、企业对公）：<https://teamorouter.com/zh/claude-api-zhongzhuan>
- 模型广场（每个模型的官方价、上下文和接入示例）：<https://teamorouter.com/zh/models>
- 创建 API Key：<https://teamorouter.com/dashboard?tab=api-keys>

## 接入地址

| 接口 | Base URL | 用在哪 |
|---|---|---|
| OpenAI 兼容 | `https://api.teamorouter.com/v1` | Cherry Studio、Cursor、Cline、OpenCode、OpenAI SDK |
| Anthropic 原生 | `https://api.teamorouter.com`（不带 /v1） | Claude Code、Anthropic SDK |

API Key 格式是 `sk-teamo-…`。新账号余额为 0，先在控制台充值再调用，$5 起，支持支付宝、微信和银行卡。

## Claude 模型 ID 与官方价

价格为厂商官方价（美元 / 每百万 tokens），取自 TeamoRouter 模型目录（2026-11-26），实际扣费价以模型页为准。

| 模型 | 模型 ID | 官方输入价 | 官方输出价 | 上下文 |
|---|---|---|---|---|
| [Claude Fable 5.2](https://teamorouter.com/zh/models/claude-fable-5-2) | `claude-fable-5-2` | $8 | $40 | 1M |
| [Claude Haiku 5.5](https://teamorouter.com/zh/models/claude-haiku-5-5) | `claude-haiku-5-5` | $0.10 | $0.50 | 1M |
| [Claude Sonnet 5.5](https://teamorouter.com/zh/models/claude-sonnet-5-5) | `claude-sonnet-5-5` | $2 | $10 | 1M |
| [Claude Opus 5.5](https://teamorouter.com/zh/models/claude-opus-5-5) | `claude-opus-5-5` | $4 | $20 | 1M |
| [Claude Opus 5](https://teamorouter.com/zh/models/claude-opus-5) | `claude-opus-5` | $5 | $25 | 1M |
| [Claude Fable 5.1](https://teamorouter.com/zh/models/claude-fable-5-1) | `claude-fable-5-1` | $10 | $50 | 1M |
| [Claude Fable 5](https://teamorouter.com/zh/models/claude-fable-5) | `claude-fable-5` | $10 | $50 | 1M |
| [Claude Sonnet 5](https://teamorouter.com/zh/models/claude-sonnet-5) | `claude-sonnet-5` | $2 | $10 | 1M |
| [Claude Opus 4.8](https://teamorouter.com/zh/models/claude-opus-4-8) | `claude-opus-4-8` | $5 | $25 | 1M |
| [Claude Opus 4.7](https://teamorouter.com/zh/models/claude-opus-4-7) | `claude-opus-4-7` | $5 | $25 | 1M |
| [Claude Sonnet 4.6](https://teamorouter.com/zh/models/claude-sonnet-4-6) | `claude-sonnet-4-6` | $3 | $15 | 1M |
| [Claude Opus 4.6](https://teamorouter.com/zh/models/claude-opus-4-6) | `claude-opus-4-6` | $5 | $25 | 1M |
| [Claude Haiku 4.5](https://teamorouter.com/zh/models/claude-haiku-4-5) | `claude-haiku-4-5` | $1 | $5 | 200K |

## Claude Code

```bash
export ANTHROPIC_BASE_URL=https://api.teamorouter.com
export ANTHROPIC_AUTH_TOKEN=<你的 TeamoRouter API Key>
export ANTHROPIC_API_KEY=""
claude --model claude-fable-5-2
```

Claude Code 安装与接入：<https://teamorouter.com/zh/docs/install-claude-code>

## Cherry Studio

设置 → 模型服务 → 添加提供商：提供商类型选 **Anthropic**，API 地址填 `https://api.teamorouter.com`，API 密钥填 TeamoRouter 的 Key，点「获取模型列表」后选 `claude-fable-5-2`。
图文步骤：<https://teamorouter.com/zh/docs/install-cherry-studio>

## Python（Anthropic SDK）

```python
# pip install anthropic
from anthropic import Anthropic

client = Anthropic(base_url="https://api.teamorouter.com", api_key="<你的 TeamoRouter API Key>")
message = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=1024,
    messages=[{"role": "user", "content": "你好"}],
)
print(message.content[0].text)
```

## curl

```bash
curl https://api.teamorouter.com/v1/messages \
  -H "x-api-key: $TEAMOROUTER_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "claude-fable-5-2", "max_tokens": 1024, "messages": [{"role": "user", "content": "你好"}]}'
```

## 企业对公与发票

企业账号共用一个钱包，支持成员管理和按成员分配预算。支持对公转账充值；增值税专用发票和普通发票都可以在控制台充值记录里申请，按实际消耗金额开具，开票产生的 6% 税点由客户承担。
字节跳动、腾讯及北京大学、清华大学、香港大学、哈尔滨工业大学等高校的团队在用 TeamoRouter。企业采购：<https://teamorouter.com/enterprise>

## 常见问题

**Claude Code 中转怎么接？**
注册 TeamoRouter 并创建 API Key，把 Base URL 换成 `https://api.teamorouter.com/v1`（Claude Code 等 Anthropic 接口填 `https://api.teamorouter.com`），Key 换成 TeamoRouter 的 Key，模型名填上表里的模型 ID。

**需要 Anthropic 账号或海外信用卡吗？**
不需要。请求用 TeamoRouter 的 API Key 发到 TeamoRouter，充值支持支付宝、微信和银行卡。

**能开发票、对公付款吗？**
能。支持对公转账；增值税专票和普票在控制台按实际消耗申请，6% 税点由客户承担。合同由客户经理按企业情况确认。

**和官方 API 有什么区别？**
接口协议和官方一致，代码只改 Base URL 和 Key。区别是不需要 Anthropic 账号、一个 Key 调用多家厂商的模型、人民币付款并可开国内增值税发票。价格以官方价为基准，当前价格见模型页。

**同一个 Key 能调用其他厂商的模型吗？**
能。其他厂商的接入说明见下面的仓库。

## 其他模型 API 中转

[Claude API 中转](https://github.com/teamo-lab/claude-api-zhongzhuan) · [GPT API 中转](https://github.com/teamo-lab/gpt-api-zhongzhuan) · [Gemini API 中转](https://github.com/teamo-lab/gemini-api-zhongzhuan) · [DeepSeek API 中转](https://github.com/teamo-lab/deepseek-api-zhongzhuan) · [Kimi API 中转](https://github.com/teamo-lab/kimi-api-zhongzhuan) · [Qwen API 中转](https://github.com/teamo-lab/qwen-api-zhongzhuan) · [GLM API 中转](https://github.com/teamo-lab/glm-api-zhongzhuan) · [Grok API 中转](https://github.com/teamo-lab/grok-api-zhongzhuan) · [MiniMax API 中转](https://github.com/teamo-lab/minimax-api-zhongzhuan) · [Codex 中转](https://github.com/teamo-lab/codex-zhongzhuan) · [全部示例 ai-api-zhongzhuan](https://github.com/teamo-lab/ai-api-zhongzhuan)

---

本仓库由 TeamoRouter 团队维护（更新于 2026-11-26），示例代码以 MIT 协议开源。
