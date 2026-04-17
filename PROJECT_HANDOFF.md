# Dramatica-Flow Enhanced — 项目交接文档 V3

> 最后更新：2026-04-17
> 本文档面向所有人，尤其是零基础用户。读完就能理解整个项目、怎么用、怎么继续迭代。

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

### GitHub 仓库

| 版本 | 仓库地址 | 说明 |
|------|---------|------|
| **原版** | https://github.com/ydsgangge-ux/dramatica-flow | 叙事逻辑强，但缺乏前期规划和质量管控 |
| **V1** | https://github.com/ZTNIAN/dramatica-flow-enhanced | 12个增强点完成但有6项未接入 |
| **V2** | https://github.com/ZTNIAN/dramatica-flow-enhanced-v2 | 修复P0/P1问题 + 知识库扩充 |
| **V3（当前）** | https://github.com/ZTNIAN/dramatica-flow-enhanced-v3 | 全面集成OpenMOSS + Web API + KB追踪 |

### 本地部署位置

```bash
# 克隆V3
git clone https://github.com/ZTNIAN/dramatica-flow-enhanced-v3.git
cd dramatica-flow-enhanced-v3
```

---

## 三、V1 → V2 → V3 的区别

### V1 做了什么

V1 在原版基础上完成了12个增强点：
- 9维度加权评分 + 17条红线一票否决
- 禁止词汇清单 + 正则扫描
- 知识库目录 + 去AI味规则
- 45条写作风格约束
- Show Don't Tell 转换表
- 对比示例库
- 返工上限3次 + 监控
- 动态分层规划器
- 巡查Agent
- 质量统计仪表盘
- 知识库查询激励

**V1 的问题**：12个功能中有6项写了代码但没接入管线（仪表盘、示例库注入、知识库注入等），等于"写了但没用"。

### V2 修了什么

V2 修复了 V1 的核心问题：
- 质量仪表盘接入管线（每章写完自动记录评分）
- 对比示例库注入 Writer prompt（写手写作时看到"好vs坏"对比）
- 知识库注入 Architect prompt（建筑师参考写作技巧和去AI规则）
- LLM 重试增强（智能判断异常 + 指数退避）
- 动态规划器接入管线
- 写作技巧库扩充（61行→265行）
- 番茄小说市场数据引入（6份报告）
- 写作示例引入（6好1坏）

**V2 的问题**：知识库只引入了一小部分，Agent提示词不够完整，动态规划器太基础。

### V3 做了什么

**V3 = V2 基础 + OpenMOSS 全量知识库 + 动态规划器升级 + Web API + KB追踪**

#### 知识库全量引入

| 类别 | V2 文件数 | V3 文件数 | 新增内容 |
|------|----------|----------|---------|
| 规则类 | 2 | 7 | 常见错误+修正、去AI指南、红线详解、95分标准、v6.0工作流 |
| 写作技巧 | 1 | 3 | 五感描写指南、Show Don't Tell详解 |
| 题材指南 | 0 | 2 | 玄幻指南、悬疑指南 |
| Agent专属 | 0 | 2 | 写手技能库、审查者检查清单 |
| 索引 | 0 | 2 | 知识库总索引、概览 |
| 数据 | 6 | 8 | 番茄读者画像深度分析+JSON |
| 示例 | 7 | 8 | +1组对话对比示例 |
| 其他 | 1 | 2 | 查询激励系统 |
| **总计** | **12** | **30+** | |

#### Agent 提示词增强

| Agent | V2 | V3 新增注入 |
|-------|-----|------------|
| ArchitectAgent | 写作技巧+去AI规则 | +五感描写指南 + 常见错误预判 |
| WriterAgent | 对比示例库 | +写手技能库 + Show Don't Tell详解 |
| AuditorAgent | 基本审计维度 | +审查者检查清单 + 完整17条红线定义 |
| MarketAnalyzerAgent | 无数据注入 | +8份番茄市场真实报告（读者画像+行为数据） |

#### 动态规划器大幅升级

V2 只有基本的进度跟踪。V3 引入了 OpenMOSS 的完整自适应公式：

