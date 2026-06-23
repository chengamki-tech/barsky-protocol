# Barsky Protocol — AI 升级版英语学习系统

> 源自 KGB 间谍 Jack Barsky 的五步英语学习法，升级为 AI 驱动的间隔重复系统。

## 背景

冷战时期，KGB 间谍 Jack Barsky 被派往美国执行深潜任务。他用一套基于 Leitner 记忆盒的五步系统学习英语，最终练到 FBI 监视三年都听不出口音。

本 skill 将他的方法论升级为 **Barsky Protocol 2.0**：AI 自动生成单词卡组、智能调度 Leitner 复习、充当口语角色扮演教练。

## 安装

### 通过 Claude Code 安装（推荐）

```bash
claude install-skill https://github.com/amki-dev/barsky-protocol
```

### 手动安装

将 `skills/barsky-protocol/SKILL.md` 复制到你的 `.claude/skills/` 目录。

## 使用方式

在 Claude Code 中触发：

```
/barsky-protocol          # 查看帮助
```

或用自然语言：

| 你说 | AI 做什么 |
|------|-----------|
| "帮我生成 B1 商务英语单词卡" | 生成 20 张卡片 + 7 天复习计划 |
| "开始今天复习" | 执行 Leitner 复习，逐张出题 |
| "练口语，场景：点咖啡" | 切换为 Coach B，角色扮演对话 |
| "把 interview 加入卡组" | 新增一张卡片到 Box 1 |
| "看看进度" | 输出统计报告 |

## 核心方法论

### Leitner 五格系统

```
Box 1（每天）→ Box 2（隔天）→ Box 3（每4天）→ Box 4（每7天）→ Box 5（每14天）
                              ↑                                    │
                              └──── 答错退回 Box 3 ←──────────────┘
```

### 30 天行动计划

- **Day 1-7**：生成卡组 + 每天 Leitner 训练（15 分钟）
- **Day 8-21**：加入 AI 口语对话练习（15 分钟）
- **Day 22-30**：挑战全英语场景模拟

## 理论基础

- **Krashen 输入假说** — 语言习得的唯一途径是可理解输入（i+1）
- **间隔重复** — 在遗忘临界点复习，将短期记忆转化为长期记忆
- **Leitner 记忆盒** — 1972 年德国科学家提出的经典间隔重复系统

## 数据格式

卡片数据存储在 `barsky-cards.json`：

```json
{
  "created": "2026-06-23",
  "profile": { "native": "zh", "target": "en", "level": "B1" },
  "cards": [
    {
      "id": 1,
      "front": "ubiquitous",
      "back_zh": "无处不在的",
      "example": "Smartphones have become ubiquitous in modern life.",
      "difficulty": 1,
      "box": 1,
      "last_review": null,
      "next_review": "2026-06-23"
    }
  ],
  "review_log": []
}
```

## 许可证

MIT © Amki
