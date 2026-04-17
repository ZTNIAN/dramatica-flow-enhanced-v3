# Dramatica-Flow Enhanced — 项目交接文档

> 最后更新：2026-04-17
> 版本：V3（基于 V1/V2 全面升级）
> 本文档面向所有人，尤其是零基础用户。读完就能理解整个项目、怎么用、怎么继续迭代。

---

## 一、这是什么？

**Dramatica-Flow Enhanced** 是一个 **AI 自动写小说系统**。你给它一句话设定，它帮你：

1. **市场分析** — 分析目标读者偏好（引用番茄小说真实数据）
2. **构建世界观** — 角色/势力/地点/规则，全部自动生成
3. **生成大纲** — 三幕结构 + 逐章规划 + 张力曲线
4. **自动写作** — 一章一章写，每章2000字
5. **自动审计** — 9维度打分 + 17条红线一票否决
6. **自动修订** — 不合格自动改，最多返工3轮

**一句话：它不是"AI写字机器"，而是"AI理解故事"。**

---

## 二、项目地址

### GitHub 仓库

| 版本 | 地址 | 说明 |
|------|------|------|
| **原版** | https://github.com/ydsgangge-ux/dramatica-flow | 叙事逻辑强，但缺乏前期规划和质量管控 |
| **V1** | https://github.com/ZTNIAN/dramatica-flow-enhanced | 12个增强点完成但有6项"写了没接入" |
| **V2** | https://github.com/ZTNIAN/dramatica-flow-enhanced-v2 | 修复V1的核心问题 + 知识库扩充 |
| **V3（当前）** | https://github.com/ZTNIAN/dramatica-flow-enhanced-v3 | 全面升级：知识库+Web界面+动态规划+KB追踪 |

### 本地部署位置

```bash
git clone https://github.com/ZTNIAN/dramatica-flow-enhanced-v3.git
cd dramatica-flow-enhanced-v3
```

---

## 三、V1 → V2 → V3 的区别

### V1 做了什么

在原版基础上完成了12个增强点：
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

**V1 的问题：12个功能中有6项写了代码但没接入管线（仪表盘、示例库注入、知识库注入等），等于"写了但没用"。**

### V2 修了什么

修复了 V1 的核心问题：
- 质量仪表盘接入管线（每章写完自动记录评分）
- 对比示例库注入 Writer prompt（写手写作时看到"好vs坏"对比）
- 知识库注入 Architect prompt（建筑师参考写作技巧和去AI规则）
- LLM 重试增强（智能判断异常 + 指数退避）
- 动态规划器接入管线
- 写作技巧库扩充（61行→265行）
- 番茄小说市场数据引入（6份报告）
- 写作示例引入（6好1坏）

**V2 的问题：知识库只引入了一小部分，Agent提示词不够完整，动态规划器太基础，Web界面功能不全。**

### V3 做了什么

**V3 = V2 + 以下全部升级**

#### 1. 知识库全量引入（12个文件 → 30+个文件）

| 类别 | V2 | V3新增 | 说明 |
|------|-----|--------|------|
| 规则类 | 2个 | +5个 | 常见错误+修正、去AI完整指南、17条红线详解、95分审查标准、v6.0工作流 |
| 写作技巧 | 1个 | +2个 | 五感描写指南（含配比表）、Show Don't Tell详解（含转换表） |
| 题材指南 | 0 | +2个 | 玄幻题材指南、悬疑题材指南 |
| Agent专属 | 0 | +2个 | 写手专属技能库、审查者专用检查清单 |
| 索引 | 0 | +2个 | 知识库总索引、概览 |
| 番茄数据 | 6份 | +2份 | 读者画像深度分析报告、JSON数据 |
| 示例 | 7组 | +1组 | 对话修改前后对比 |
| 其他 | 0 | +1个 | 知识库查询激励系统 |

#### 2. Agent 提示词增强

| Agent | V2注入了什么 | V3新增注入 |
|-------|-------------|-----------|
| ArchitectAgent（建筑师） | 写作技巧+去AI规则 | +五感描写指南 + 常见错误预判 |
| WriterAgent（写手） | 对比示例库 | +写手技能库（钩子/对话/节奏/章末） + Show Don't Tell详解 |
| AuditorAgent（审计员） | 基本审计维度 | +审查者检查清单（9维逐项） + 完整17条红线定义 |
| MarketAnalyzerAgent（市场） | 无 | +8份番茄小说真实报告（读者画像+行为数据） |

