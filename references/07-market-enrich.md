# 岗位热词扫描与简历 enrichment

> 简历 Markdown 初稿/冲刺稿生成后**强制执行**。用市场热词对齐措辞，禁止为凑热词编造经历。

## 1. 何时触发

以下任一完成后，**先做热词扫描 enrichment，再问是否转 Word/PDF**：

- Mode A：A2 生成骨架 Markdown 之后（用户可先看骨架，但交付转文件前必须跑完本步）
- Mode B：第 8 步 STAR 终稿之后（若用户给了整份简历则 enrichment 整份；仅单项目则 enrichment 该项目段 + 相关自我介绍建议）
- Full Pipeline：整合冲刺终稿之后

用户说「跳过热词扫描」才可跳过；默认**不跳过**。

## 2. 执行命令（自动）

用用户「应聘岗位」作 `--role`（缺省则用 `AI产品经理`）：

```bash
# 在 skill 根目录执行（含 scripts/ 的那一层）
python scripts/market_scanner.py --role "AI产品经理" --output market_scan_report.md
```

可选：若用户明确投 Boss 等平台，可加 `--platform boss`（易反爬，失败则改 `general`）。

依赖未装时先：

```bash
pip install -r requirements.txt
```

## 3. 失败降级（仍须 enrichment，不可假装已扫描）

若脚本失败（超时、反爬、无结果）：

1. 告知用户扫描失败原因（一两句）。
2. **降级**：用本 skill `scripts/market_scanner.py` 内 `CAPABILITY_DIMENSIONS` 词表 + 用户岗位/JD（若有）做「静态热词清单」，或改用一次联网搜索岗位 JD 摘要（若环境允许）。
3. 在产出中标注：`市场扫描来源：脚本成功 | 静态词表降级 | 手动 JD`。
4. **不要**因扫描失败而跳过 enrichment 步骤。

## 4. 如何 enrichment（红线）

读 `market_scan_report.md`（或降级清单）后：

1. 抽出 Top 热词（报告「简历优化建议」中的词优先，其次各维度 Top 3）。
2. 对照当前简历，做三类标注：
   - **已覆盖**：简历已有对应经历 → 可微调措辞，自然融入热词（不堆砌）。
   - **可强化**：经历真实存在但表述偏弱 → 改自我介绍/工作经历/项目「主要工作」用词，对齐热词。
   - **未覆盖**：热词市场要、用户无证据 → **不写入简历正文**，列入「市场热词缺口说明」。
3. 输出一版 **热词 enrichment 后的完整简历 Markdown**（STAR 版式不变）。
4. 附简短对照表：

```markdown
### 市场热词 enrichment 说明
- 扫描岗位：{role}｜来源：{脚本成功/降级}
- 已对齐：{热词} → 简历「{位置}」
- 建议面试准备（未写入简历）：{缺口热词}
```

## 5. 禁止

- 禁止为匹配热词编造项目、技能、数据。
- 禁止把整份报告关键词塞进自我介绍。
- 禁止改变 STAR 版式。
- 禁止扫描失败时静默跳过本步。
