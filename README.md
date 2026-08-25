# 📖 narrative-engine

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Release](https://img.shields.io/github/v/release/lihaoran0412/dsh-narrative-engine?style=for-the-badge)](https://github.com/lihaoran0412/dsh-narrative-engine/releases)

> **插件形态：AI Preset**：DeepSeek Harness 的沉浸叙事**双向创作引擎**。把小说导入为可游玩的 v3 世界，角色扮演游玩后反向写成同人小说。核心入口是 `agent.cordis.yml` + `preset.yml`，自包含两个技能。安装时请将 `narrative-engine` 整个目录复制到 DSH 的 preset 目录，不要只复制单个文件。

**双向创作闭环**——小说 → 可游玩世界 → 游玩经历 → 同人小说，四个阶段一条龙：

```
① 导入：小说 → 世界（骨架提取/补全/切入点）
② 游玩：复用 adult-tension-narrative 引擎（18+ 校验/硬边界/同意/存档）
③ 沉淀：游玩 → 素材（硬事实层/润色层）
④ 写作：素材 → 小说（分章流水线 + 12 维深度审阅）
```

**尺度放开，边界一直都在**——18+ 也把年龄、边界、当前同意放在第一优先，`暂停` / `安全词` 随时可用。写作阶段只能写「游玩中已发生且已同意」的内容，不因创作需要凭空添加。

> ⚠️ **成年人内容（18+ Only）**：仅处理虚构成年人，所有参与角色必须明确年满 18 岁。人物边界、当前同意和安全状态始终优先于剧情推进。

## 🚀 快速安装

让 AI 直接安装本仓库：

```text
帮我安装这个 preset：https://github.com/lihaoran0412/dsh-narrative-engine
```

也可以手动安装：克隆/下载本仓库，把 `narrative-engine` 整个目录复制到你的 DSH 用户 preset 目录：

```text
<你的 DSH_HOME 或 ~/.dsh>/.agent-presets/narrative-engine
```

即最终路径为 `~/.dsh/.agent-presets/narrative-engine/`（含 `agent.cordis.yml`、`preset.yml`、`skills/`）。重启 dsh web（或新开会话），在会话模式选择器里选「叙事引擎」模式即可开始。

> 本 preset **自包含**：连带核心依赖 `adult-tension-narrative` 技能一起打包，无需单独安装。

## ✨ 核心能力

**导入：把小说变成能玩的世界**
- **多格式解析**——txt / epub / PDF / DOCX / URL / 粘贴文本，一次导入
- **骨架提取**——自动提取世界观 / 角色卡 / 时间线 / 文风，生成 `world_bible.md`、`characters/`、`timeline.md`、`outline.md`、`style.md`
- **补全程度可选**——轻 / 中 / 重三档，`保留 <项>` 可让某项保持原样，`微调 <项>=<值>` 逐项调整
- **节拍点切入**——从小说任意切入点开始游玩，`续写` 从结尾之后继续

**游玩：世界真的会往前**
- 完全复用 `adult-tension-narrative` 引擎——NPC 有记忆/立场/底线、时间推进、随机开局可预锁、全维 YAML 存档
- 每回合自动落盘 `playlog/`，可推翻回合
- `字数 <短/中/长/超长>` 控制输出篇幅

**写作：把游玩经历反向写成小说**
- 分章流水线——`大纲` → `写下一章` → `重写第 N 章` → `导出小说`
- **12 维深度审阅**——独立 reader 智能体对每章做一致性/忠实度/18+ 合规/文笔节奏/角色卡完整性等 12 维检查
- **批量审阅防死循环**——全部章节写完统一审，reader 用 one-shot 模式结算自动释放
- **风格三选一**——游玩前和写小说前由你选择：仿写原文 / 另有风格 / AI 决定

## 🧠 用什么模型玩

具体游玩体验和出文效果，会随模型版本、上下文长度和平台配置而变化。当前体验上，**推荐优先尝试 DeepSeek 或 Grok**：

- **DeepSeek**：目前更适合中文长线叙事、人物关系和状态连续性。
- **Grok**：目前更适合临场感强、风格更放开的剧情表达。

其他模型也能正常游玩；对话轮次越深，角色一致性、时间线和语感就越依赖所选模型的上下文能力。建议按你的平台与偏好实际试用。

## ▶️ 怎么开始

**想直接玩，一句话就够：**

```text
导入 <小说路径/URL>
```

按流程确认骨架 / 补全 / 切入点，然后 `开始游玩` 即可进入可游玩世界：

```text
开始游玩
```

**游玩中**，用这些命令推进和管理（复用 adult-tension-narrative 命令）：

| 命令 | 什么时候用 |
| --- | --- |
| `继续`（`c`） | 剧情停在可接续处时，向前推一个节拍 |
| `快进到明天早上` | 跳过一段时间，让离屏事件被追算 |
| `状态` | 人话看板：地点、在场、暂停与否、当前压力、许可 |
| `存档` / `保存 X` / `载入 X` | 管理命名存档，从原节点继续 |
| `本局不碰 X` | 划一条本局硬边界；`解除不碰 X` 可撤销 |
| `暂停` / `安全词` | 立刻停止亲密升级，任何时候都可以 |

**游玩结束，反向写成小说：**

| 命令 | 什么时候用 |
| --- | --- |
| `写小说` | 从游玩素材开始写作 |
| `大纲` | 生成/查看小说大纲 |
| `写下一章` | 生成下一章 |
| `重写第 N 章` | 重写某章 |
| `审阅` | 触发 reader 深度审阅（12 维，批量审阅） |
| `风格 <仿写原文/另有风格/AI决定>` | 设置写作风格（三选一） |
| `字数 <短/中/长>` | 写作字数档位 |
| `导出小说` | 输出完整小说 |

## 📁 文件放在哪里

| 位置 | 里面是什么 |
| --- | --- |
| `agent.cordis.yml` | preset 组合（persona + 工具 + 技能挂载） |
| `preset.yml` | 显示名「叙事引擎」 |
| `skills/narrative-engine/` | 本引擎技能（导入/游玩落盘/沉淀/写作/审阅） |
| `skills/adult-tension-narrative/` | 核心依赖技能（MIT 许可，游玩引擎） |

## 🏷️ 版本发布（Releases）

稳定版本在 [GitHub Releases](https://github.com/lihaoran0412/dsh-narrative-engine/releases) 发布，适合需要固定版本快照的场景：

| 版本 | 发布日期 | 说明 |
| --- | --- | --- |
| v0.2 | 2026-08-25 | 批量审阅防死循环、reader one-shot、12 维审阅、字数校验、角色卡完整性、存档编辑稳定、导入搜索高效 |
| v0.1 | 2026-08-24 | 初始版本：小说导入 → 游玩 → 反向写成同人小说 |

## 📜 许可证

- 本 preset（`narrative-engine` 技能 + 组合）：**MIT**，Copyright (c) 2026 李浩然
- `adult-tension-narrative`：MIT，Copyright (c) 2026 daha1216（见 `skills/adult-tension-narrative/LICENSE`）