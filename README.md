# 🧠 Smart Router — 让 AI Agent 自动省钱

> **简单任务走免费模型，复杂任务才花钱。** 26+ 免费模型随时待命。

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenRouter](https://img.shields.io/badge/OpenRouter-Free%20Models-orange.svg)](https://openrouter.ai)

## 🔥 解决什么问题

你有一个 AI Agent，每天调用 API 花不少钱。但仔细看——翻译、格式化、摘要、提取这些简单任务，真的需要 GPT-4 / Claude 吗？

**不需要。** OpenRouter 有 26+ 免费模型可以干这些活。

## ✨ 怎么用

```bash
# 1. 注册 OpenRouter（免费）拿 API Key
export OPENROUTER_API_KEY="sk-or-..."

# 2. 把 SKILL.md 放进你的 Agent 技能目录
git clone https://github.com/webkubor/smart-router.git
cp smart-router/SKILL.md ~/path/to/your/agent/skills/

# 3. 搞定。Agent 读完文档就会自动判断什么时候用免费模型。
```

## 📊 省多少

| 场景 | 全用付费 | Smart Router | 省 |
|------|---------|-------------|-----|
| 100 次翻译 | ~$2 | $0 | 100% |
| 月混合任务 | ~$30 | ~$8 | 73% |

## 🧩 兼容

任何能读 SKILL.md 的 Agent：OpenClaw、Claude Code、Cursor、Codex、OpenCode 等。

---

Built by [webkubor](https://github.com/webkubor)