#### 3. 动态规划器大幅升级

V2 只有基本的进度跟踪。V3 引入了完整的自适应分层公式：

```
给定总章节数，自动计算最优规划范围：

100-300章:   战役每20章, 战术每1-3章  → 轻规划
300-800章:   战役每20-30章, 战术每2-4章 → 标准规划
800-1500章:  战役每30-50章, 战术每3-5章 → 强化规划
1500-3000章: 战役每50-80章, 战术每5-10章 → 重度规划
3000+章:     战役每80-100章, 战术每10-15章 → 四层结构（战略→卷→篇→战术）
```

**新增功能：**
- 自动生成战役规划（给定章节数，自动划分并设置高潮点）
- 审计分数反馈到张力曲线（低分降张力、高分可加速、红线强制减速）
- 四层结构（超长篇1500章+自动启用卷→篇层）

#### 4. KB 查询追踪

每次 Agent 使用知识库时自动记录查询，每章写完保存统计。可追踪：
- 哪个 Agent 查了什么知识库
- 每章的 KB 使用频率
- 知识库利用率趋势

#### 5. Web 界面增强

| 功能 | 说明 |
|------|------|
| 市场分析面板 | 步骤3顶部，一键分析目标读者，引用番茄真实数据 |
| 质量仪表盘 | 步骤6新tab，4个汇总数字 + 维度得分条形图 + 章节趋势柱状图 |
| KB查询统计 | 步骤6新tab，按角色/文件统计查询次数 |

#### 6. Web API 新增端点

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
| 适合谁 | 喜欢点按钮、看图形界面 | 喜欢敲命令、批量操作 |
| 功能 | 创建书、写章节、看状态、审计、市场分析、仪表盘 | 同上 + 全部命令 |
| 区别 | 界面友好 | 功能最全 |

**结论：日常写作用 Web UI，前期设计用 CLI。**

### 4.2 首次部署（5步）

```bash
# 第1步：克隆项目
git clone https://github.com/ZTNIAN/dramatica-flow-enhanced-v3.git
cd dramatica-flow-enhanced-v3

# 第2步：创建虚拟环境
python3 -m venv .venv
source .venv/bin/activate    # Linux/Mac
# .venv\Scripts\activate     # Windows

# 第3步：安装依赖
pip install -e .

# 第4步：配置API Key
cp .env.example .env
# 用编辑器打开 .env，填入你的 DeepSeek API Key
```

`.env` 文件内容：
```env
LLM_PROVIDER=deepseek
DEEPSEEK_API_KEY=你的key           # 去 https://platform.deepseek.com 申请
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat
DEFAULT_WORDS_PER_CHAPTER=2000
DEFAULT_TEMPERATURE=0.7
AUDITOR_TEMPERATURE=0.0
BOOKS_DIR=./books
```

```bash
# 第5步：启动
# 方式A：命令行
df --help

# 方式B：Web UI
uvicorn core.server:app --reload --host 0.0.0.0 --port 8766
# 然后浏览器打开 http://127.0.0.1:8766/
```

### 4.3 日常使用流程

```bash
# 第1步：市场分析（可选，V3增强：自动引用番茄真实数据）
df market 科幻 --premise "你的设定"

# 第2步：世界观构建（必做）
df worldbuild "废灵根少年觉醒上古传承逆袭" --genre 玄幻

# 第3步：大纲规划（必做）
df outline --book 生成的书名

# 第4步：开始写作
df write 书名          # CLI写一章
# 或用 Web UI 点「写作」按钮

# 第5步：查看状态
df status 书名

# 第6步：导出
df export 书名
```

### 4.4 命令速查表

| 命令 | 作用 | 什么时候用 |
|------|------|-----------|
| `df doctor` | 检查API连接 | 第一次用，或出问题时 |
| `df market 题材` | 市场分析 | 写新书前（可选） |
| `df worldbuild "设定"` | 世界观构建 | 写新书（必做） |
| `df outline --book 书名` | 大纲规划 | 世界观后（必做） |
| `df write 书名` | 写下一章 | 日常写作 |
| `df audit 书名 --chapter N` | 手动审计 | 对某章不满意时 |
| `df revise 书名 --chapter N` | 手动修订 | 审计不通过时 |
| `df status 书名` | 查看状态 | 随时 |
| `df export 书名` | 导出正文 | 写完后 |

