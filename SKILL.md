---
name: ai-pm-resume-pipeline
description: >
  AI/B端/大模型应用产品经理简历流水线：从零引导生成完整中文简历，或将已有项目经历
  重构为「痛点抽象—机制设计—价值闭环」表达，并同步产出面试追问题；简历撰写后自动
  执行 market_scanner 岗位热词扫描并 enrichment 措辞。当用户想写简历、做简历、生成简历、
  优化/润色/重构简历、改项目经历、包装项目、STAR、自我评价、对照 JD 改简历、冲 AI
  产品经理/B端产品经理岗位时使用。支持三模式——Mode A 从零构建、Mode B 升级已有项目段、
  Full Pipeline 先 A 后 B。不要用于非简历写作。
---

# AI PM 简历流水线（Build → Upgrade）

合并两套能力：

1. **Mode A（从零构建）**：引导式采集 → 方法论改写 → 完整 Markdown 简历骨架  
2. **Mode B（项目升级）**：把项目从执行流水账重构为可抗面试追问的机制叙事  
3. **Full Pipeline**：先 A 出可投递骨架，再对每个项目跑 B，得到冲刺版

本文件只编排流程；细则在 `references/`，按需 Read。

---

## 开场：先选模式（不要直接开写）

触发后用一句话确认模式，并说明差异：

| 模式 | 何时选 | 产出 |
|---|---|---|
| **A 从零构建** | 没有完整简历 / 转行刚起步 | 五板块完整 MD 简历（项目段 STAR） |
| **B 项目升级** | 已有简历或项目描述，要冲 AI/B 端 PM | 机制级改写后仍按 **STAR 格式展示** + 面试追问题库 |
| **Full Pipeline** | 从零做到冲刺版（推荐） | A 骨架 → B 升级内容 → **STAR 终稿** + 题库 |

用户说「写简历/生成简历」且未贴旧稿 → 默认 **Full Pipeline**（可改成只 A）。  
用户贴了项目段落或说「改这段/润色/重构」→ 默认 **Mode B**。  
用户明确「只要骨架、先不打磨」→ **Mode A only**。

可选：有 JD 就贴进来（三模式通用）。贴了则 Read `references/04-jd-tailoring.md` 建岗位画像；可用 `scripts/jd_analyzer.py` 作关键词补充（词表匹配，不替代画像）。

**撰写后强制步骤**：任意模式产出简历 Markdown 后，必须执行「岗位热词扫描 enrichment」（见下方专节 / `references/07-market-enrich.md`），再用热词丰富措辞；然后才问转 Word/PDF。

---

## 全局红线（三模式共用）

1. 每句话对应一项可被面试官追问的能力；拒绝万能套话。  
2. **禁止编造数据**；没有真实数字就写定性结果或标 `[待补数据]`，**不允许「合理估算」百分比**。  
3. 不夸大角色：协助≠主导；demo≠生产上线。  
4. JD 缺口如实写入「岗位匹配说明」，不硬凑经历。  
5. 默认面向**应用层** AI PM；模型层/基建层按 `01-methodology.md` + 岗位画像调整侧重。  
6. **热词 enrichment 不得虚构经历**；未覆盖的热词只进缺口说明。

---

## 岗位热词扫描 enrichment（撰写后强制）

简历 Markdown 生成后（A2 / B 第 8 步 / Pipeline 整合后），**自动**执行，细则 Read `references/07-market-enrich.md`。

```bash
python scripts/market_scanner.py --role "{应聘岗位或默认AI产品经理}" --output market_scan_report.md
```

1. 运行上述命令（skill 根目录）；失败则按 `07-market-enrich.md` 降级，**不可跳过 enrichment**。  
2. 根据报告 Top 热词 enrichment 简历措辞（自我介绍 / 近两年工作经历 / 项目 STAR）。  
3. 输出 enrichment 后的完整 Markdown +「市场热词 enrichment 说明」。  
4. 再问是否转 Word/PDF。

用户明确说「跳过热词扫描」才可跳过。

---

## Mode A — 从零构建

严格按顺序，每步等用户确认。

### A0. 展示信息清单（不要直接开问）

说明将分组采集，★ 必填，可「跳过」。清单：

```
① 基本信息  ② 自我介绍  ③ 教育经历  ④ 工作经历  ⑤ 项目介绍(STAR骨架)
```

完整字段、字符上下限：Read `references/02-intake-fields.md` 后执行。

### A0.5. 可选 JD

见开场；跳过则按应用层默认。

### A1. 逐板块采集

一次一个板块；字段含下限–上限；过短给扩写方向、过长给精简建议；板块结束复述确认。  
工作经历/项目可重复：「还有下一段吗？」

