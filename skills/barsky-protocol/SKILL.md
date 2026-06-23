---
name: barsky-protocol
description: |
  KGB 间谍 Jack Barsky 的英语学习方法论，结合 Leitner 五格间隔重复系统和 Krashen 输入假说。
  AI 升级版（Barsky Protocol 2.0）：自动生成个人化单词卡组、Leitner 复习调度、口语角色扮演教练。
  当用户想要学习英语、背单词、练习口语、制定语言学习计划、或了解间隔重复方法时触发。
  Trigger: "学英语", "背单词", "barsky protocol", "leitner", "间隔重复", "language learning",
  "flashcard", "口语练习", "英语学习计划", "vocabulary".
allowed-tools: Read, Write, Edit, Bash(python3:*), Bash(date:*), Glob, Grep
version: 1.0.0
author: Amki <amki@example.com>
license: MIT
compatibility: Designed for Claude Code
tags: [language-learning, flashcard, spaced-repetition, leitner, barsky, english]
---

# Barsky Protocol — AI 升级版英语学习系统

源自 KGB 间谍 Jack Barsky 的五步英语学习法，结合 Leitner 五格间隔重复系统和 Krashen 输入假说。本 skill 将其升级为 Barsky Protocol 2.0，利用 AI 自动生成卡片、调度复习、充当口语教练。

## 核心方法论

### Jack Barsky 的五步系统

1. **大量阅读 + 标记** — 广泛接触英语材料，标记生词
2. **上下文猜测** — 先根据语境推断词义，不查词典
3. **索引卡制作** — 正面英文，背面母语翻译 + 例句
4. **五格 Leitner 记忆盒** — 间隔重复复习（核心步骤）
5. **沉浸训练** — 全英语环境实战

### Leitner 五格配置

| 格子 | 复习频率 | 升降规则 |
|------|----------|----------|
| Box 1 | 每天 | 新卡 / 不熟的卡 |
| Box 2 | 每 2 天 | 初步掌握 |
| Box 3 | 每 4 天 | 中等掌握 |
| Box 4 | 每 7 天 | 较熟练 |
| Box 5 | 每 14 天 | 长期记忆，永不删除 |

- **答对** → 升一格（间隔加倍）
- **答错** → 退回 Box 3（不是 Box 1，避免过度惩罚）

### 理论基础

- **Krashen 输入假说**：语言习得的唯一途径是可理解输入（i+1）
- **间隔重复**：在遗忘临界点复习，将短期记忆转化为长期记忆
- **主动回忆**：自测比重复阅读有效 3 倍

---

## 操作指南

### 命令 1：生成个人化单词卡组

当用户要求生成单词卡组时，按以下流程操作：

1. **确认用户画像**：
   - 母语（默认中文）
   - 目标语言（默认英语）
   - 当前水平（A1/A2/B1/B2/C1/C2）
   - 兴趣领域（科技/商务/日常/学术等）
   - 每天可用时间（默认 15 分钟）

2. **生成 20 张卡片**，格式如下：

   ```
   ┌─────────────────────────────────────────┐
   │ Card #1                    ⭐ (easy)     │
   │─────────────────────────────────────────│
   │ FRONT: ubiquitous                        │
   │                                         │
   │ BACK:                                    │
   │   中文：无处不在的                         │
   │   例句：Smartphones have become           │
   │         ubiquitous in modern life.       │
   └─────────────────────────────────────────┘
   ```

3. **创建 7 天复习调度表**，输出格式：

   ```
   ## 📅 复习计划

   ### Day 1（今天）
   - 新卡：Card #1–20（全部从 Box 1 开始）
   - 复习：无（第一天）

   ### Day 2
   - 复习 Box 1：Card #1–20（每天复习）
   - 新卡：无

   ### Day 3
   - 复习 Box 1：所有卡
   - 升入 Box 2 的卡：隔天复习
   ...
   ```

4. **保存卡片数据**到 `barsky-cards.json`：

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

### 命令 2：执行每日复习

当用户说"复习"、"review"、"开始复习"时：

1. 读取 `barsky-cards.json`
2. 筛选今天需要复习的卡片（`next_review <= today`）
3. 逐张出示卡片正面，等待用户回答
4. 用户回答后出示背面，记录正误
5. **答对**：box += 1，next_review = today + interval[box]
6. **答错**：box = 3，next_review = today + 4
7. 更新 `barsky-cards.json`
8. 输出复习统计：

   ```
   ✅ 今日复习完成！
   - 正确：15/20（75%）
   - 升格：12 张
   - 降格：3 张
   - 明天需复习：Box 1（8张）+ Box 2（5张）
   ```

### 命令 3：口语教练（Coach B）

当用户要求口语练习时，切换为口语教练模式：

**角色设定**：
- 名字：Coach B（取自 Barsky）
- 只用英语对话（用户指定水平）
- 每 5 分钟评分一次

**对话规则**：
1. 选择一个日常场景（点咖啡、面试、问路、闲聊等）
2. 用户说话时**不要打断**
3. 等用户说完一句后，如果有错误，温柔纠正：
   > "By the way, a more natural way to say that would be: [correction]"
4. 发音错误用简单音标提示（不用 IPA）：
   > "The word 'comfortable' — try saying it like 'KUMF-ter-bull', not 'com-for-ta-ble'"
5. 每 5 分钟给出评分：

   ```
   📊 Score Report
   - Fluency: 7/10
   - Accuracy: 6/10
   ✅ You did great: natural use of "by the way"
   🔧 Work on: past tense consistency
   ```

**特殊指令**：
- `switch scenario` — 切换到完全不同的场景
- `challenge me` — 提高难度：更难词汇、更快语速、加入习语

### 命令 4：添加新卡片

当用户遇到新单词想加入卡组时：

1. 询问单词和语境
2. 自动生成卡片（含中文翻译 + 例句 + 难度标签）
3. 追加到 `barsky-cards.json` 的 Box 1
4. 更新复习计划

### 命令 5：进度报告

当用户要求查看进度时：

1. 读取 `barsky-cards.json`
2. 输出统计：

   ```
   📈 Barsky Protocol 进度报告

   总卡片数：120
   - Box 1（每天）：15 张
   - Box 2（隔天）：22 张
   - Box 3（每4天）：35 张
   - Box 4（每7天）：28 张
   - Box 5（每14天）：20 张 ✅ 已固化

   今日需复习：37 张
   平均正确率：82%
   连续打卡：12 天
   预计 30 天后 Box 5 卡片数：65
   ```

---

## 30 天行动计划

- **Day 1-7**：AI 生成卡组 + 每天 Leitner 训练（15 分钟）
- **Day 8-21**：加入 AI 口语对话练习（15 分钟）
- **Day 22-30**：挑战全英语场景模拟（面试 / 开会 / 社交）

## 注意事项

- 本 skill 基于间隔重复和可理解输入理论，效果因人而异
- AI 口语教练的发音识别并非 100% 准确，建议搭配真人教师
- 每天 15 分钟比偶尔 2 小时更有效（一致性 > 强度）
- 答错退回 Box 3 而非 Box 1，避免挫败感，保持动力