### 4.5 Web UI 操作流程

1. 打开 http://127.0.0.1:8766/
2. 步骤1（API配置）：填入 DeepSeek API Key → 保存
3. 步骤2（创建书籍）：点「+ 创建新书籍」→ 填书名、题材
4. 步骤3（世界观）：先点「市场分析」看看读者喜好 → 然后「AI 生成世界观」
5. 步骤4（大纲）：AI 自动生成三幕结构 + 章纲
6. 步骤5（写作）：点「写下一章」→ AI 自动写 + 审计 + 修订
7. 步骤6（审计）：查看审计结果、质量仪表盘、KB统计
8. 步骤7（导出）：导出为 Markdown 或 TXT

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

### 坑3：Python虚拟环境报错

```bash
# 如果 pip install -e . 报 externally-managed-environment
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
```

### 坑4：catbox文件链接72小时过期

```bash
# 重新上传
curl -F "reqtype=fileupload" -F "time=72h" -F "fileToUpload=@文件路径" https://litterbox.catbox.moe/resources/internals/api.php
```

### 坑5：git push 经常挂（TLS 连接失败）⭐

```bash
# ❌ git push 经常卡死或报 GnuTLS recv error (-110)
git push origin main

# ✅ 用 GitHub Contents API 逐文件上传（见下方方法）
```

### 坑6：GitHub API 大文件上传报错

```bash
# ❌ shell 变量传大文件内容会报 Argument list too large
CONTENT=$(base64 -w0 huge_file.py)

# ✅ 用 Python urllib 直接调用（见下方脚本）
```

### 坑7：from ..llm 导入bug

```bash
# 从GitHub下载单文件后出现 from ..llm 报错
python3 -c "import pathlib; p=pathlib.Path('file.py'); p.write_text(p.read_text().replace('from ..llm','from .llm'))"
```

### 坑8：DeepSeek API Key安全

**API Key 不要发在聊天记录里！** 用 `.env` 文件配置。`.env` 不要提交到 git。

### 坑9：entry point 缓存

改了 `cli/main.py` 但 `df --help` 不显示新命令：
```bash
find . -type d -name __pycache__ -exec rm -rf {} + 2>/dev/null
pip install --force-reinstall --no-deps -e .
```

---

## 六、迭代写入方式（推荐方法）

### 为什么不推荐 git push

本服务器的 git 客户端存在 TLS 连接问题（GnuTLS recv error -110），`git push` 经常卡死。这是服务器环境问题，不是代码问题。

### 推荐方法：GitHub Contents API 逐文件上传

#### 方法1：小文件（<1MB）用 curl

```bash
TOKEN="你的GitHub Token"
REPO="ZTNIAN/dramatica-flow-enhanced-v3"
filepath="要上传的文件路径"

CONTENT=$(base64 -w0 "$filepath")
SHA=$(curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/$REPO/contents/$filepath" | \
  python3 -c "import sys,json; print(json.load(sys.stdin).get('sha',''))")

DATA="{\"message\":\"update $filepath\",\"content\":\"$CONTENT\",\"branch\":\"main\""
[ -n "$SHA" ] && DATA="$DATA,\"sha\":\"$SHA\""
DATA="$DATA}"

curl -s -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github+json" \
  "https://api.github.com/repos/$REPO/contents/$filepath" \
  -d "$DATA"
```

#### 方法2：大文件（>1MB）用 Python

```bash
cd /path/to/project
python3 << 'PYEOF'
import base64, json, urllib.request

TOKEN = "你的GitHub Token"
REPO = "ZTNIAN/dramatica-flow-enhanced-v3"
filepath = "core/server.py"

with open(filepath, "rb") as f:
    content_b64 = base64.b64encode(f.read()).decode()

req = urllib.request.Request(
    f"https://api.github.com/repos/{REPO}/contents/{filepath}",
    headers={"Authorization": f"token {TOKEN}", "Accept": "application/vnd.github+json"}
)
try:
    sha = json.loads(urllib.request.urlopen(req).read()).get("sha", "")
except:
    sha = ""

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
result = json.loads(urllib.request.urlopen(req).read())
print(f"{filepath} → {result.get('commit',{}).get('sha','ERROR')[:8]}")
PYEOF
```

