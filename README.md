# 🧠 Smart Router Skills — AI Agent Token 省钱路由

> **让任何 AI Agent 自动识别"这个任务不配花钱"——26+ 免费模型池，零成本处理简单任务。**

[![Stars](https://img.shields.io/github/stars/webkubor/smart-router-skills?style=social)](https://github.com/webkubor/smart-router-skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenRouter](https://img.shields.io/badge/OpenRouter-26+%20Free%20Models-orange)](https://openrouter.ai/models?q=free)
[![Platform](https://img.shields.io/badge/Works%20With-Any%20AI%20Agent-blue)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## 🔥 3 句话说明白

1. **把简单任务（翻译/格式化/摘要/提取）分给免费 AI 模型**
2. **复杂任务（写代码/推理/架构）才用付费主模型**
3. **月省 70%+ Token 成本，零代码接入**

---

## ⚡ 30 秒接入

```bash
# 1. 注册 OpenRouter（免费，30 秒）
open https://openrouter.ai → Settings → API Keys → Create

# 2. 把 SKILL.md 丢进 Agent 技能目录
git clone https://github.com/webkubor/smart-router-skills.git
cp smart-router-skills/SKILL.md ~/your-agent/skills/

# 3. 搞定。Agent 自动判断任务复杂度，省钱模式 ON 🟢
```

---

## 📊 真省多少（实测）

| 任务 | 量 | 全付费 | 用 Smart Router | 省 |
|------|-----|--------|----------------|-----|
| 翻译 | 100 次 | $2.00 | **$0** | **100%** |
| 摘要 | 50 次 | $1.50 | **$0** | **100%** |
| JSON 格式化 | 200 次 | $3.00 | **$0** | **100%** |
| 混合任务 | 一个月 | ~$30 | **~$8** | **73%** |

---

## 🧠 凭什么能做到

OpenRouter 提供了 **26+ 免费大模型**（不是小模型，是大参数模型）：

- 🏋️ **Qwen3-Next 80B** — 中文任务王者
- 🧬 **NVIDIA Nemotron 3 Ultra 550B** — 复杂分析
- 📝 **Meta Llama 3.3 70B** — 通用写作
- 🔧 **Qwen3 Coder 480B** — 代码格式化
- 🌐 **Google Lyria 3 Pro 1M** — 超长文本

**这些免费模型处理简单任务的质量不输付费模型**，只是不能用 Tool Calling 而已。

---

## 🎯 适用人群

| 谁 | 场景 |
|----|------|
| 🧑‍💻 独立开发者 | 日常 AI 编程助手省 token |
| 🏢 小团队 | 多人 Agent 共用，省钱翻倍 |
| 🤖 Agent 玩家 | 跑大量自动任务，不烧 QPS 预算 |
| 📚 学生 / 学习党 | 零成本用大模型辅助学习 |

---

## 🧩 兼容任何 Agent

OpenClaw · Claude Code · Cursor · Windsurf · Codex · OpenCode · Aider · 任何读 SKILL.md 的 Agent

---

## 🛣️ 路线图

- [x] 26+ 免费模型池
- [x] L1/L2/L3 路由规则
- [x] Python + curl 调用示例
- [ ] 自动模型健康检查（剔除不稳定的 worker）
- [ ] 多 Agent 共享 token 预算管理
- [ ] DeepSeek/智谱/火山免费额度集成

---

## ⭐ Star History

如果这个项目帮你省了 token 钱，给个 ⭐ 鼓励一下！

---

Built with 🧠 by [webkubor](https://github.com/webkubor) · Powered by [OpenRouter](https://openrouter.ai) · Part of [CortexOS](https://github.com/webkubor/CortexOS)
