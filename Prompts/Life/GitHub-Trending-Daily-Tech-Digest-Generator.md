# GitHub Trending Daily Tech Digest Generator

## 📋 Task Objective

Generate a **Simplified Chinese technical digest** for the Top 10 repositories from **yesterday's** (00:00-23:59, local time) GitHub Trending, covering:
1. Project functionality and purpose
2. **In-depth analysis of core tech stack, frameworks, and methodologies**
3. Technical highlights and innovations
4. Speculation on trending reasons

**Core Requirements**:
- ✅ Output entirely in **Simplified Chinese** (preserve English for technical terms)
- ✅ Engineer's perspective, technically focused, avoid marketing language
- ✅ Completeness > word count; prefer exceeding limits over losing key technical information
- ✅ All uncertain content must be marked as **"(inferred)"** or **"Not explicitly stated in README"**

---

## 🔧 Execution Steps (Platform-Adaptive)

### **Step 1: Confirm Capabilities & Acquire Data**

**For ALL models (platform-agnostic approach)**:

1. **Get current time**:
   - Claude: Call `user_time_v0`
   - ChatGPT: Use built-in time awareness or search "current date"
   - Others: Use available time/date tools or search

2. **Calculate yesterday's date** (YYYY-MM-DD format)

3. **Acquire Top 10 repositories** (try in order):
   
   **Step 3a**: Try third-party APIs
   - `https://api.gitterapp.com/repositories?since=daily`
   - `https://ghapi.huchen.dev/repositories?since=daily`
   - `https://api.ossinsight.io/v1/trending/repos/?period=daily`
   
   **Step 3b**: If APIs fail, use web search
   - Search: "GitHub Trending yesterday [date]"
   - Extract repo names from results
   
   **Step 3c**: If search fails, try direct page access
   - `https://github.com/trending?since=daily`
   - (May fail due to JavaScript requirements)
   
   **Step 3d**: If all fail, request manual input
   - Output data collection checklist
   - Wait for user to provide repo list

4. **For each repository, fetch README**:
   - Try: `https://raw.githubusercontent.com/{owner}/{repo}/main/README.md`
   - If fails: Try `master` branch
   - If fails: Use repo description + infer from structure

5. **Extract metadata** (stars, language, license) from:
   - API response (if used)
   - Repository page
   - README frontmatter

6. **Proceed to generate digest** following output format

---

### **Step 1.5: Data Source Strategy (Multi-layered Fallback)**

**CRITICAL**: GitHub Trending page uses client-side JavaScript rendering, which may be inaccessible to many LLM tools. Use the following layered approach:

#### **Tier 1: Third-party APIs (Recommended - Most Reliable)**

Try these APIs in order until one succeeds:

**Option A: GitHub Trending API (Unofficial but stable)**
```
GET https://api.gitterapp.com/repositories?since=daily&language=
```
- Returns JSON with repo name, description, stars, language
- Add `&language=python` to filter by language (optional)

**Option B: Trending API by huchenme**
```
GET https://ghapi.huchen.dev/repositories?since=daily
```
- Similar structure, different maintainer

**Option C: OSS Insight API**
```
GET https://api.ossinsight.io/v1/trending/repos/?period=daily
```
- More comprehensive, includes stats

**If all APIs fail**, proceed to Tier 2.

---

#### **Tier 2: Web Search (Fallback for models with search capabilities)**

If APIs are unavailable, search for:
```
"GitHub Trending yesterday YYYY-MM-DD"
"GitHub 热门项目 昨日"
"GitHub Trending daily top 10"
```

Look for:
- Tech news sites (e.g., dev.to, HackerNews, Reddit r/programming)
- GitHub Trending aggregator sites
- Developer newsletters that cover daily trending

Extract repository names from search results, then fetch individual repos.

---

#### **Tier 3: Direct Page Access (Last Resort)**

**Only if you have JavaScript rendering capabilities**:
```
https://github.com/trending?since=daily&spoken_language_code=
```

**Known limitations**:
- Requires JavaScript execution (most LLM tools cannot do this)
- May be blocked by rate limiting
- Page structure may change