```
总章节数 → 自动计算最优规划范围：
100-300章:   战役20章, 战术1-3章  → 轻规划
300-800章:   战役20-30章, 战术2-4章 → 标准规划
800-1500章:  战役30-50章, 战术3-5章 → 强化规划
1500-3000章: 战役50-80章, 战术5-10章 → 重度规划
3000+章:     战役80-100章, 战术10-15章 → 四层结构
```

新增功能：
- **自动生成战役规划**：给定总章节数，自动划分战役并设置高潮点
- **审计分数反馈**：低分（<85）→后续3章张力降1；高分（≥95）→后续2章升1；红线→后续5章降2
- **四层结构**：超长篇1500章+自动启用 战略→卷→篇→战术

#### 管线 KB 查询追踪

每次 Agent 使用知识库时自动记录查询，每章写完保存统计到 `kb_queries.json`。可追踪：
- 哪个 Agent 查了什么知识库
- 每章的 KB 使用频率
- 知识库利用率趋势

#### Web API 新增端点

| 端点 | 方法 | 用途 |
|------|------|------|
| `/api/books/{id}/worldbuild` | POST | 世界观构建 |
| `/api/books/{id}/outline` | POST | 大纲规划 |
| `/api/action/market` | POST | 市场分析（注入番茄数据） |
| `/api/books/{id}/quality-dashboard` | GET | 质量仪表盘数据 |
| `/api/books/{id}/kb-queries` | GET | KB查询统计 |

---

## 四、小白操作手册

### 4.1 两种用法

| | Web UI（浏览器） | CLI（命令行） |
|--|-----------------|---------------|
| 怎么打开 | 浏览器打开 http://127.0.0.1:8766/ | 终端输入 `df` 命令 |
| 适合谁 | 喜欢点击按钮、看图形界面 | 喜欢敲命令、批量操作 |
| 功能 | 创建书、写章节、看状态、审计 | 同上 + 世界观构建/大纲规划/市场分析 |
| 区别 | 界面友好，但功能可能不全 | 功能最全，新功能优先在CLI |

**结论：日常写作用Web UI，前期设计（世界观/大纲）用CLI。**

### 4.2 首次部署

```bash
# 1. 克隆项目
git clone https://github.com/ZTNIAN/dramatica-flow-enhanced-v3.git
cd dramatica-flow-enhanced-v3

# 2. 创建虚拟环境
python3 -m venv .venv
source .venv/bin/activate    # Linux/Mac
# .venv\Scripts\activate     # Windows

# 3. 安装依赖
pip install -e .

# 4. 配置API Key
cp .env.example .env
# 编辑 .env，填入你的 DeepSeek API Key
```

### 4.3 .env 配置

```env
LLM_PROVIDER=deepseek           # 用 deepseek 或 ollama（本地免费）
DEEPSEEK_API_KEY=你的key         # 去 https://platform.deepseek.com 申请
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat
DEFAULT_WORDS_PER_CHAPTER=2000   # 每章字数
DEFAULT_TEMPERATURE=0.7          # 写作温度（越高越随机）
AUDITOR_TEMPERATURE=0.0          # 审计温度（0=最客观）
BOOKS_DIR=./books                # 书籍数据目录
```

### 4.4 启动方式

```bash
# 方式A：命令行
source .venv/bin/activate
df --help          # 查看所有命令
df doctor          # 检查API连接

# 方式B：Web UI
uvicorn core.server:app --reload --host 0.0.0.0 --port 8766
# 浏览器打开 http://127.0.0.1:8766/
```

### 4.5 日常使用流程

```bash
# 1. 市场分析（可选，V3增强：自动引用番茄真实数据）
df market 科幻 --premise "你的设定"

# 2. 世界观构建（必做）
df worldbuild "废灵根少年觉醒上古传承逆袭" --genre 玄幻

# 3. 大纲规划（必做）
df outline --book 生成的书名

# 4. 开始写作
df write 书名        # CLI写一章
# 或用Web UI点按钮

# 5. 查看状态
df status 书名

# 6. 导出
df export 书名
```

