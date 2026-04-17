# Dramatica-Flow Enhanced — 项目交接文档 V3

> 最后更新：2026-04-17 11:10
> **下次把本文件发给 AI，它就能读懂整个项目，包括V2→V3改了什么、怎么继续迭代V4。**

---

## 一、这是什么？

**Dramatica-Flow Enhanced** 是一个 AI 自动写小说系统。你给它一句话设定，它帮你：

1. 构建完整世界观（角色/势力/地点/规则）
2. 生成三幕结构大纲 + 逐章规划
3. 一章一章自动写（每章2000字）
4. 写完自动审计（9维度打分 + 17条红线）
5. 不合格自动修订，最多返工3轮

**核心卖点：** 它不是"AI写字机器"，而是"AI理解故事"——有因果链、信息边界、伏笔管理、情感弧线。

---

## 二、项目地址

| 地址 | 说明 |
|------|------|
| **原版仓库** | https://github.com/ydsgangge-ux/dramatica-flow |
| **增强版V1仓库** | https://github.com/ZTNIAN/dramatica-flow-enhanced |
| **增强版V2仓库** | https://github.com/ZTNIAN/dramatica-flow-enhanced-v2 |
| **增强版V3仓库（当前）** | https://github.com/ZTNIAN/dramatica-flow-enhanced-v2 — 同一仓库，main分支已更新到V3 |
| **OpenMOSS知识库来源** | https://github.com/uluckyXH/OpenMOSS |
| OpenMOSS gz包 | https://litter.catbox.moe/xyzwkq.gz |
| OpenMOSS 7z包 | https://litter.catbox.moe/2bmxac.7z |

---

## 三、V2 和 V3 的区别

### V2 做了什么

V2 修复了 V1 的 P0/P1 问题：
- 质量仪表盘接入管线
- 对比示例库注入 Writer prompt
- 知识库注入 Architect prompt
- LLM 重试增强
- 动态规划器接入管线
- 写作技巧库扩充
- 番茄小说数据引入
- 写作示例引入

### V2 遗留问题

| # | 问题 | 说明 |
|---|------|------|
| 1 | 知识库只引入了一小部分 | OpenMOSS 有大量规则/技巧/示例/指南未引入 |
| 2 | Agent 提示词不够完整 | 缺少 OODA 循环、写手技能库、审查者检查清单 |
| 3 | 动态规划器太基础 | 只有基本的进度跟踪，没有自适应分层公式 |
| 4 | 没有测试反馈闭环 | 缺少 MiroFish 式的读者测试→反馈→优化循环 |
| 5 | 没有题材指南 | 玄幻/悬疑等题材的具体写作指南 |
| 6 | 五感描写和 Show Don't Tell 不够详细 | 只有概要，缺少详细的技巧和示例 |

### V3 做了什么

**V3 = V2 + OpenMOSS 全量知识库集成 + 动态规划器升级 + 管线优化**

| 改动 | 文件 | 效果 |
|------|------|------|
| 知识库全量引入 | `core/knowledge_base/` | 从12个文件扩展到30+个文件 |
| 五感描写指南注入 Architect | `core/agents/__init__.py` | 建筑师规划蓝图时自动参考五感配比 |
| 常见错误注入 Architect | `core/agents/__init__.py` | 建筑师在 risk_scan 中预判常见错误 |
| 写手技能库注入 Writer | `core/agents/__init__.py` | 写手写作时参考开篇钩子/人物出场/对话技巧/节奏控制 |
| Show Don't Tell 详解注入 Writer | `core/agents/__init__.py` | 写手写完后自查，确保没有直接说"感到XX" |
| 审查者检查清单注入 Auditor | `core/agents/__init__.py` | 审计员按9维逐项检查清单核对 |
| 完整红线清单注入 Auditor | `core/agents/__init__.py` | 17条红线详细定义+示例+避免方法 |
| 动态规划器升级 | `core/dynamic_planner.py` | 引入 OpenMOSS 完整自适应公式（100-5000+章） |
| 动态规划器自动战役生成 | `core/dynamic_planner.py` | 根据总章节数自动生成战役规划 |
| 审计分数反馈到张力曲线 | `core/pipeline.py` | 低分自动降张力，高分可加速，红线强制减速 |
| 卷/篇规划层 | `core/dynamic_planner.py` | 超长篇1500章+自动启用四层结构 |
| 番茄读者画像深度数据 | `core/knowledge_base/fanqie-data/` | 补充7z包中的深度分析报告和JSON数据 |
| 题材指南 | `core/knowledge_base/references/genre-guides/` | 玄幻+悬疑题材写作指南 |

