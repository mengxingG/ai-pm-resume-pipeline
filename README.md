# AI PM Resume Pipeline

AI产品经理 / B端产品经理 / 大模型应用产品经理简历流水线 Skill。

合并原 `ai-pm-resume`（从零构建）与 `resume-pm-rewriter`（项目升级），支持三模式：Mode A 从零生成完整简历、Mode B 升级已有项目经历、Full Pipeline 先 A 后 B 出冲刺终稿。

## 核心能力

1. **Mode A（从零构建）**：引导式填空采集五板块信息，按 AI PM 方法论改写为完整 Markdown 简历（项目段先用 STAR 骨架）。
2. **Mode B（项目升级）**：将项目经历从"执行动作堆砌"重构为"**业务痛点抽象—产品判断—机制设计—价值闭环**"，并同步输出面试追问题。
3. **Full Pipeline（推荐）**：先跑 Mode A 出骨架，再对核心项目逐个跑 Mode B，替换项目段得到可投递冲刺版。

支持可选填入岗位 JD：判定应用层/模型层/基础设施层，抽取岗位画像并定向突出匹配项；禁止编造数据。

## 工作流程

### Full Pipeline（默认推荐）

```
展示 5 大板块信息清单 → 可选填入 JD → 岗位画像
    ↓
逐板块引导采集（字符上下限校验）→ 生成 STAR 骨架 Markdown
    ↓
用户确认骨架 → 选择要升级的 1～2 个核心项目
    ↓
Mode B：核心矛盾 → 痛点词 → 机制 → 总述（稳健/锋利）
    ↓
逐条 Bullet 改写（含逐词能力映射）→ 成果段 → 替换进简历
    ↓
整合冲刺终稿 + 面试追问题库 → 确认后转 Word / PDF
```

### Mode A only

```
展示清单 → 可选 JD → 逐板块采集 → 方法论改写 → Markdown 骨架 → 确认 → 转文件
```

### Mode B only

```
输入项目经历（或整份旧简历）
    ↓
核心矛盾提炼 → 痛点词提炼 → 解法机制提炼 → 可突出能力判断
    ↓
项目总述（稳健版/锋利版）→ 用户确认
    ↓
逐条 Bullet 改写（每条含逐词能力映射）→ 用户逐条确认
    ↓
成果段 → 最终整合版
    ↓
同步输出面试追问题文件
```

## 核心机制

| 机制 | 说明 |
|------|------|
| **三模式路由** | 无旧稿默认 Full Pipeline；贴项目/说润色改写走 Mode B；明确「只要骨架」走 Mode A |
| **先清单后逐问** | Mode A 先展示 5 大板块，一次只推进一个板块，答完确认再问下一组 |
| **字符上下限校验** | 每个字段标注下限–上限；过短提示扩写、过长提示精简、套话改写为具体能力 |
| **A 保事实 / B 保锋利** | Mode A 项目只求事实完整；锋利机制表达留给 Mode B，避免骨架阶段过度包装 |
| **颗粒度三层控制** | 总述讲矛盾、Bullet 讲机制、成果段讲数据，严格分层 |
| **痛点词规范** | "原因（结果词）"格式，原因与结果语义不重复 |
| **逐词能力映射** | 每条 Bullet 的模块名逐词解释体现什么 PM 能力 |
| **面试题联动** | 每条 Bullet 确认后同步写入面试追问题文件 |
| **总述差异化** | 同一简历多个项目不同句式结构，避免同质化 |
| **可选 JD 定向** | 判定岗位类型 + 排序/措辞对齐 + 岗位匹配说明（缺口如实提示，不硬凑） |
| **禁止编造数据** | 无真实数字写定性结果或标 `[待补数据]`，不允许「合理估算」百分比 |

## 目录结构

```
ai-pm-resume-pipeline/
├── SKILL.md                      # Skill 主文件（模式路由 + 流程编排）
├── README.md                     # 说明文档
├── requirements.txt              # 可选脚本依赖
├── scripts/
│   ├── jd_analyzer.py            # JD 解析与关键词提取
│   └── market_scanner.py         # AI PM 市场趋势扫描
└── references/
    ├── 01-methodology.md         # 简历方法论（能力模型/板块规则/电话面试）
    ├── 02-intake-fields.md       # Mode A 字段定义 + 字符上下限 + 校验
    ├── 03-output-format.md       # Markdown 模板（STAR 骨架 / B 冲刺版）
    ├── 04-jd-tailoring.md        # JD 岗位类型判定 + 定向突出 + 匹配说明
    ├── 05-project-rewrite.md     # Mode B 改写规则、公式、禁止项
    └── 06-ai-pm-glossary.md      # AI 产品经理术语表与用法参考
```

## 脚本使用

脚本为可选补充；主流程（A / B / Pipeline）不依赖脚本。JD 定向以 `references/04-jd-tailoring.md` 岗位画像为主。

### JD 解析

```bash
# 从 URL 解析 JD
python scripts/jd_analyzer.py --url "https://example.com/job/12345"

# 从本地文件解析
python scripts/jd_analyzer.py --file jd.txt

# 与简历内容对比，输出匹配度
python scripts/jd_analyzer.py --file jd.txt --resume resume.txt
```

### 市场趋势扫描

```bash
# 扫描 AI 产品经理岗位热词
python scripts/market_scanner.py --role "AI产品经理"

# 指定平台
python scripts/market_scanner.py --role "AI产品经理" --platform boss

# 输出 markdown 报告
python scripts/market_scanner.py --role "AI产品经理" --output report.md
```

> 招聘站常反爬，扫描失败可直接跳过，不影响主流程。

## 安装依赖

```bash
pip install -r requirements.txt
```

## 安装

### Cursor / Claude Code

```bash
# 全局可用
cp -R ai-pm-resume-pipeline ~/.cursor/skills/ai-pm-resume-pipeline

# 或仅当前项目
mkdir -p .cursor/skills && cp -R ai-pm-resume-pipeline .cursor/skills/ai-pm-resume-pipeline
```

若仍装有旧版 `ai-pm-resume` / `resume-pm-rewriter`，建议移走以免触发词冲突。安装后请**新开对话**再使用。

### Claude.ai / App 网页版

上传同目录下的 `ai-pm-resume-pipeline.skill`：

1. 打开 Claude.ai → **Settings → Skills**
2. 上传 `ai-pm-resume-pipeline.skill`
3. 对话里说「帮我写 AI 产品经理简历」或「用简历流水线从零做到冲刺版」即可

> `.skill` 为 zip 包，内含 `SKILL.md` + `references/`。修改 references 后需重新打包再上传。

## 触发方式

在 Cursor / Claude / Windsurf 中使用以下触发词：

> 写简历、做简历、生成简历、简历优化、简历润色、项目经历改写、帮我改简历、简历重构、包装项目、STAR、对照 JD 改简历、resume rewrite、项目描述优化

模式口令示例：

> 用简历流水线从零做到冲刺版｜只要骨架先不升级项目｜按 Mode B 升级这段项目经历

## License

MIT（内容合并自 `ai-pm-resume` 与 `resume-pm-rewriter`）