### 4.6 命令速查表

| 命令 | 作用 | 什么时候用 |
|------|------|-----------|
| `df doctor` | 诊断API连接 | 第一次用，或出问题时 |
| `df market 题材` | 市场分析（V3增强） | 开始写新书前（可选） |
| `df worldbuild "设定"` | 构建世界观 | 开始写新书（必做） |
| `df outline --book 书名` | 生成大纲 | 世界观构建后（必做） |
| `df write 书名` | 写下一章 | 日常写作 |
| `df audit 书名 --chapter N` | 手动审计第N章 | 对某章质量不满意时 |
| `df revise 书名 --chapter N` | 手动修订第N章 | 审计不通过时 |
| `df status 书名` | 查看状态 | 随时 |
| `df export 书名` | 导出正文 | 写完后 |

---

## 五、踩坑记录（重要！）

### 坑1：heredoc写中文文件会损坏

```bash
# ❌ 不要用
cat > file << 'EOF' 中文内容 EOF

# ✅ 用这个
python3 -c "with open('file','w') as f: f.write('中文内容')"
```

### 坑2：sed无法匹配中文字符

```bash
# ❌ 不要用
sed -i 's/中文/替换/' file

# ✅ 用这个
python3 -c "import pathlib; p=pathlib.Path('file'); p.write_text(p.read_text().replace('中文','替换'))"
```

### 坑3：Python虚拟环境

```bash
# 如果 pip install -e . 报 externally-managed-environment
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
```

### 坑4：catbox文件链接72小时过期

交接文档里的压缩包链接会过期，需要重新上传：
```bash
curl -F "reqtype=fileupload" -F "time=72h" -F "fileToUpload=@文件路径" https://litterbox.catbox.moe/resources/internals/api.php
```

### 坑5：git push 经常挂（TLS 连接失败）⭐

```bash
# ❌ git push 经常卡死或报 GnuTLS recv error
git push origin main

# ✅ 用 GitHub API 逐文件上传（稳定可靠）
# 方法见下方"迭代写入方式"章节
```

### 坑6：GitHub API 大文件上传

单个文件内容通过 shell 变量传递会报 `Argument list too large`。
```bash
# ✅ 用 Python urllib 直接调用 API（见下方脚本）
```

### 坑7：from ..llm 导入bug

```bash
# 从GitHub下载文件后出现 from ..llm 报错
python3 -c "import pathlib; p=pathlib.Path('file.py'); p.write_text(p.read_text().replace('from ..llm','from .llm'))"
```

### 坑8：DeepSeek API Key安全

API Key 不要发在聊天记录里！用 `.env` 文件配置。`.env` 文件不要提交到 git。

---

## 六、迭代写入方式（推荐方法）

### 为什么不推荐 git push

本服务器的 git 客户端存在 TLS 连接问题（GnuTLS recv error -110），`git push` 经常卡死。这是环境问题，不是代码问题。

### 推荐方法：GitHub Contents API 逐文件上传

#### 小文件（<1MB）用 curl

```bash
TOKEN="你的GitHub Token"
REPO="ZTNIAN/dramatica-flow-enhanced-v3"
filepath="core/agents/__init__.py"

# 读取文件并base64编码
CONTENT=$(base64 -w0 "$filepath")

# 获取已有文件的SHA（如果存在）
SHA=$(curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/$REPO/contents/$filepath" | \
  python3 -c "import sys,json; print(json.load(sys.stdin).get('sha',''))")

# 构造请求
DATA="{\"message\":\"update $filepath\",\"content\":\"$CONTENT\",\"branch\":\"main\""
[ -n "$SHA" ] && DATA="$DATA,\"sha\":\"$SHA\""
DATA="$DATA}"

# 上传
curl -s -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github+json" \
  "https://api.github.com/repos/$REPO/contents/$filepath" \
  -d "$DATA"
```

#### 大文件（>1MB）用 Python