#### 方法3：批量上传模板

```bash
#!/bin/bash
TOKEN="ghp_xxxxx"
REPO="ZTNIAN/dramatica-flow-enhanced-v3"

upload() {
    local f="$1" msg="$2"
    # 使用上面的 Python 脚本，替换 filepath 和 message
    python3 -c "
import base64, json, urllib.request
T='$TOKEN'; R='$REPO'; F='$f'; M='$msg'
with open(F,'rb') as fh: c=base64.b64encode(fh.read()).decode()
try:
    s=json.loads(urllib.request.urlopen(urllib.request.Request(f'https://api.github.com/repos/{R}/contents/{F}',headers={'Authorization':f'token {T}'})).read()).get('sha','')
except: s=''
d=json.dumps({'message':M,'content':c,'branch':'main',**({'sha':s} if s else {})}).encode()
r=urllib.request.urlopen(urllib.request.Request(f'https://api.github.com/repos/{R}/contents/{F}',data=d,headers={'Authorization':f'token {T}','Content-Type':'application/json'},method='PUT'))
print(f'{F} OK')
"
}

upload "core/agents/__init__.py" "更新Agent"
upload "core/pipeline.py" "更新管线"
upload "core/server.py" "更新API"
```

---

## 七、V3 是怎么迭代的

### 迭代过程

1. **用户上传交接文档和 OpenMOSS 压缩包**
   - V1 的 `PROJECT_HANDOFF.md` + `USER_MANUAL.md`
   - V2 的 `PROJECT_HANDOFF.md`
   - OpenMOSS 操作手册（gz包 + 7z包链接）

2. **AI 下载并阅读所有文档**，理解项目全貌

3. **AI 下载并解压 OpenMOSS 压缩包**，分析精华内容
   - gz 包（1.6MB）：完整的 OpenMOSS 知识库 + Agent提示词 + 代码
   - 7z 包（158KB）：归档文档 + 工作流程 + 完整测试案例

4. **AI 克隆 V2 仓库**，分析当前代码状态

5. **AI 在服务器上修改代码**（约40分钟）：
   - 复制 18 个 OpenMOSS 知识库文件到 `core/knowledge_base/`
   - 修改 `core/agents/__init__.py` — KB追踪 + 番茄数据注入 + 提示词增强
   - 修改 `core/pipeline.py` — KB激励接入 + 审计反馈到张力曲线
   - 重写 `core/dynamic_planner.py` — 完整自适应公式 + 四层结构
   - 修改 `core/server.py` — 新增 6 个 API 端点
   - 修改 `dramatica_flow_web_ui.html` — 市场分析 + 仪表盘 + KB统计面板
   - 生成 `PROJECT_HANDOFF.md` 完整交接文档

6. **用户给 GitHub Token**（格式：`ghp_xxxxx`），AI 通过 GitHub API 推送代码

7. **AI 清理 token 痕迹**，提醒用户 revoke

### 改了什么文件

| 文件 | 改动类型 | 改动内容 |
|------|---------|---------|
| `core/agents/__init__.py` | 修改 | KB追踪 + 番茄数据注入 + 4个Agent提示词增强 |
| `core/pipeline.py` | 修改 | KBIncentiveTracker接入 + 审计→张力曲线反馈 |
| `core/dynamic_planner.py` | 重写 | OpenMOSS完整自适应公式 + 四层结构 + 自动战役生成 |
| `core/server.py` | 修改 | 新增6个API端点 |
| `dramatica_flow_web_ui.html` | 修改 | +166行：市场分析面板 + 质量仪表盘 + KB统计 |
| `core/knowledge_base/rules/` | 新增5文件 | 常见错误、去AI指南、红线、95分标准、v6.0工作流 |
| `core/knowledge_base/references/` | 新增4文件 | 五感描写、Show Don't Tell、玄幻指南、悬疑指南 |
| `core/knowledge_base/agent-specific/` | 新增2文件 | 写手技能库、审查者检查清单 |
| `core/knowledge_base/indexes/` | 新增2文件 | 总索引、概览 |
| `core/knowledge_base/fanqie-data/` | 新增2文件 | 读者画像深度分析+JSON |
| `core/knowledge_base/examples/` | 新增1文件 | 对话对比示例 |
| `core/knowledge_base/` | 新增1文件 | 查询激励系统 |
| `PROJECT_HANDOFF.md` | 重写 | 完整交接文档 |

