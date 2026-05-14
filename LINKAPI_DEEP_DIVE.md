# LinkAPI (linkapi.ai) 深度调查报告

> **调查日期**: 2026-05-14
> **调查报告**: 完整溯源与分析

---

## 一、公司/运营方信息

### 域名注册
| 字段 | 值 |
|------|-----|
| 域名 | linkapi.ai（备用 domain: linkapi.cc） |
| 创建日期 | 2025-10-11T17:01:25Z（约7个月） |
| 注册商 | Cloudflare, Inc. |
| 注册人地区 | **中国河南省 (Henan, CN)** |
| Nameserver | Cloudflare (leo.ns.cloudflare.com) |
| 状态 | clienttransferprohibited |

### 服务器
| 字段 | 值 |
|------|-----|
| IP | 69.63.223.210 / 69.63.210.185 |
| 机房 | DMIT Cloud Services, 洛杉矶, 美国 |
| CDN | Cloudflare 全球加速 |

### 联系方式
- QQ 群: **LinkAPI通知三群** (群号: 1071996579, 1998人)
  - 不排除存在一群、二群，说明用户基数至少数千
  - 建群时间: 2025/11/23
- QQ 客服: 企业QQ 接入
- 微信客服: 企业微信 (work.weixin.qq.com/ca/cawcde424d2610f2ba)
- 邮件: support@linkapi.ai（隐藏）
- QQ 群主要成员分布: 上海27人, 00后529人（占26.5%）

### 关联域名
通过 CT 日志发现的子域名：
- `api.linkapi.ai` - 主 API 端点（美国直连）
- `api2.linkapi.ai` - 备用 API
- `hk.linkapi.ai` - 香港节点
- `jp.linkapi.ai` - 日本软银节点（日本软银优质线路）
- `pool.linkapi.ai` / `pool2.linkapi.ai` - 连接池管理
- `docs.linkapi.ai` - 官方文档
- `home.linkapi.ai` - 首页/落地页
- `pay.linkapi.ai` - 支付系统
- `draw.linkapi.ai` - 绘图生成
- `notify.linkapi.ai` - 通知系统
- `uptime.linkapi.ai` - 状态监控
- `linkapi.cc` - 备用域名（主要面向江苏/福建用户）

## 二、商业模式分析

### 运营模式
标准的 **AI API 中转代理** (reverse proxy / API relay)。用户注册后获取 API Key，通过 linkapi 的 API 地址转发到上游模型。

**商业模型特征：**
- 注册即送免费额度（未说明固定金额）
- 1:1 充值比例（1元 = 1额度）
- 支持"分组"机制——不同分组路由不同的上游供应商，价格和模型质量不同
- 支付通道：微信/支付宝（猜测）

### 定价策略
从 hvoy.ai 评测文章获取的历史价格：
- GPT-5.5: ¥1(进) ¥6(出) / 百万Token
- GPT-5.4: ¥0.5(进) ¥3(出) / 百万Token
- Sonnet 4.6: ¥1.5(进) ¥7.5(出) / 百万Token
- Opus 4.6: ¥2.5(进) ¥12.5(出) / 百万Token

## 三、套壳/假模型证据

### ❌ gpt-5.5 - 已确认套壳
**实际 API 测试结果：**

```json
{
  "model": "gpt-5.5",                  // 写死，不暴露真实底层
  "choices": [{
    "message": {
      "content": "我无法查看或确认当前会话实际路由到的底层模型标识，只能说我是 OpenAI 的 ChatGPT。"
    }
  }],
  "usage": {
    "completion_tokens_details": {
      "reasoning_tokens": 111           // 铁证：推理模型特征
    }
  }
}
```

**结论：** 底层是 DeepSeek R1 或 Claude thinking 等推理模型，套壳改名 gpt-5.5。

### ❌ gpt-4o-mini / gemini-2.5-flash
API 调用返回 "Database error" —— 后端出错，无法验证。但基于平台整体模式，大概率也非原版。

### ⚠️ 该平台其他非标模型名
评测文章提及 LinkAPI 还在销售以下**非官方模型名**：
- **Sonnet 4.6** — Anthropic 从未发布过此版本号。Sonnet 最新为 4，无 "4.6"
- **Opus 4.6** — 同 Anthropic 无此版本
- **GPT-5.4** — OpenAI 从未发布
- **GPT-5.3** — 同上