```bash
cd /path/to/project
python3 << 'PYEOF'
import base64, json, urllib.request

TOKEN = "你的GitHub Token"
REPO = "ZTNIAN/dramatica-flow-enhanced-v3"
filepath = "core/server.py"

# 读取文件
with open(filepath, "rb") as f:
    content_b64 = base64.b64encode(f.read()).decode()

# 获取已有SHA
req = urllib.request.Request(
    f"https://api.github.com/repos/{REPO}/contents/{filepath}",
    headers={"Authorization": f"token {TOKEN}", "Accept": "application/vnd.github+json"}
)
try:
    resp = urllib.request.urlopen(req)
    sha = json.loads(resp.read()).get("sha", "")
except:
    sha = ""

# 上传
data = json.dumps({
    "message": "update " + filepath,
    "content": content_b64,
    "branch": "main",
    **({"sha": sha} if sha else {}),
}).encode()

req = urllib.request.Request(
    f"https://api.github.com/repos/{REPO}/contents/{filepath}",
    data=data,
    headers={
        "Authorization": f"token {TOKEN}",
        "Accept": "application/vnd.github+json",
        "Content-Type": "application/json",
    },
    method="PUT"
)
resp = urllib.request.urlopen(req)
result = json.loads(resp.read())
print(f"{filepath} → {result.get('commit', {}).get('sha', 'ERROR')[:8]}")
PYEOF
```

#### 批量上传模板

```bash
#!/bin/bash
TOKEN="你的GitHub Token"
REPO="ZTNIAN/dramatica-flow-enhanced-v3"

upload() {
    local f="$1"
    local msg="$2"
    # ... (使用上面的 Python 脚本)
}

upload "core/agents/__init__.py" "更新Agent提示词"
upload "core/pipeline.py" "更新管线"
upload "core/server.py" "更新API端点"
```

---

## 七、V3 是怎么迭代的

### 迭代过程

1. **用户上传交接文档和 OpenMOSS 压缩包**（V1 的 md + V2 的 md + 操作手册 + gz/7z 链接）
2. **AI 下载并阅读所有文档**，理解项目全貌
3. **AI 下载并解压 OpenMOSS 压缩包**，分析精华内容
4. **AI 克隆 V2 仓库**，分析当前代码状态
5. **AI 在服务器上修改代码**（约30分钟）：
   - 复制 18 个 OpenMOSS 知识库文件到 `core/knowledge_base/`
   - 修改 `core/agents/__init__.py` — 添加 KB 追踪 + 番茄数据注入 + Agent 提示词增强
   - 修改 `core/pipeline.py` — KB 激励接入 + 审计分数反馈到张力曲线
   - 修改 `core/dynamic_planner.py` — 完整自适应公式 + 四层结构
   - 修改 `core/server.py` — 新增 6 个 API 端点
   - 生成 PROJECT_HANDOFF.md 交接文档
6. **用户给 GitHub Token**，AI 通过 GitHub API 推送代码到新仓库
7. **AI 清理 token 痕迹**，提醒用户 revoke

### 改了什么文件

| 文件 | 改动类型 | 改动内容 |
|------|---------|---------|
| `core/agents/__init__.py` | 修改 | KB追踪机制 + 番茄数据注入 + Agent提示词增强 |
| `core/pipeline.py` | 修改 | KBIncentiveTracker接入 + 审计反馈到张力曲线 |
| `core/dynamic_planner.py` | 重写 | OpenMOSS完整自适应公式 + 四层结构 + 自动战役生成 |
| `core/server.py` | 修改 | 新增6个API端点 |
| `core/knowledge_base/rules/` | 新增5文件 | 常见错误、去AI指南、红线、95分标准、v6.0工作流 |
| `core/knowledge_base/references/` | 新增4文件 | 五感描写、Show Don't Tell、玄幻指南、悬疑指南 |
| `core/knowledge_base/agent-specific/` | 新增2文件 | 写手技能库、审查者检查清单 |
| `core/knowledge_base/indexes/` | 新增2文件 | 总索引、概览 |
| `core/knowledge_base/fanqie-data/` | 新增2文件 | 读者画像深度分析+JSON |
| `core/knowledge_base/examples/` | 新增1文件 | 对话对比示例 |
| `core/knowledge_base/` | 新增1文件 | 查询激励系统 |
| `PROJECT_HANDOFF.md` | 新增/重写 | 完整交接文档 |