### 代码统计

- 修改文件：6个
- 新增文件：18个
- 总增行数：约3200行

---

## 八、后续迭代流程（通用模板）

每次迭代只需要做 **两件事**：

### 第1步：发交接文档

把本文件 `PROJECT_HANDOFF.md` 发给 AI。它就能读懂整个项目。

如果有新的参考资料（比如运行日志、审计报告），也一起发。

### 第2步：给 GitHub Token

```
New personal access token (classic)：ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**获取方法**：
1. 打开 https://github.com/settings/tokens
2. 点「Generate new token (classic)」
3. Note 填 "dramatica-flow-迭代"
4. 勾选 `repo` 权限（第一个勾）
5. 点「Generate token」
6. 复制 `ghp_xxxxx` 发给 AI

**⚠️ AI 推完代码后必须立刻 revoke 这个 token！** 因为 token 会出现在聊天记录里，不安全。

### AI 会做的事

1. 读交接文档 → 理解项目
2. 在服务器上修改代码
3. 用 GitHub API 逐文件推送（因为 git push 有 TLS 问题）
4. 更新交接文档
5. 告诉你推完了

### 你只需要做

1. **Revoke token**（推完后立刻做）
2. **本地拉取最新代码**：

```bash
cd dramatica-flow-enhanced-v3
git fetch origin
git reset --hard origin/main
```

---

## 九、下一步可以做什么

| 优先级 | 任务 | 说明 |
|--------|------|------|
| P1 | MiroFish式测试反馈闭环 | 模拟读者测试→反馈分类→Agent反思→改进 |
| P1 | Agent能力画像 | 类似"职工成长专家"，给每个Agent打分和改进建议 |
| P2 | 完善错误处理 | LLM超时/格式错误时的降级策略 |
| P2 | 单元测试 | 覆盖validators和新增Agent |
| P2 | 支持更多LLM | Claude、GPT-4等 |
| P2 | 导出格式增强 | PDF、EPUB格式支持 |

---

## 十、技术栈

| 组件 | 技术 |
|------|------|
| 语言 | Python 3.11+ |
| 后端 | FastAPI |
| CLI | Typer |
| 数据存储 | 文件系统（JSON + Markdown） |
| LLM | DeepSeek API（默认）/ Ollama（本地免费） |
| 前端 | 单文件 HTML（暗色主题） |
| 校验 | Pydantic v2 |

---

## 十一、文件结构

```
dramatica-flow-enhanced-v3/
├── cli/main.py                          # CLI入口，13个命令
├── core/
│   ├── agents/__init__.py               # 9个Agent（核心，最大文件）
│   ├── pipeline.py                      # 写作管线
│   ├── llm/__init__.py                  # LLM抽象层（DeepSeek/Ollama）
│   ├── narrative/__init__.py            # 叙事引擎（因果链/伏笔/时间轴）
│   ├── state/__init__.py                # 状态管理
│   ├── types/                           # 数据类型
│   ├── validators/__init__.py           # 写后验证器（13类规则）
│   ├── server.py                        # FastAPI服务器（11个API端点）
│   ├── quality_dashboard.py             # 质量仪表盘
│   ├── dynamic_planner.py               # 动态规划器（自适应分层）
│   ├── kb_incentive.py                  # 知识库查询激励
│   └── knowledge_base/                  # 知识库（30+文件）
│       ├── rules/                       # 规则类
│       ├── references/                  # 参考类
│       ├── agent-specific/              # Agent专属
│       ├── examples/                    # 写作示例
│       ├── fanqie-data/                 # 番茄市场数据
│       └── indexes/                     # 索引
├── dramatica_flow_web_ui.html           # Web UI（暗色主题单文件）
├── dramatica_flow_timeline.html         # 时间轴可视化
├── pyproject.toml                       # 项目配置
├── .env.example                         # 环境变量模板
├── PROJECT_HANDOFF.md                   # 本文件
└── USER_MANUAL.md                       # 操作手册
```

---

## 十二、Agent 体系（9个Agent）

| Agent | 职责 | 触发时机 | 输入 | 输出 |
|-------|------|---------|------|------|
| WorldBuilderAgent | 一句话→世界观 | `df worldbuild` | 设定+题材 | 角色/势力/地点JSON |
| OutlinePlannerAgent | 大纲+章纲 | `df outline` | 世界观JSON | 三幕结构+章纲JSON |
| MarketAnalyzerAgent | 市场分析 | `df market` | 题材+平台 | 风格指南+读者偏好 |
| ArchitectAgent | 规划单章蓝图 | 每章写前 | 章纲+世界状态 | 蓝图（冲突/伏笔/情感弧） |
| WriterAgent | 生成正文 | 每章写手 | 蓝图+世界状态 | 正文+结算表 |
| PatrolAgent | 快速扫描 | 写后立即 | 正文+蓝图 | 通过/打回 |
| AuditorAgent | 9维加权审计 | 巡查后 | 正文+蓝图+真相文件 | 评分+问题清单 |
| ReviserAgent | 修订正文 | 审计不通过 | 正文+问题清单 | 修订后正文 |
| SummaryAgent | 章节摘要 | 写完后 | 正文+结算表 | 结构化摘要 |

---

## 十三、写作管线流程

```
[市场分析]（可选）
    题材 → MarketAnalyzerAgent → 风格指南 + 读者偏好
    ↓
