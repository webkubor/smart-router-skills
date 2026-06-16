---
name: smart-router-skills
version: 1.0.0
description: "智能路由 — 把简单任务分发给免费 AI 模型执行，为主模型省钱。任何 Agent 都能用。触发条件: 需要做简单文本任务（翻译、格式化、摘要、提取、分类）时，用免费模型代替付费模型。触发词: smart-router、智能路由、省钱模式、省 token、free model、route free。"
license: MIT
author: webkubor
category: resource
platforms: [linux, macos, windows]
metadata:
  openclaw:
    tags: [routing, cost-saving, openrouter, free-models, optimization]
    requires:
      env: [OPENROUTER_API_KEY]
---

# Smart Router — AI Agent 省钱路由

> **让简单任务走免费模型，复杂任务才花钱。**
> 26+ 个免费模型随时待命，任何 Agent 都能接入。

## 这是什么

一个路由策略文档。Agent 读完它之后，就知道什么时候该用免费模型、什么时候必须用付费模型。**不需要代码**——Agent 自己根据文档判断和调用。

## 核心理念

```
不是所有任务都需要 GPT-4 / Claude / GLM-5.2
翻译一段话？格式化 JSON？提取关键信息？→ 免费模型够了
写代码？多步推理？架构设计？→ 才用付费模型
```

## 免费 Worker 池（OpenRouter）

> 只需一个 OpenRouter API Key（免费注册），就能调用以下所有模型

### 🏋️ 大参数（80B+）

| Worker | 型号 | 上下文 | 擅长 |
|--------|------|--------|------|
| `qwen3-next-80b` | Qwen3-Next 80B-A3B | 262K | 中文任务、格式化、摘要 |
| `nemotron-ultra-550b` | NVIDIA Nemotron 3 Ultra | 1M | 复杂分类、深度分析 |
| `nemotron-super-120b` | NVIDIA Nemotron 3 Super | 1M | 分类、提取、分析 |
| `gpt-oss-120b` | OpenAI GPT-OSS 120B | 131K | 通用英文任务 |
| `llama-3.3-70b` | Meta Llama 3.3 70B | 131K | 通用任务、文案润色 |
| `hermes-3-405b` | Nous Hermes 3 405B | 131K | 指令遵循、创意写作 |

### 🔧 代码 & 长文本

| Worker | 型号 | 上下文 | 擅长 |
|--------|------|--------|------|
| `qwen3-coder` | Qwen3 Coder 480B | 1M | 代码格式化、语法修复 |
| `lyria-3-pro` | Google Lyria 3 Pro | 1M | 超长文本处理、翻译 |
| `owl-alpha` | Owl Alpha | 1M | 超长上下文任务 |

### ⚡ 轻量快速

| Worker | 型号 | 上下文 | 擅长 |
|--------|------|--------|------|
| `gemma-4-31b` | Google Gemma 4 31B | 262K | 信息提取、结构化输出 |
| `nex-n2-pro` | Nex AGI N2-Pro | 262K | 轻量推理 |
| `gpt-oss-20b` | OpenAI GPT-OSS 20B | 131K | 极简任务、快速响应 |
| `llama-3.2-3b` | Meta Llama 3.2 3B | 131K | 极简任务、JSON 格式化 |

### 🎲 自动路由

| Worker | 说明 |
|--------|------|
| `openrouter/free` | OpenRouter 官方自动选择最佳免费模型 |

## 路由规则

```
任务复杂度判断
    │
    ├── 🟢 L1 简单 → 免费模型
    │   ├─ 翻译（中↔英、任意语言）
    │   ├─ 格式化（HTML↔Markdown、JSON 转换）
    │   ├─ 文本摘要（非推理类）
    │   ├─ 信息提取（从文本中提取字段）
    │   ├─ 分类 / 打标签
    │   ├─ 代码注释生成
    │   ├─ 列表 / 表格生成
    │   └─ 正则 / 模板生成
    │
    ├── 🟡 L2 中等 → 免费模型优先，失败回退付费
    │   ├─ 简短文案撰写
    │   ├─ API 响应解析
    │   ├─ 简单 SQL 生成
    │   └─ 邮件 / 消息草稿
    │
    └── 🔴 L3 复杂 → 付费主模型
        ├─ 代码生成 / 重构
        ├─ 多步推理
        ├─ 架构设计
        ├─ Tool Calling
        └─ 对话回复
```

## 使用方式

### 方式一：Agent 自主判断

Agent 读完本文档后，遇到任务自动判断复杂度，选择免费或付费模型。

### 方式二：显式标注

```
用户: "[route: free] 把这段 HTML 转成 Markdown"
Agent: → qwen3-next-80b 执行（免费）
```

### 方式三：定时切换

```
# 非核心时段全走免费模型
用户: "晚上 23:00 到早上 8:00 自动切省钱模式"
Agent: → 该时段 L1/L2 任务全部走免费模型
```

## 调用示例

```python
import urllib.request, json, os

def route_to_free_model(task, content):
    """把简单任务路由到免费 OpenRouter 模型"""
    url = "https://openrouter.ai/api/v1/chat/completions"
    headers = {
        "Authorization": f"Bearer {os.environ['OPENROUTER_API_KEY']}",
        "Content-Type": "application/json"
    }
    body = {
        "model": "qwen/qwen3-next-80b-a3b-instruct:free",  # 免费
        "messages": [{"role": "user", "content": f"{task}\n\n{content}"}],
        "max_tokens": 2000
    }
    req = urllib.request.Request(url, data=json.dumps(body).encode(), headers=headers)
    resp = urllib.request.urlopen(req, timeout=30)
    return json.loads(resp.read())["choices"][0]["message"]["content"]

# 翻译 — 免费
result = route_to_free_model("翻译成英文", "这是一段中文文本")
```

```bash
# curl 一行搞定
curl -s https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "meta-llama/llama-3.3-70b-instruct:free",
    "messages": [{"role":"user","content":"Summarize: ..."}]
  }' | jq -r '.choices[0].message.content'
```

## 核心约束

1. **免费模型不能用 tool calling** — 只用于纯文本输入输出
2. **失败自动回退** — 免费模型超时/报错时，自动切付费主模型
3. **安全敏感任务不走免费** — 涉及密钥、密码、用户隐私的必须走主模型
4. **关键输出需验证** — JSON、代码等格式输出需要校验有效性
5. **速率限制** — 免费模型有 RPM 限制（通常 20/分钟），高频任务走付费

## 成本对比

| 场景 | 全用付费模型 | Smart Router | 省了多少 |
|------|------------|-------------|---------|
| 100 次翻译 | ~$2.00 | $0.00 | 100% |
| 50 次摘要 | ~$1.50 | $0.00 | 100% |
| 200 次格式化 | ~$3.00 | $0.00 | 100% |
| 混合任务（月） | ~$30 | ~$8 | 73% |

## 获取 OpenRouter API Key

1. 注册 [openrouter.ai](https://openrouter.ai)（免费）
2. Settings → API Keys → Create Key
3. `export OPENROUTER_API_KEY="sk-or-..."`

## 相关资源

- [OpenRouter 模型列表](https://openrouter.ai/models)
- [免费模型自动更新](https://openrouter.ai/models?q=free)

---

Built by [webkubor](https://github.com/webkubor) · Part of [CortexOS](https://github.com/webkubor/CortexOS)