### 给 Token 的格式

```
New personal access token (classic)：ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

去 https://github.com/settings/tokens → Generate new token (classic) → 勾选 `repo` 权限 → 生成。

**⚠️ AI 推完代码后必须立刻 revoke 这个 token！** 因为 token 会出现在聊天记录里，不安全。

---

## 八、后续迭代流程（通用模板）

每次迭代只需要两件事：

### 第1步：发交接文档

把本文件 `PROJECT_HANDOFF.md` 发给 AI。它就能读懂整个项目：
- 当前版本做了什么
- 代码结构是什么
- 哪些文件是核心
- 还有什么没做

如果有新的问题清单或参考资料，也一起发。

### 第2步：给 GitHub Token

```
New personal access token (classic)：ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

AI 会：
1. 读交接文档 → 理解项目
2. 在服务器上修改代码
3. 用 GitHub API 推送代码
4. 更新交接文档
5. 清理 token 痕迹

你只需要：
1. **Revoke token**（推完立刻做）
2. **本地拉取最新代码**：`git fetch origin && git reset --hard origin/main`

### 本地拉取最新代码

```bash
cd dramatica-flow-enhanced-v3
git fetch origin
git reset --hard origin/main
# 测试新功能
```

---

## 九、V3 还没做的事（留给 V4）

| 优先级 | 任务 | 说明 |
|--------|------|------|
| P1 | Web UI 前端集成世界观构建 | HTML添加表单+结果展示，后端API已就绪 |
| P1 | Web UI 前端集成大纲规划 | HTML添加章纲表格+张力曲线图，后端API已就绪 |
| P1 | Web UI 前端集成市场分析 | HTML添加结果展示，后端API已就绪 |
| P1 | Web UI 添加质量仪表盘面板 | 各章评分趋势+维度雷达图 |
| P1 | Web UI 添加KB查询统计面板 | 展示Agent查询知识库频率 |
| P2 | 测试反馈闭环 | 模拟MiroFish读者测试→反馈→Agent反思 |
| P2 | Agent能力画像 | 类似OpenMOSS的职工成长专家 |
| P2 | 完善错误处理 | LLM超时/格式错误降级策略 |
| P2 | 单元测试 | 覆盖validators和新增Agent |

---

## 十、技术栈

| 组件 | 技术 |
|------|------|
| 语言 | Python 3.11+ |
| 后端 | FastAPI |
| CLI | Typer |
| 数据存储 | 文件系统（JSON + Markdown） |
| LLM | DeepSeek API（默认）/ Ollama（本地免费） |
| 前端 | 单文件 HTML |
| 校验 | Pydantic v2 |

---

## 十一、文件结构速查

```
dramatica-flow-enhanced-v3/
├── cli/main.py                          # CLI入口，13个命令
├── core/
│   ├── agents/__init__.py               # 9个Agent（核心，最大文件）
│   ├── pipeline.py                      # 写作管线
│   ├── llm/__init__.py                  # LLM抽象层
│   ├── narrative/__init__.py            # 叙事引擎
│   ├── state/__init__.py                # 状态管理
│   ├── types/                           # 数据类型
│   ├── validators/__init__.py           # 写后验证器
│   ├── server.py                        # FastAPI服务器（V3新增6个端点）
│   ├── quality_dashboard.py             # 质量仪表盘
│   ├── dynamic_planner.py               # 动态规划器（V3大幅升级）
│   ├── kb_incentive.py                  # 知识库查询激励
│   └── knowledge_base/                  # 知识库（30+文件）
│       ├── rules/                       # 规则类（去AI/红线/审查标准）
│       ├── references/                  # 参考类（五感/ShowDon'tTell/题材指南）
│       ├── agent-specific/              # Agent专属（写手技能/审查清单）
│       ├── examples/                    # 写作示例
│       ├── fanqie-data/                 # 番茄市场数据
│       └── indexes/                     # 索引
├── dramatica_flow_web_ui.html           # Web UI
├── pyproject.toml                       # 项目配置
├── .env.example                         # 环境变量模板
├── PROJECT_HANDOFF.md                   # 本文件
└── USER_MANUAL.md                       # 操作手册
```

