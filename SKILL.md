---
name: x-engagement
version: 4.0.0
description: "X/Twitter 运营自动化。完整 onboarding → Persona 学习 → 人类行为模拟 → 记忆系统 → 定时任务 → For You 关注 → Following 互动"
---

# X 运营自动化 Skill v4.0

## 快速开始

**触发条件：**
- "刷推 [时间]"
- "运营推特 [时间]"
- "去X上互动 [时间]"

**首次运行：**
自动进入 Onboarding 流程（详见 `docs/onboarding.md`）

**后续运行：**
读取配置 → 直接刷推

---

## 文档结构

```
x-engagement/
├── SKILL.md                    # 主入口（本文件）
├── docs/
│   ├── onboarding.md           # Onboarding 流程
│   ├── human-behavior.md       # 人类行为模拟规范
│   ├── memory-system.md        # 记忆系统设计
│   ├── cron-jobs.md            # 定时任务
│   ├── comment-generation.md   # 评论生成逻辑
│   └── natural-language-parser.md # 自然语言时间解析
├── templates/
│   ├── persona.md              # Persona 模板
│   ├── config.json             # 配置模板
│   └── daily-log.md            # 每日日志模板
└── scripts/
    ├── setup-cron.sh           # 设置定时任务
    └── check-onboarding.sh     # 检查状态
```

---

## 核心功能

### 1. Onboarding（首次运行）

**5个阶段：**
1. 浏览器连接 + 登录检查
2. 选择 Persona（自己或其他账号）
3. 学习 Persona（抓取100条 → 生成描述）
4. 刷推习惯配置
5. 保存配置

**详见：** `docs/onboarding.md`

---

### 2. 人类行为模拟

**核心原则：** 不追求完美，追求"足够像真人"

**包含：**
- 随机时间生成器（正态分布）
- 人类滚动模式（小/中/大滚动）
- 鼠标轨迹模拟
- 频率限制
- 评论间隔（3-6分钟）

**详见：** `docs/human-behavior.md`

---

### 3. 记忆系统

**三层记忆：**

```
memory/daily/hotspots/
├── .onboarding_complete     # Onboarding 标记
├── .config.json             # 用户配置
├── personas/
│   └── [handle].md          # Persona 描述
├── events/                  # 重大事件（永久）
├── tables/                  # 每日热点（7天）
└── history/
    ├── comments/            # 评论历史（避免自相矛盾）
    └── daily/               # 每日日志
```

**关键功能：**
- 记录每次评论内容
- 记录用户说过的话（如"昨天出去吃饭了"）
- 评论前检查历史，避免矛盾

**详见：** `docs/memory-system.md`

---

### 4. 定时任务

**每日热点总结：**
- 时间：每天早上10点
- 内容：抓取 Top 10 → 更新热点表格 → 推送给用户

**刷推提醒（用户自定义）：**
- 支持自然语言设置
- 固定时间："早上9点、下午3点、晚上9点"
- 随机时间："每天3次，随机时间"
- 工作日/周末："工作日晚上8点，周末随机3次"

**设置方法：**
```bash
./scripts/setup-cron.sh
```

**详见：** `docs/cron-jobs.md`

---

### 5. 刷推流程

**For You 页面：**
1. 浏览（真人滚动模式）
2. 关注（根据配置条件）

**Following 页面：**
1. 点赞（有价值的推文）
2. 评论（2小时内，使用 persona 风格）
3. 记录评论到历史

**详见：** `docs/comment-generation.md`

---

## 使用示例

### 首次使用

```
用户: 刷推
Bot: 开始 Onboarding...
     1. 检查浏览器...
     2. 请选择 persona...
     3. 学习中...
     4. 配置刷推习惯...
     5. 完成！开始刷推...
```

### 后续使用

```
用户: 刷推半小时
Bot: 读取配置...
     For You: 浏览 18-30 屏，关注 6 人
     Following: 点赞 18 条，评论 6 条
     开始...
```

---

## 关键特性

| 特性 | 说明 |
|------|------|
| 完整 Onboarding | 5阶段引导，学习 persona |
| 人类行为模拟 | 随机延迟、滚动模式、频率限制 |
| 记忆系统 | 评论历史、用户信息、热点表格 |
| 定时任务 | 每日热点总结 |
| 避免矛盾 | 评论前检查历史记录 |
| 结构化设计 | 多文件组织，易于维护 |

---

## 必读文档

按顺序阅读：

1. `docs/onboarding.md` - 了解首次运行流程
2. `docs/human-behavior.md` - 了解人类行为模拟
3. `docs/memory-system.md` - 了解记忆系统
4. `docs/comment-generation.md` - 了解评论生成

---

*版本: 4.0.0*
*更新: 2026-03-02*
*改进: 结构化设计 + 记忆系统 + 定时任务 + 人类行为规范*