---

## 四、V3 引入的全部 OpenMOSS 知识库

### 规则类（core/knowledge_base/rules/）

| 文件 | 内容 | 谁在用 |
|------|------|--------|
| `anti_ai_rules.md` | 去AI味规则、45特征润色系统 | Writer + Architect |
| `common-mistakes.md` **[NEW]** | 常见错误及避免方法（附修正示例） | Architect（risk_scan） |
| `de-ai-guidelines.md` **[NEW]** | 去AI味完整指南（4步法+45特征详解） | Writer（自检） |
| `redlines.md` **[NEW]** | 17条红线完整定义+示例+避免方法 | Auditor |
| `review-criteria-95.md` **[NEW]** | 95分审查标准详解（6维+检查清单） | Auditor |
| `v6.0-workflow-overview.md` **[NEW]** | v6.0双流程工作流概览 | 参考 |

### 写作技巧类（core/knowledge_base/references/writing-techniques/）

| 文件 | 内容 | 谁在用 |
|------|------|--------|
| `five-senses-description.md` **[NEW]** | 五感描写技巧+配比表+快速生成表 | Architect（蓝图标注） |
| `show-dont-tell.md` **[NEW]** | Show Don't Tell详解+转换表+常用词汇表 | Writer（写后自查） |

### 题材指南类（core/knowledge_base/references/genre-guides/）

| 文件 | 内容 | 谁在用 |
|------|------|--------|
| `xianxia-guide.md` **[NEW]** | 玄幻题材写作指南 | Writer + Architect |
| `mystery-guide.md` **[NEW]** | 悬疑题材写作指南 | Writer + Architect |

### Agent 专属类（core/knowledge_base/agent-specific/）

| 文件 | 内容 | 谁在用 |
|------|------|--------|
| `writer-skills.md` **[NEW]** | 写手专属技能库（钩子/五感/对话/节奏/章末） | Writer |
| `reviewer-checklist.md` **[NEW]** | 审查者专用检查清单（9维逐项+红线逐条） | Auditor |

### 示例类（core/knowledge_base/examples/）

| 目录/文件 | 内容 | 状态 |
|-----------|------|------|
| `good/` (6个) | 正面写作示例（对话/心理/钩子/场景） | V2已有 |
| `bad/ai-flavor-heavy.md` | 反面示例（AI味重） | V2已有 |
| `before-after/revision-01-scene.md` | 修改前后对比-场景 | V2已有 |
| `before-after/revision-02-dialogue.md` **[NEW]** | 修改前后对比-对话 | V3新增 |

### 数据类（core/knowledge_base/fanqie-data/）

| 文件 | 内容 | 状态 |
|------|------|------|
| 6份市场报告 | 番茄小说市场调研 | V2已有 |
| `番茄读者画像深度分析报告_v1.0.md` **[NEW]** | 读者画像深度分析（7z包） | V3新增 |
| `番茄读者画像深度数据_v1.0.json` **[NEW]** | 读者画像JSON数据（7z包） | V3新增 |

### 索引类（core/knowledge_base/indexes/）

| 文件 | 内容 | 状态 |
|------|------|------|
| `master-index.md` **[NEW]** | 知识库总索引 | V3新增 |
| `knowledge-base-overview.md` **[NEW]** | 知识库概览 | V3新增 |

### 其他（core/knowledge_base/）

| 文件 | 内容 | 状态 |
|------|------|------|
| `query-incentive-system.md` **[NEW]** | 知识库查询激励系统（积分+称号） | V3新增（待接入管线） |
| `writing_techniques.md` | 写作技巧库（V2扩充版） | V2已有 |
| `before_after_examples.md` | 对比示例库 | V2已有 |

---

## 五、动态规划器 V3 升级

### 核心变化

V2 的动态规划器只有基本的进度跟踪。V3 引入了 OpenMOSS 的完整自适应分层公式：

```python
# 根据总章节数自动计算最优范围
100-300章:   战役20章, 战术1-3章  → 轻规划
300-800章:   战役20-30章, 战术2-4章 → 标准规划
800-1500章:  战役30-50章, 战术3-5章 → 强化规划
1500-3000章: 战役50-80章, 战术5-10章 → 重度规划
3000+章:     战役80-100章, 战术10-15章 → 四层结构
```

### 新增功能