**Full Pipeline 提示**：Mode A 项目段只求**事实完整**（背景/目标/你做的事/真实结果），不要在 A 阶段追求锋利措辞——留给 Mode B。

### A2. 生成 Markdown 骨架

Read `references/01-methodology.md` + `references/03-output-format.md`。  
改写（非原样拼接）自我介绍、工作经历、STAR 项目、教育经历。  
有岗位画像则按 `04-jd-tailoring.md` 排序/措辞对齐，并附**岗位匹配说明**。  
生成后逐条说明「为什么这么写、体现什么能力」。

### A3. 热词 enrichment → 交付与分流

**先**执行「岗位热词扫描 enrichment」，再：

- **A only**：问是否转 Word/PDF；结束。  
- **Full Pipeline**：进入「Pipeline 桥接」（B 升级可用 enrichment 后的稿为底）；不要在未升级项目前鼓励投递冲刺岗。

---

## Pipeline 桥接（A → B）

Mode A 骨架（建议已做过一轮热词 enrichment）确认后，列出全部项目，问：

> 接下来按 Mode B 升级项目内容（机制叙事打磨），**简历展示仍用 STAR（背景/目标/主要工作/成果）**。  
> 建议优先升级最想写进简历、最能代表 AI/B 端能力的 1–2 个项目。从哪个开始？

对选中的每个项目：把 A 中该项目的 STAR 全文作为 Mode B 输入，执行 Mode B；  
升级完成后用 **STAR 冲刺版**（见 `03-output-format.md`）**替换**简历「项目介绍」对应章节（保留项目名/时间；副标题可微调）。  
**禁止**把终稿改成「总述 + 能力模块 Bullet」版式——那只是 Mode B 中间打磨形态。  
全部选定项目完成后：

1. 整合后的完整简历 Markdown（项目段全部为 STAR）  
2. **再次**执行「岗位热词扫描 enrichment」（冲刺版措辞再对齐一轮）  
3. 各项目面试追问题库（可合并为一个文件）  
4. 若有 JD：更新岗位匹配说明  

再问是否转 Word/PDF。

---

## Mode B — 已有简历 / 项目升级

Read `references/05-project-rewrite.md`（必读）与 `references/06-ai-pm-glossary.md`（术语不确定时）。

输入：用户粘贴的项目经历，或 Pipeline 传入的 STAR 段。一次只升级**一个**项目。

### 输出步骤（逐步确认，禁止一次甩终稿）

1. 核心矛盾提炼（3–5 句）  
2. 痛点词提炼（原因（结果词）格式）  
3. 解法机制提炼  
4. 可突出产品能力判断  
5. 项目总述：稳健版 + 锋利版（附术语解释；**总述不单独作为简历版式**，确认后并入 STAR「背景/目标」）→ 等确认  
6. 逐条 Bullet：一次一条；含改写文本、逐词能力映射、核心信号；确认后写入面试追问（中间态，用于打磨「主要工作」）  
7. 成果段：效果提升 + 能力沉淀 + 后续扩展（**仅真实数据**）  
8. **最终整合版（强制 STAR 展示）**：按 `03-output-format.md` / `05-project-rewrite.md` 第 8 节，把已确认内容映射为：

```markdown
### {通俗项目名}（{技术栈副标题}）｜{项目时间}
- **背景**：…
- **目标**：…
- **主要工作**：…（分点；写入已确认的机制 Bullet 改写）
- **成果**：…
```

可粘贴进简历的只有这一 STAR 结构；不要输出「总述段落 + 模块名 Bullet 列表」作为终稿。

9. **岗位热词扫描 enrichment**（见专节）：自动跑 `market_scanner.py`，用热词 enrichment 本项目/整份简历后，再交付。

多项目时：做完一个再问下一个；注意各项目 STAR 措辞有差异、避免同质化。全部项目结束后若尚未对整份简历做过 enrichment，补跑一次。

若用户给了整份旧简历：先标出建议升级的项目段与不动的板块（基本信息/教育通常不动；工作经历仅在空泛时轻改，深度打磨留给项目段）。

---

## 资源索引

| 文件 | 何时读 |
|---|---|
| `references/02-intake-fields.md` | Mode A 采集 |
| `references/01-methodology.md` | Mode A 生成；解释「为什么这么写」 |
| `references/03-output-format.md` | Mode A / 终稿排版 |
| `references/04-jd-tailoring.md` | 用户提供了 JD |
| `references/05-project-rewrite.md` | Mode B / Pipeline 第二阶段 |
| `references/06-ai-pm-glossary.md` | 术语用法拿不准时 |
| `references/07-market-enrich.md` | 简历撰写后的热词扫描 enrichment（强制） |
| `scripts/jd_analyzer.py` | 可选关键词补充 |
| `scripts/market_scanner.py` | **撰写后强制**；失败则按 07 降级 |