**If this fails**: Output the data collection checklist and request manual input.

---

#### **Tier 4: Manual Input (Guaranteed Fallback)**

If all automated methods fail, output:
```
❌ 无法自动获取 GitHub Trending 数据

已尝试的方法：
- [ ] Third-party APIs (api.gitterapp.com, ghapi.huchen.dev, ossinsight.io)
- [ ] Web search for trending repositories
- [ ] Direct page access (github.com/trending)

所有自动化方法均失败。请手动提供昨日 (YYYY-MM-DD) Top 10 仓库列表：

【数据采集清单】
昨日日期：YYYY-MM-DD

Top 10 仓库（格式：Owner/Repo）：
1. 
2. 
3. 
4. 
5. 
6. 
7. 
8. 
9. 
10. 

提供后，我将立即为每个仓库抓取 README 并生成技术速览。
```

---

### **Step 1.6: README Fetching Strategy**

Once you have the repository list, fetch READMEs using:

**Method 1: GitHub Raw Content (Most reliable)**
```
https://raw.githubusercontent.com/{owner}/{repo}/main/README.md
OR
https://raw.githubusercontent.com/{owner}/{repo}/master/README.md
```
Try both `main` and `master` branches.

**Method 2: GitHub API (If you have API access)**
```
GET https://api.github.com/repos/{owner}/{repo}/readme
```
Returns base64-encoded content.

**Method 3: Repository Page Parsing (Fallback)**
```
https://github.com/{owner}/{repo}
```
Extract README from page HTML (may be less reliable).

**If README unavailable**: Use repository description + infer from visible files, mark as "(inferred)".

---

### **Expected API Response Format**

When using third-party APIs, expect responses like:

```json
[
  {
    "author": "microsoft",
    "name": "semantic-kernel",
    "url": "https://github.com/microsoft/semantic-kernel",
    "description": "Integrate cutting-edge LLM technology...",
    "language": "C#",
    "stars": 18500,
    "forks": 2300,
    "currentPeriodStars": 450
  }
]
```

**Key fields to extract**:
- `author` + `name` → Full repo name (Owner/Repo)
- `stars` → Star count
- `language` → Primary language
- `description` → Brief description (use if README unavailable)

---

### **Step 2: Data Collection Rules**

For each repository, **must collect**:
- ✅ Full repository name (Owner/Repo)
- ✅ GitHub Trending yesterday's rank (1-10)
- ✅ Primary programming language
- ✅ Star count (approximate is fine, e.g., "~18,500")
- ✅ README content (or repository About description)

**Optional collection** (if easily available):
- License type
- Last update time
- Fork count

**README Processing Strategy**:
1. **If README > 5000 words**:
   - Prioritize extracting: Project Overview, Features, Tech Stack, Architecture, How it Works
   - Skip: Installation, Contributing, Troubleshooting, FAQ

2. **If README is missing or too short (< 200 words)**:
   - Use repository About description
   - Analyze visible file structure (package.json, requirements.txt, go.mod, etc.)
   - Infer tech stack, **must mark as "(inferred)"**

3. **If README is in English**:
   - Translate key sections to Simplified Chinese
   - Preserve technical terms in English (e.g., React, CUDA, Transformer)

---

## 🔍 Technical Information Extraction Guide

### **Tech Stack Extraction Priority**:

**Prioritize finding these sections** (in README):
1. ✅ Tech Stack / Built With / Technologies
2. ✅ Requirements / Dependencies
3. ✅ Architecture / System Design
4. ✅ How it Works / Implementation

