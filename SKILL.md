---
name: smart-router
description: "Smart Router — CS 智能路由省钱模式。所有 Agent 必读。简单任务走免费模型，核心任务才用主模型。免费模型按聪明度排序优先使用。"
---

# Smart Router — Agent 任务梯度路由

> **铁律：免费模型当小弟随便用，自己的 token 留给核心任务。**

## 三级任务梯度

### 🟢 L1 — 免费模型干（随便用，不花自己的 token）

```
翻译、格式化、摘要、信息提取、分类、打标签、代码注释
列表/表格生成、正则生成、简单文案、消息草稿、JSON 转换
```

**这类任务一律走 Smart Router 免费模型，禁止消耗主模型 token。**

### 🟡 L2 — 免费模型优先，失败回退主模型

```
API 响应解析、简单 SQL、中等文案、数据清洗
```

先让免费模型试，搞砸了再用主模型。

### 🔴 L3 — 主模型认真干（webkubor 交给你的核心任务）

```
代码生成/重构、多步推理、架构设计、Tool Calling、对话回复
```

**webkubor 交办的核心任务必须用自己的主模型认真完成。**

## 免费 Worker 池（按聪明度排序，优先用排前面的）

> 数据源：OpenRouter 实时免费模型列表，按参数量 × 上下文 × 实际能力排序
> 更新时间：2026-06-23

### Tier S — 最强免费（优先用）

| 优先级 | Worker | 型号 | 参数 | 上下文 | 擅长 |
|--------|--------|------|------|--------|------|
| 1 | `nemotron-ultra-550b` | NVIDIA Nemotron 3 Ultra | 550B-A55B | 1M | 复杂分类、深度分析、推理 |
| 2 | `qwen3-coder` | Qwen3 Coder 480B | 480B-A35B | 1M | 代码格式化、语法修复、长文本 |
| 3 | `hermes-3-405b` | Nous Hermes 3 | 405B | 131K | 指令遵循、创意写作 |

### Tier A — 强力免费

| 优先级 | Worker | 型号 | 参数 | 上下文 | 擅长 |
|--------|--------|------|------|--------|------|
| 4 | `nemotron-super-120b` | NVIDIA Nemotron 3 Super | 120B-A12B | 1M | 分类、提取、分析 |
| 5 | `gpt-oss-120b` | OpenAI GPT-OSS | 120B | 131K | 通用英文任务 |
| 6 | `qwen3-next-80b` | Qwen3-Next | 80B-A3B | 262K | 中文任务、格式化、摘要 |

### Tier B — 通用免费

| 优先级 | Worker | 型号 | 参数 | 上下文 | 擅长 |
|--------|--------|------|------|--------|------|
| 7 | `llama-3.3-70b` | Meta Llama 3.3 | 70B | 131K | 通用任务、文案润色 |
| 8 | `gemma-4-31b` | Google Gemma 4 | 31B | 262K | 信息提取、结构化输出 |
| 9 | `nemotron-nano-30b` | NVIDIA Nemotron 3 Nano | 30B-A3B | 256K | 轻量推理 |

### Tier C — 快速免费（极简任务）

| 优先级 | Worker | 型号 | 参数 | 上下文 | 擅长 |
|--------|--------|------|------|--------|------|
| 10 | `gpt-oss-20b` | OpenAI GPT-OSS | 20B | 131K | 极简任务、快速响应 |
| 11 | `llama-3.2-3b` | Meta Llama 3.2 | 3B | 131K | JSON 格式化、极简 |
| 12 | `openrouter/free` | OpenRouter 自动选择 | — | 200K | 懒得选就让它自动选 |

## 调用方法

```bash
# curl 一行搞定（示例：用最强的 nemotron-ultra 做翻译）
curl -s https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-ultra-550b-a55b:free",
    "messages": [{"role":"user","content":"翻译成英文：这是一段中文文本"}]
  }' | jq -r '.choices[0].message.content'
```

```python
import urllib.request, json, os

def route_free(task, content, worker="nvidia/nemotron-3-ultra-550b-a55b:free"):
    """把任务路由到免费模型。默认用最强的。"""
    url = "https://openrouter.ai/api/v1/chat/completions"
    headers = {
        "Authorization": f"Bearer {os.environ['OPENROUTER_API_KEY']}",
        "Content-Type": "application/json"
    }
    body = {
        "model": worker,
        "messages": [{"role": "user", "content": f"{task}\n\n{content}"}],
        "max_tokens": 4000
    }
    req = urllib.request.Request(url, data=json.dumps(body).encode(), headers=headers)
    resp = urllib.request.urlopen(req, timeout=60)
    return json.loads(resp.read())["choices"][0]["message"]["content"]
```

## 核心约束

1. **免费模型不能用 tool calling** — 只做纯文本输入输出
2. **失败自动回退** — 免费模型超时/报错时，自动切主模型
3. **安全敏感任务不走免费** — 密钥、密码、用户隐私必须走主模型
4. **关键输出需验证** — JSON、代码等格式输出需要校验有效性
5. **有速率限制** — 免费模型通常 20 RPM，高频任务走主模型
6. **优先用更强的** — 排前面的模型更聪明，优先选 Tier S/A

## 选型参考：按任务类型选 Worker

| 任务 | 推荐 Worker | 理由 |
|------|------------|------|
| 中文翻译/摘要 | `qwen3-next-80b` | 中文最强 |
| 代码格式化 | `qwen3-coder` | 代码专用 480B |
| 深度分析/分类 | `nemotron-ultra-550b` | 参数最大 |
| 长文本处理(>131K) | `nemotron-super-120b` | 1M 上下文 |
| 通用英文 | `gpt-oss-120b` | OpenAI 出品 |
| 极简 JSON | `llama-3.2-3b` | 快速省事 |
| 不知道选啥 | `openrouter/free` | 自动选择 |