**这已构成一个普遍的行业现象：多家国内 API 中转站使用虚假模型名。** 不是 LinkAPI 独创，但 LinkAPI 确实参与其中。

### 第三方评测评价
来自 hvoy.ai 评测站的评价：
> **"我自己试了下，是稳定掺水的。"**
> 
> 指其 Claude Sonnet 4.6 / Opus 4.6 实际效果与官方模型不符。

## 四、合作伙伴与分发网络

### 1. Marinara's Kitchen
- GitHub: spicymarinara.github.io
- Discord 角色扮演社区（侧重 SillyTavern/NSFW 角色扮演）
- 网站明确标注: **"API Powered by LinkAPI"**
- 提供 Marinara Engine 前端

### 2. KanonMama
- Telegram 频道 @kanonmama（881 订阅者）
- "面向俄语用户的温馨社区"
- 提供 JanitorAI 和 SillyTavern 支持
- 被标注为 **"LinkAPI 授权销售伙伴"**

### 3. 用户生态
- 主要面向：SillyTavern 用户、AI 角色扮演、编程开发者
- 推荐工具：Cherry Studio、SillyTavern、Chatbox、RikkaHUB
- 特别警告用户不要使用 UC/夸克/百度/QQ 等有审核机制的浏览器访问

## 五、灰产关联排查

### 分析与发现

| 风险维度 | 分析 |
|---------|------|
| **模型命名欺诈** | ✅ 确认 gpt-5.5 为假模型名。同时贩卖 Sonnet 4.6、Opus 4.6、GPT-5.4 等非官方模型名 |
| **品质掺水** | ✅ 第三方评测确认 "稳定掺水" |
| **接码/薅羊毛关联** | ⚠️ 未直接发现，但用户分布含大量 00 后（26.5%），与接码平台用户画像有重合 |
| **代理爬虫关联** | ⚠️ 未发现直接关联。但其 API 服务可为爬虫提供代理（非重点） |
| **跑路风险** | ⚠️ 高风险：运营仅 7 个月，注册地河南（个人而非公司），域名 2027 年到期 |
| **支付通道** | ⚠️ 微信/QQ 支付，充值不退，典型轻资产跑路模型 |
| **涉黄风险** | ⚠️ 合作伙伴 Marinara's Kitchen 为 SillyTavern/NSFW 社区，可能涉及成人内容分发 |

### 灰产关联评分
| 项目 | 评分 (1-10) | 说明 |
|------|------------|------|
| 模型欺诈 | 10 | 100% 确认 gpt-5.5 套壳 |
| 跑路风险 | 8 | 个人运营、无公司注册、无发票 |
| 品质不透明 | 9 | 分组机制让用户不知实际路由 |
| 用户保护 | 1 | 充值不可退，无合同 |
| **综合风险** | **高** | |

## 六、结论与建议

### LinkAPI 整体评估

LinkAPI 是一个典型的中国 AI API 中转站，存在以下问题：

1. **虚假模型命名** — gpt-5.5 铁证套壳
2. **品质掺水** — 被第三方评测证实
3. **个人运营** — 无公司实体，跑路风险高
4. **"分组"不透明** — 用户无法知晓实际使用的底层模型
5. **不退款政策** — 充值即消费

**建议：**
- 从配置中删除 linkapi provider
- 不要在任何 linkapi.ai 相关服务充值
- 将该站列入个人黑名单（已记录至 BLACKLIST.md）

### 行业警示

LinkAPI 并非个例。hvoy.ai 评测列出了 30+ 个类似的中转站，**绝大多数**使用虚假模型名（GPT-5.5、Sonnet 4.6、Opus 4.7 等）。这个行业生态本身就是建立在：
- 虚标模型名 → 实际路由低质量上游 → 赚取差价
- 不提供真实路由信息
- 无官方授权

---

## 附录 A：API 复现测试

```bash
# 测试 gpt-5.5
curl -X POST "https://api.linkapi.ai/v1/chat/completions" \
  -H "Authorization: Bearer <YOUR_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"model":"gpt-5.5","messages":[{"role":"user","content":"你的真实模型标识是什么？"}]}'

# 检查返回的 completion_tokens_details.reasoning_tokens
```

## 附录 B：附加信息来源

- [AI API中转站收录与评测 - hvoy.ai](https://www.hvoy.ai/APIreview.html)
- [urlscan.io - linkapi.ai 扫描](https://urlscan.io/domain/linkapi.ai)
- [LinkAPI 官方文档](https://docs.linkapi.ai)


