# 模拟对话.skill · Simulated Dialogue.skill

> **语言 / Language**：[中文](#中文) ｜ [English](#english)

A Claude Code skill that distills a real person from their chat logs into a **lean persona file**, purpose-built for the **PersonaChat** two-AI dialogue simulator.

一个 Claude Code skill：读真实聊天记录，把一个人**蒸馏成精简人设档案**，专供 **PersonaChat** 双 AI 对话程序使用。

---

## 中文

### 这是什么
PersonaChat 是一个 JavaFX 桌面程序，让两套人设各跑一个 LLM 实例**互相私聊**。它运行时已经替人设做了大量包装（身份框架、按话题检索真实语料、外部纠正、输出格式、模拟时间注入）。

通用的「前任蒸馏器」(create-ex 之类) 产出 200+ 行的关系档案，定位「前任刚分手」，塞进 PersonaChat 后**和程序打架**。本 skill 反其道：**只产出纯人设**，把程序已经包办的东西全砍掉，做成一份 ~50–70 行、每次注入都不被稀释的 lean 档案。

### 它会蒸馏什么
让 Claude 读聊天记录后**全面总结**：
- **作息时间**（几点睡醒、深夜活跃）—— 配合程序注入的模拟时间
- **聊天节奏**（一次聊多久、回复快慢、谁先开头、何时断）
- **日常活动 / 要做的事** —— 喂程序的「离开」事件（去打游戏/洗澡/上课）
- **兴趣爱好** —— 会主动聊的话题
- **双方性格、说话风格**（口头禅 / 语气 / 标点 / emoji / 连发 / 真实原句）
- **情感模式**（开心 / 不对劲 / 撒娇 / 吃醋 / 受伤 + 触发器）
- **发图 / 发表情 / 发语音的时机**
- **事件全集**（转账、出门、剪头发…）—— 单独成 `events.md`，按需检索

### 特殊产出约定
| 想表达 | 模型输出 | 落地方式 |
|---|---|---|
| 发图片 | `【一张xxx的图片】` | 占位，生图接口以后接 |
| 发语音 | `【一段xxx的语音】` | 占位，语音生成以后接 |
| 发表情包 | `【表情:label】` | **真发**：从表情库按情绪挑真实 gif/png |

### 安装
把 `Simulated Dialogue.skill/` 整个目录放进 `~/.claude/skills/`，重启 Claude Code。
（skill 触发名：`sim-dialogue`）

### 用法
对 Claude 说「帮我蒸馏一个模拟对话人设」，它会依次问你：蒸馏谁 → 时间线定位 → 新 skill id，然后读语料、按维度总结、给你预览、写入人设文件，并把它接进 PersonaChat 程序。

### 路线图
- **P1** 蒸馏 skill（本仓库）
- **P2** PersonaChat 解析图片/语音/表情占位标记并在 UI 渲染
- **P3** 表情库：聚类去重 → 打情绪标签 → 模型按心情挑 → UI 渲染真表情
- **P4** 事件系统：程序按需检索 `events.md`；接生图 / 语音生成接口

### ⚠️ 隐私
本 skill 处理私人聊天记录。**所有原始数据与蒸馏出的个人人设仅本地存储，请勿上传。** 本仓库只含通用工具本体，不含任何真人数据。仅用于个人回忆与创作，勿用于骚扰、跟踪或侵犯他人隐私。

---

## English

### What it is
PersonaChat is a JavaFX desktop app where two personas each run an LLM instance and **chat with each other**. At runtime it already wraps the persona heavily (identity framing, topic-based retrieval of real corpus snippets, external corrections, output format, simulated-time injection).

Generic "ex-partner distillers" (like create-ex) produce 200+ line relationship archives framed as "freshly broken up" — which **fights** PersonaChat. This skill does the opposite: it outputs **persona only**, stripping everything the program already handles, yielding a ~50–70 line lean file that doesn't get diluted on every injection.

### What it distills
Have Claude read the chat logs and **comprehensively summarize**:
- **Daily rhythm** (sleep/wake times, late-night activity) — pairs with the program's simulated-clock
- **Chat cadence** (how long they talk, reply speed, who starts, when it ends)
- **Daily activities / errands** — feeds the program's "leave" events (gaming/shower/class)
- **Interests** — topics they bring up
- **Both sides' personality & speech style** (catchphrases / tone / punctuation / emoji / bursts / real lines)
- **Emotional patterns** (happy / off / clingy / jealous / hurt + triggers)
- **When they send pics / stickers / voice messages**
- **Full event catalog** (transfers, going out, haircuts…) — kept in a separate `events.md`, retrieved on demand

### Special output conventions
| Intent | Model outputs | How it lands |
|---|---|---|
| Send an image | `【一张xxx的图片】` | Placeholder; image-gen hookup later |
| Send a voice msg | `【一段xxx的语音】` | Placeholder; voice-gen hookup later |
| Send a sticker | `【表情:label】` | **Real**: pick an actual gif/png from the sticker library by mood |

### Install
Drop the whole `Simulated Dialogue.skill/` directory into `~/.claude/skills/` and restart Claude Code. (Skill trigger name: `sim-dialogue`.)

### Usage
Tell Claude "help me distill a simulated-dialogue persona". It asks: who to distill → timeline stance → new skill id, then reads the corpus, summarizes by dimension, previews for you, writes the persona file, and wires it into PersonaChat.

### Roadmap
- **P1** Distillation skill (this repo)
- **P2** PersonaChat parses image/voice/sticker markers and renders them in the UI
- **P3** Sticker library: cluster & dedupe → emotion labels → model picks by mood → render real stickers
- **P4** Event system: program retrieves `events.md` on demand; image-gen / voice-gen hookups

### ⚠️ Privacy
This skill processes private chat logs. **All raw data and the distilled personal personas stay local — do not upload them.** This repo contains only the generic tool, with no real-person data. For personal reflection and creative use only — not for harassment, stalking, or privacy invasion.