1. **自动生成战役规划**：给定总章节数，自动划分战役并设置高潮点
2. **审计分数反馈**：低分自动降后续张力，高分可加速，红线强制减速
3. **四层结构**：超长篇自动启用 战略→卷→篇→战术 四层
4. **中途调整**：支持中途修改总章节数，自动重算所有范围

### 审计反馈规则

```
红线触发 → 后续5章张力降2
审计分 < 85 → 后续3章张力降1
审计分 >= 95 → 后续2章张力升1
```

---

## 六、Agent 体系（9个Agent）

| Agent | 职责 | V3 增强 |
|-------|------|---------|
| WorldBuilderAgent | 从一句话生成世界观 | — |
| OutlinePlannerAgent | 生成三幕大纲+章纲 | — |
| MarketAnalyzerAgent | 市场分析 | 番茄读者画像深度数据 |
| ArchitectAgent | 规划单章蓝图 | +五感描写指南+常见错误预判 |
| WriterAgent | 生成章节正文 | +写手技能库+Show Don't Tell详解 |
| PatrolAgent | 快速扫描（P0/P1/P2） | — |
| AuditorAgent | 9维加权审计 | +审查者检查清单+完整红线定义 |
| ReviserAgent | 修订正文 | — |
| SummaryAgent | 生成章节摘要 | — |

---

## 七、文件结构

```
dramatica-flow-enhanced-v2/
├── cli/main.py                          # CLI入口，13个命令
├── core/
│   ├── agents/__init__.py               # 9个Agent（核心，最大文件）V3增强
│   ├── pipeline.py                      # 写作管线 V3增强
│   ├── llm/__init__.py                  # LLM抽象层
│   ├── narrative/__init__.py            # 叙事引擎
│   ├── state/__init__.py                # 状态管理
│   ├── types/                           # 数据类型
│   ├── validators/__init__.py           # 写后验证器
│   ├── server.py                        # FastAPI服务器
│   ├── quality_dashboard.py             # 质量仪表盘
│   ├── dynamic_planner.py               # 动态规划器 V3大幅升级
│   ├── kb_incentive.py                  # 知识库查询激励
│   └── knowledge_base/
│       ├── anti_ai_rules.md             # 去AI味规则
│       ├── writing_techniques.md        # 写作技巧库
│       ├── before_after_examples.md     # 对比示例
│       ├── rules/                       # V3新增：规则类知识库
│       │   ├── common-mistakes.md
│       │   ├── de-ai-guidelines.md
│       │   ├── redlines.md
│       │   ├── review-criteria-95.md
│       │   └── v6.0-workflow-overview.md
│       ├── references/                  # V3新增：参考类知识库
│       │   ├── writing-techniques/
│       │   │   ├── five-senses-description.md
│       │   │   └── show-dont-tell.md
│       │   └── genre-guides/
│       │       ├── xianxia-guide.md
│       │       └── mystery-guide.md
│       ├── agent-specific/              # V3新增：Agent专属知识库
│       │   ├── writer-skills.md
│       │   └── reviewer-checklist.md
│       ├── indexes/                     # V3新增：索引
│       │   ├── master-index.md
│       │   └── knowledge-base-overview.md
│       ├── query-incentive-system.md    # V3新增：查询激励
│       ├── fanqie-data/                 # 番茄市场数据（V3补充）
│       └── examples/                    # 写作示例
├── dramatica_flow_web_ui.html           # Web UI
├── pyproject.toml
├── .env                                 # API Key配置（不提交git）
├── CHANGELOG-V2.md
├── PROJECT_HANDOFF.md                   # 本文件
└── USER_MANUAL.md
```

---

## 八、V3 还没做的事

| 优先级 | 任务 | 说明 |
|--------|------|------|
| P1 | Web UI 添加世界观构建页面 | 表单输入设定，点击生成，可视化展示结果 |
| P1 | Web UI 添加大纲规划页面 | 展示三幕结构 + 章纲表格 + 张力曲线图 |
| P1 | 知识库查询激励接入管线 | Agent 实际调用 kb_incentive 记录查询 |
| P1 | 番茄数据注入 MarketAnalyzer | 把市场报告注入到市场分析Agent的prompt |
| P2 | 完善错误处理 | LLM超时/格式错误时的降级策略 |
| P2 | 添加单元测试 | 覆盖 validators 和新增Agent的基本功能 |
| P2 | server.py更新 | 暴露新Agent的API端点 |
| P2 | 测试反馈闭环（MiroFish式） | 模拟读者测试→反馈→Agent反思→改进 |

---

## 九、本地部署指南

### 首次部署