[世界构建]（必做）
    一句话设定 → WorldBuilderAgent → 世界观JSON
    ↓
[大纲规划]（必做）
    世界观 → OutlinePlannerAgent → 三幕结构 + 章纲
    ↓
[单章循环]（每章重复以下流程）
    ├── 快照备份
    ├── 建筑师：规划蓝图（注入五感+常见错误预判）
    ├── 写手：生成正文（注入写手技能库+ShowDon'tTell）+ 结算表
    ├── 验证器：零LLM硬规则扫描（13类禁止词/Tell式表达）
    ├── 巡查者：P0/P1/P2快速扫描 → 打回修正
    ├── 审计员：9维度加权评分（≥95分+单项≥85+无红线）
    │   └── 不通过 → 修订者修正 → 再审（最多3轮）
    ├── 保存最终稿
    ├── 因果链提取
    ├── 摘要生成
    ├── 状态结算
    ├── 质量仪表盘记录
    ├── 动态规划器更新（审计→张力曲线反馈）
    └── KB查询统计保存
    ↓
[导出]
    df export → Markdown / TXT
```

---

## 十四、OpenMOSS 知识库说明

V3 引入了 OpenMOSS 的全部知识库内容：

| 文件 | 内容 | 谁在用 |
|------|------|--------|
| `rules/anti_ai_rules.md` | 去AI味规则、45特征润色系统 | Writer + Architect |
| `rules/common-mistakes.md` | 常见错误及避免方法（附修正示例） | Architect |
| `rules/de-ai-guidelines.md` | 去AI味完整指南（4步法） | Writer |
| `rules/redlines.md` | 17条红线完整定义+示例+避免方法 | Auditor |
| `rules/review-criteria-95.md` | 95分审查标准详解（6维+检查清单） | Auditor |
| `rules/v6.0-workflow-overview.md` | v6.0双流程工作流概览 | 参考 |
| `references/writing-techniques/five-senses-description.md` | 五感描写+配比表 | Architect |
| `references/writing-techniques/show-dont-tell.md` | ShowDon'tTell+转换表 | Writer |
| `references/genre-guides/xianxia-guide.md` | 玄幻题材指南 | Writer + Architect |
| `references/genre-guides/mystery-guide.md` | 悬疑题材指南 | Writer + Architect |
| `agent-specific/writer-skills.md` | 写手专属技能库（钩子/对话/节奏） | Writer |
| `agent-specific/reviewer-checklist.md` | 审查者检查清单（9维逐项） | Auditor |
| `fanqie-data/` | 8份番茄市场报告 | MarketAnalyzer |
| `examples/` | 好/坏/对比写作示例 | Writer |

**OpenMOSS 完整仓库**（本次仅引入知识库部分）：
- GitHub: https://github.com/uluckyXH/OpenMOSS
- gz包: https://litter.catbox.moe/xyzwkq.gz
- 7z包: https://litter.catbox.moe/2bmxac.7z

---

*本文档由AI自动生成。下次迭代时，把本文件发给AI即可快速理解整个项目。*
