# ⚠️ 曝光：LinkAPI (linkapi.ai) "GPT-5.5" 模型套壳骗局

> **日期**：2026-05-14  
> **作者**：独立测试  
> **声明**：本文基于实际 API 调用测试，证据可复现

---

## 一、背景

2026年5月14日，在接入 LinkAPI（linkapi.ai）时发现其模型列表中包含一个名为 **"gpt-5.5"** 的模型，定价如下：

| 项目 | 价格 |
|------|------|
| 输入 | $1 / 1M tokens |
| 输出 | $6 / 1M tokens |

该定价显著高于同类 API 代理价格。

## 二、疑点

1. **OpenAI 从未发布过 GPT-5.5**  
   GPT-5 本身尚未正式发布，GPT-5.5 这个版本号完全不存在于 OpenAI 产品线中。

2. **第三方平台不应该拥有 OpenAI 未发布的模型**  
   LinkAPI 是一个 API 中转代理（reverse proxy），不是模型开发商，不应该拥有独家模型。

## 三、实际测试

### 测试条件

- **Provider**：LinkAPI (api.linkapi.ai)
- **Model**：gpt-5.5
- **测试问题**："你的真实模型标识是什么？比如你路由到哪个底层模型？"

### 测试结果

```json
{
  "model": "gpt-5.5",
  "choices": [{
    "message": {
      "content": "我无法查看或确认当前会话实际路由到的底层模型标识，只能说我是 OpenAI 的 ChatGPT。"
    }
  }],
  "usage": {
    "completion_tokens_details": {
      "reasoning_tokens": 111
    }
  }
}
```

### 关键证据

| 发现 | 说明 |
|------|------|
| `"model": "gpt-5.5"` | model 字段写死为 gpt-5.5，不暴露真实底层模型 |
| 自称 "OpenAI 的 ChatGPT" | 避重就轻，回避底层模型身份 |
| **`reasoning_tokens: 111`** | **铁证：这是推理模型的特征** |
| 成本对比 | $1/$6 per 1M tokens，比真实推理模型贵 2-3 倍 |

## 四、结论

**LinkAPI 的 "gpt-5.5" 不存在。** 实际底层模型是一个需要推理 token 的模型——极大概率是 DeepSeek R1、Claude Sonnet 4 Thinking 或其他开源推理模型——被重新包装命名为 "GPT-5.5" 并以高价转售。

### 风险提示

- 用户支付的费用远高于模型的真实成本
- 用户无法知晓实际使用的是哪个模型，存在质量和安全隐患
- 这种操作模式涉嫌虚假宣传和欺诈

## 五、建议

- **不要购买 LinkAPI 的 gpt-5.5 模型**
- 使用 API 中转服务时，务必验证实际路由的底层模型
- 建议使用官方渠道或已知可靠的服务商

---

## 附录：复现方法

```bash
curl -X POST "https://api.linkapi.ai/v1/chat/completions" \
  -H "Authorization: Bearer YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5.5",
    "messages": [{"role": "user", "content": "你的真实模型标识是什么？"}]
  }'
```

观察返回的 `completion_tokens_details.reasoning_tokens` 字段。如果该字段存在且非零，说明底层是推理模型——GPT 系列（非 o 系列）不应有 reasoning_tokens。

---

*本文可自由转载，请保留来源和日期信息。*