```bash
# 1. 克隆项目
git clone https://github.com/ZTNIAN/dramatica-flow-enhanced-v2.git
cd dramatica-flow-enhanced-v2

# 2. 创建虚拟环境
python3 -m venv .venv
source .venv/bin/activate

# 3. 安装依赖
pip install -e .

# 4. 配置API Key
cp .env.example .env
# 编辑 .env，填入你的 DeepSeek API Key
```

### 启动方式

```bash
# CLI模式
source .venv/bin/activate
df --help

# Web UI模式
uvicorn core.server:app --reload --host 0.0.0.0 --port 8766
# 浏览器打开 http://127.0.0.1:8766/
```

### 日常使用流程

```bash
df market 科幻 --premise "你的设定"     # 市场分析（可选）
df worldbuild "设定" --genre 玄幻       # 世界观构建（必做）
df outline --book 书名                  # 大纲规划（必做）
df write 书名                           # 开始写作
df status 书名                          # 查看状态
df export 书名                          # 导出
```

---

## 十、如何迭代 V4

### V4 应该做什么（按优先级）

| 优先级 | 任务 | 说明 |
|--------|------|------|
| P1 | Web UI 集成世界观构建 | 目前只有CLI，Web UI缺此功能 |
| P1 | Web UI 集成大纲规划 | 展示三幕结构+章纲+张力曲线图 |
| P1 | 知识库查询激励接入管线 | Agent写完/审完后自动记录查询积分 |
| P1 | 番茄数据注入MarketAnalyzer | market命令输出时引用番茄数据 |
| P2 | 测试反馈闭环 | 模拟MiroFish读者测试→反馈分类→Agent反思 |
| P2 | Agent能力画像 | 类似OpenMOSS的职工成长专家，给Agent打分和改进建议 |
| P2 | 完善错误处理 | LLM超时/格式错误时的重试+降级策略 |
| P2 | 单元测试 | 覆盖validators和新增Agent |

### V4 迭代流程

**第1步：准备交接材料**
- 把本文件 `PROJECT_HANDOFF.md` 发给AI
- 如果有新的问题清单，也一起发

**第2步：给 GitHub Token**
- 去 https://github.com/settings/tokens → Generate new token (classic)
- 勾选 `repo` 权限
- 把 token 发给AI
- **AI推完代码后立刻 revoke 这个 token！**

**第3步：AI 在云端改代码 → 推送**

**第4步：你本地拉取**
```bash
git fetch origin && git reset --hard origin/main
```

---

## 十一、踩坑记录

### 坑1：heredoc写中文文件会损坏
```bash
# ✅ 用这个
python3 -c "with open('file','w') as f: f.write('中文内容')"
```

### 坑2：sed无法匹配中文字符
```bash
# ✅ 用这个
python3 -c "import pathlib; p=pathlib.Path('file'); p.write_text(p.read_text().replace('中文','替换'))"
```

### 坑3：GitHub推送TLS连接失败
用 GitHub Contents API 逐文件上传。

### 坑4：catbox文件链接72小时过期
交接文档里的压缩包链接会过期，需要重新上传：
```bash
curl -F "reqtype=fileupload" -F "time=72h" -F "fileToUpload=@文件路径" https://litterbox.catbox.moe/resources/internals/api.php
```

---

## 十二、技术栈

| 组件 | 技术 |
|------|------|
| 语言 | Python 3.11+ |
| 后端 | FastAPI |
| CLI | Typer |
| 数据存储 | 文件系统（JSON + Markdown） |
| LLM | DeepSeek API / Ollama |
| 前端 | 单文件 HTML |
| 校验 | Pydantic v2 |

---

## 十三、V2→V3 知识库对比

| 维度 | V2 | V3 |
|------|----|----|
| 知识库文件数 | 12 | 30+ |
| 规则类 | 2个（部分） | 6个（完整） |
| 写作技巧 | 1个 | 3个（含五感+Show Don't Tell） |
| 题材指南 | 0 | 2个（玄幻+悬疑） |
| Agent专属 | 0 | 2个（写手技能+审查清单） |
| 索引 | 0 | 2个 |
| 番茄数据 | 6份 | 8份（补充深度分析） |
| 动态规划器 | 基本进度跟踪 | 完整自适应公式+审计反馈 |
| Agent提示词注入 | 去AI味+对比示例 | +五感+常见错误+写手技能+Show Don't Tell+审查清单+完整红线 |

---

*本文档由AI自动生成。下次迭代时，把本文件发给AI即可快速理解整个项目。*