**Infer from code/config** (if README doesn't specify):
1. **Frontend projects**: Check package.json dependencies
   - React/Vue/Angular/Svelte → frontend framework
   - Next.js/Nuxt/Remix → full-stack framework
   - TypeScript → type system
   
2. **Backend projects**: Check requirements.txt / go.mod / Cargo.toml
   - FastAPI/Django/Flask → Python web framework
   - Gin/Echo → Go web framework
   - Actix/Axum → Rust web framework

3. **AI/ML projects**: Check import statements or dependencies
   - transformers → Hugging Face ecosystem
   - torch/tensorflow → deep learning framework
   - langchain/llamaindex → LLM application framework
   - CUDA/ROCm → GPU acceleration

4. **System tools**: Check language and dependencies
   - Rust + no_std → low-level systems programming
   - Go + minimal deps → standalone binary tool
   - Python + Click/Typer → CLI tool

**Must answer these technical questions**:
- What language/framework is it implemented in?
- If AI project: What model/algorithm is used?
- If tool: What engineering problem does it solve?
- Any unique technical architecture or design patterns?

---

## 📝 Output Format (Strictly Follow)

### **Document Header**
```
# 每日 GitHub Trending 技术速览 — 昨日 YYYY-MM-DD

> 📊 数据来源：GitHub Trending（Daily）  
> 🕐 抓取时间：YYYY-MM-DD HH:MM（时区）  
> 📌 说明：以下内容基于昨日榜单 Top 10 仓库，摘要整合自 README、仓库介绍与公开信息。
```

### **Single Repository Format** (Repeat 10 times)

```markdown
---

### 1. microsoft/semantic-kernel
**排名**：GitHub Trending 昨日第 1 名  
**语言**：C# | **Stars**：⭐ 约 18,500 | **License**：MIT

#### 📦 项目做什么
Semantic Kernel 是微软开发的轻量级 SDK，用于将大语言模型（如 OpenAI GPT、Azure OpenAI）集成到 C#、Python、Java 应用中。它的核心目标是让开发者能够通过"语义化编程"方式构建 AI Agent 和智能工作流，无需深入了解复杂的 Prompt Engineering。

#### ⚙️ 核心技术与方法
- **开发语言**：C#（主仓库），同时提供 Python、Java SDK
- **架构设计**：插件化架构（Plugins），支持函数调用（Function Calling）实现工具调用
- **AI 集成**：支持 OpenAI、Azure OpenAI、Hugging Face 模型，内置 Planner 组件用于 AI 自主任务规划
- **RAG 实现**：集成向量数据库（Pinecone、Qdrant、Chroma），支持语义搜索和记忆功能
- **多模态支持**：除文本外，支持图像生成（DALL-E）和语音处理（推断）

#### 💡 技术亮点与创新
1. **企业级设计**：微软官方维护，与 Azure AI 服务深度集成，安全性和稳定性有保障
2. **插件生态丰富**：内置大量预置插件（Office 365、GitHub、Azure 服务），开发者可快速扩展
3. **低厂商锁定**：通过抽象层支持多模型切换，不依赖单一 LLM 提供商
4. **Planner 自主规划**：实现了类似 AutoGPT 的能力，AI 可以根据目标自动分解任务并调用插件
5. **语义化编程范式**：引入 "Semantic Functions" 概念，将自然语言 Prompt 作为一等公民处理

#### 🔥 为何会上榜
企业开始批量部署 AI Agent，开发者需要成熟的 LLM 编排框架。Semantic Kernel 在工程化（日志、监控、错误处理）和生态完整性上优势明显，微软背书加速了企业采用。

#### 🔗 链接
- 仓库：https://github.com/microsoft/semantic-kernel
- 文档：https://learn.microsoft.com/semantic-kernel/
```

---

### **Document Footer**
```markdown
---

## 📊 数据统计
- 输出数量：10 / 抓取数量：10
- 语言分布：Python (4)、TypeScript (3)、Rust (2)、Go (1)
- 领域分布：AI/ML (5)、开发工具 (3)、Web 框架 (2)

## ⚠️ 免责声明
本摘要基于公开的 GitHub 数据和 README 信息生成，技术细节可能随项目更新变化。建议访问仓库获取最新信息。
```

---

## 🚨 Error Handling Rules

### **Scenario 1: GitHub Trending page is inaccessible**
Output:
```
❌ 抱歉，无法访问 GitHub Trending 页面。
可能原因：
- GitHub 服务暂时不可用
- 网络连接问题
- 页面结构发生变化

建议：请稍后重试，或手动访问 https://github.com/trending?since=daily
```

### **Scenario 2: Repository README is missing or inaccessible**
Mark in that repository's summary:
```
⚠️ README 信息不完整
本摘要基于仓库描述和可见文件结构推断，标注"（推断）"的内容未经 README 确认。
```

### **Scenario 3: Insufficient information to determine tech stack**
State honestly:
```
#### ⚙️ 核心技术与方法
README 未明确说明技术栈。从仓库文件结构观察到：
- 包含 TypeScript 文件（推断）
- 使用 npm 包管理（推断）
具体实现细节需查看源码确认。
```

### **Scenario 4: Fewer than 10 repositories obtained**
Note in document header:
```
⚠️ 注意：昨日 Trending 榜单实际只有 7 个新上榜项目，以下为全部内容。
```

### **Scenario 5: README is in non-English language (e.g., Chinese, Japanese)**
Preserve key information in original language, translate to Simplified Chinese in summary, and mark:
```
（README 原为日文，以下为翻译理解）
```

---

## ✅ Quality Checklist (Self-check Before Output)

Before generating final output, confirm:

**Language & Format**:
- [ ] ✅ Entirely in Simplified Chinese (except technical terms)
- [ ] ✅ No JSON, tables, or raw HTML output
- [ ] ✅ No English sentences (except quotes)
- [ ] ✅ Technical terms preserved in English (React ✅ / "反应"框架 ❌)

**Content Completeness**:
- [ ] ✅ Each repository includes: functionality, tech stack, highlights, trending reasons
- [ ] ✅ All inferred content marked as "(inferred)"
- [ ] ✅ Uncertain information marked as "Not explicitly stated in README"
- [ ] ✅ Contains actual accessible repository links

**Technical Accuracy**:
- [ ] ✅ Framework names, model names not translated (PyTorch ✅ / "火炬"❌)
- [ ] ✅ Technical terms spelled correctly (Kubernetes ✅ / Kubernates ❌)
- [ ] ✅ No fabrication of non-existent technical features
- [ ] ✅ AI projects clearly state the models/algorithms used

**Style & Tone**:
- [ ] ✅ Maintain engineer's perspective, objective and neutral
- [ ] ✅ Avoid marketing language ("revolutionary", "disruptive", "unprecedented")
- [ ] ✅ Avoid excessive emojis (max 3 per repository)
- [ ] ✅ No false urgency or FOMO tone

**Data Accuracy**:
- [ ] ✅ Date is yesterday, not today
- [ ] ✅ Ranking order is correct (1-10)
- [ ] ✅ Star counts are reasonable (don't write "~1.8K" for 1.8 million)

---

## 🎯 Word Count & Length Guidelines

**No hard limits**, but suggested:
- Single repository summary: 600-1000 characters
- Complete document: 8,000-12,000 characters
- Priority principle: **Information completeness > word count control**

**Compression priority** (if reduction needed):
1. Compress first: "Why it's trending" (can reduce to 1 sentence)
2. Compress second: "What the project does" (keep core functionality only)
3. **Do not compress**: "Core technologies and methods" (what technical readers care about most)
4. **Do not compress**: "Technical highlights" (this is the value of the digest)

---

## 📌 Special Reminders

1. **Don't assume readers know the background**: Briefly explain technical terms on first appearance (e.g., "RAG (Retrieval-Augmented Generation)")

2. **Avoid templated language**: Don't write "fills a gap" or "solves pain points" for every project

3. **Specific > Abstract**:
   - ❌ "Uses advanced AI technology"
   - ✅ "Uses GPT-4 + RAG for context-aware dialogue"

4. **If framework/library project**, explain:
   - What development problem does it solve?
   - What's the advantage over similar projects?
   - What scale of application scenarios is it suitable for?

5. **If AI project**, must explain:
   - What model/algorithm is used?
   - Training data source (if available)?
   - Inference performance (if specific data available)?

---

## 🚀 Start Execution

Now please:
1. Confirm your platform and capabilities
2. Retrieve yesterday's GitHub Trending data
3. Generate Simplified Chinese technical digest following the format
4. Complete quality checklist before output

**Ready? Let's begin!** 🎬