---

## 十二、Agent 体系（9个Agent）

| Agent | 职责 | 触发时机 | V3增强 |
|-------|------|---------|--------|
| WorldBuilderAgent | 从一句话生成世界观 | `df worldbuild` | — |
| OutlinePlannerAgent | 生成三幕大纲+章纲 | `df outline` | — |
| MarketAnalyzerAgent | 市场分析 | `df market` | +番茄真实数据 |
| ArchitectAgent | 规划单章蓝图 | 每章写前 | +五感+常见错误 |
| WriterAgent | 生成章节正文 | 每章写手 | +写手技能库+ShowDon'tTell |
| PatrolAgent | 快速扫描（P0/P1/P2） | 写后立即 | — |
| AuditorAgent | 9维加权审计 | 巡查后 | +审查清单+完整红线 |
| ReviserAgent | 修订正文 | 审计不通过时 | — |
| SummaryAgent | 生成章节摘要 | 写完后 | — |

---

## 十三、写作管线流程

```
[世界构建] 一句话设定 → WorldBuilder → 世界观JSON
    ↓
[大纲规划] 世界观 → OutlinePlanner → 三幕结构 + 章纲
    ↓
[单章循环]（每章重复）
    ├── 快照备份
    ├── 建筑师：规划蓝图（注入五感+常见错误预判）
    ├── 写手：生成正文（注入写手技能库+ShowDon'tTell）+ 结算表
    ├── 验证器：零LLM硬规则扫描
    ├── 巡查者：P0/P1/P2快速扫描
    ├── 审计员：9维度加权评分（≥95分+单项≥85+无红线）
    │   └── 不通过 → 修订者修正 → 再审（最多3轮）
    ├── 保存最终稿
    ├── 因果链提取
    ├── 摘要生成
    ├── 状态结算
    ├── 质量仪表盘记录
    ├── 动态规划器更新（审计→张力曲线反馈）
    └── KB查询统计保存
```

---

## 十四、OpenMOSS 知识库说明

V3 引入了 OpenMOSS 的全部知识库内容，存放在 `core/knowledge_base/` 下：

| 文件 | 内容 | 谁在用 |
|------|------|--------|
| `rules/anti_ai_rules.md` | 去AI味规则、45特征润色系统 | Writer + Architect |
| `rules/common-mistakes.md` | 常见错误及避免方法 | Architect（risk_scan） |
| `rules/de-ai-guidelines.md` | 去AI味完整指南 | Writer（自检） |
| `rules/redlines.md` | 17条红线完整定义+示例 | Auditor |
| `rules/review-criteria-95.md` | 95分审查标准详解 | Auditor |
| `rules/v6.0-workflow-overview.md` | v6.0双流程工作流 | 参考 |
| `references/writing-techniques/five-senses-description.md` | 五感描写+配比表 | Architect |
| `references/writing-techniques/show-dont-tell.md` | ShowDon'tTell+转换表 | Writer |
| `references/genre-guides/xianxia-guide.md` | 玄幻题材指南 | Writer + Architect |
| `references/genre-guides/mystery-guide.md` | 悬疑题材指南 | Writer + Architect |
| `agent-specific/writer-skills.md` | 写手专属技能库 | Writer |
| `agent-specific/reviewer-checklist.md` | 审查者检查清单 | Auditor |
| `examples/` | 好/坏/对比写作示例 | Writer |
| `fanqie-data/` | 番茄市场报告+读者画像 | MarketAnalyzer |

**OpenMOSS 完整仓库**（本次未全部引入代码部分，V4可继续）：
- `app/` — FastAPI后端（任务管理/Agent协作/评审系统）
- `prompts/role/` — 25个专业Agent的提示词
- `knowledge-base/` — 已全部引入
- `static/` — Web管理界面

---

*本文档由AI自动生成。下次迭代时，把本文件发给AI即可快速理解整个项目。*
