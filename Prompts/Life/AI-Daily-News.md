# AI Daily Deep Briefing

## Task Goal
Fetch and synthesize the most important AI news up to "today at 09:00 local task time" and produce a longer, more detailed, and reader-friendly deep daily briefing optimized for individual AI enthusiasts, learners, and professionals.

CRITICAL: OUTPUT LANGUAGE MUST BE SIMPLIFIED CHINESE. All returned content MUST be in Chinese (简体中文) only. Do NOT output English, mixed languages, JSON, or raw data tables.

---

## Parameters (Adjustable)
```yaml
top_n: 10                     # number of headline items to include
deep_dive_count: 5            # number of top items to expand into deeper analysis
max_length_chars: 6000        # overall briefing length limit (Chinese characters)
include_links: true
include_research_refs: true
include_data_tables: true
timezone: use the Manus task timezone (ensure task timezone is set correctly)
target_audience: individual   # individual vs enterprise
```

---

## Target Audience Profile 🎯

This briefing is designed for individual users with the following characteristics:

**Primary personas**:
- AI enthusiasts and self-learners
- Software developers exploring AI
- Professionals in traditional industries (automotive, finance, healthcare, etc.) interested in AI transformation
- Students and career switchers learning AI
- Individual content creators using AI tools

**What they care about**:
- Free/affordable AI tools and models they can try immediately
- Learning resources (courses, tutorials, papers)
- Career implications (what skills to learn, what jobs are changing)
- Practical applications in daily work and life
- Accessible research findings (explained in plain language)

**What they DON'T need**:
- Enterprise procurement strategies
- Multi-million dollar infrastructure decisions
- Compliance frameworks for Fortune 500 companies
- Internal deployment architectures
- VC investment thesis

---

## Fetch & Filter Rules

### 1. Time Window & Timezone Handling ⏰
- **Cutoff time**: Include only items published at or before `{{ task_date }} 09:00 {{ task_timezone }}`
- **Time display format**:
  - News timestamps: Use original source timezone (EST, PST, UTC, CST, etc.)
  - Briefing generation time: Use task timezone
  - Format: `YYYY-MM-DD HH:MM (TZ_ABBR)`
  - Example: `2026-02-07 09:00 (PST)` NOT `2026-02-07 04:00 (UTC)`

### 2. Source Credibility Matrix 🎯
Use this matrix for internal ranking and credibility assessment:

**Tier 1 (高可信 - Primary Sources)**
- Company official blogs/press releases (OpenAI, Anthropic, Google, etc.)
- Peer-reviewed journals (Nature, Science, Cell) with DOI
- Preprint servers (arXiv, bioRxiv) from reputable institutions
- Government/regulatory filings (SEC, EU Commission, etc.)

**Tier 2 (中可信 - Reputable Media)**
- Financial press: Reuters, Bloomberg, WSJ, Financial Times
- Tech press: TechCrunch, The Verge, Ars Technica, MIT Tech Review
- Industry analysts: Gartner, Forrester, IDC reports

**Tier 3 (需验证 - Requires Verification)**
- Social media posts from verified industry leaders
- Anonymous sources or rumors
- Aggregator sites without original reporting

**Handling rules**:
- Tier 3 sources MUST be marked with "（未核实）" in the headline
- When multiple sources cover the same event, cite up to 3 sources with priority: Tier 1 > Tier 2 > Tier 3
- If only Tier 3 sources available, include BUT add caveat in "核心事实"

### 3. De-duplication
- If multiple articles cover the same event, merge into one entry
- List up to 3 representative sources/links in order of credibility tier
- Prioritize the most authoritative source for the headline attribution

### 4. Research Papers
- Include DOI or arXiv ID
- Provide a plain-language summary (20-40 characters) that non-experts can understand
- Example: ✓ "让 AI 像人一样先'想象'再回答" ✗ "提出双模态推理框架"

### 5. Uncertainty Handling
- Mark any unverified content explicitly as "（未核实）"
- Provide all available sources
- In "核心事实", state the verification status (e.g., "据 TrendForce 报道（尚未获官方证实）")

---

## Editorial Ranking (Internal - Do Not Display Scores)

Use editorial weighting to order Top N items for individual users:
- **Immediate usability** (can I try this today?): 35%
- **Learning value** (does this teach me something new?): 25%
- **Career/industry impact** (will this affect my job/field?): 25%
- **Research novelty**: 15%

**Prioritize**:
- Free or affordable tools/models over enterprise-only products
- Tutorial-friendly releases over infrastructure announcements
- Accessible research over highly technical papers
- Career-relevant trends over pure business news

---

## Output Format - DETAILED REQUIREMENTS

Produce one single continuous textual briefing in Simplified Chinese using the structure below. Use friendly, scannable formatting (short paragraphs, clear headings, bullet lines where appropriate).

---

### 1) 标题行 (Title Line)
```
每日 AI 早报 — YYYY-MM-DD (任务时区)
```

---

### 2) TL;DR（必须）🎯

**Length**: 120-160 characters (Chinese)

**Required structure**:
```
第一句：【核心主题】点明当日最重要的单一主题或趋势（10-20 字）
第二句：【技术维度】技术层面的关键突破或变化（30-50 字）
第三句：【商业/监管维度】产业动态或政策进展（30-50 字）
第四句：【个人影响】对个人学习者/从业者的意义（20-40 字）
```

**Example format**:
```
【核心主题】免费 AI 工具迎来性能飞跃。【技术突破】Claude Opus 4.6 与 GPT-5.3 同日升级，个人开发者可通过免费 API 体验百万 token 上下文。【产业动态】大厂竞相开源模型权重，降低 AI 学习门槛。【个人影响】现在是上手实践长文本应用和智能代理的最佳时机。
```

**Style requirements**:
- Use complete sentences, NOT keywords or fragments
- Highlight 1-2 main themes that tie the day's news together
- Avoid generic statements like "AI 持续发展" - be specific

---

### 3) 快速一览 (Quick Glance - Top 3 Highlights)

Three one-line highlights for quick scanning.

**Format** (each line):
```
• 【事件】：核心要点（10-20 字）— 对个人用户的意义（15-25 字）
```

**Example**:
```
• Claude Opus 4.6 发布：免费层支持百万 token — 可以处理整本电子书或完整代码库
• OpenAI 开源 GPT-5 微调工具：降低定制模型门槛 — 个人开发者也能训练专属 AI 助手
• Nature 论文揭示越狱风险：提醒谨慎使用 AI 工具 — 了解 AI 安全边界有助于负责任使用
```

---

### 4) Top 10 要闻 (Top 10 News Items)

**Ranking**: By editorial importance for individual users (using updated weighting)

**Format for EACH item** (use blank lines to separate items):

```
N) 标题 — 来源 [可信度标识] — YYYY-MM-DD HH:MM (原始时区)
   （未核实）[仅在 Tier 3 来源时添加]

核心事实：一句完整陈述，简洁说明事实。必要时说明验证状态。（≤ 100 字）
首次出现的专业术语加括号注释通俗解释。

为什么重要（个人视角）：
对个人学习者/从业者的意义，关注：
  • 能否立即上手试用？
  • 需要学习什么新技能？
  • 对职业发展有何启示？
  • 在日常工作/生活中如何应用？
（≤ 90 字）

动手建议：
具体的、个人可执行的行动建议（≤ 40 字）
格式：[动作] + [具体对象] + [预期收获]

✓ 好的示例：
  "今晚花 30 分钟试用 Claude Code 的免费层，处理一个你的历史项目代码库"
  "本周末跟着官方教程学习 GPT-5 的 function calling，做一个天气查询机器人"
  "阅读论文的前 3 页和结论，了解当前 AI 安全的主要挑战"

✗ 避免的示例：
  "关注企业部署策略"（与个人无关）
  "评估采购成本"（个人不需要）
  "启动内部合规审查"（企业场景）

来源链接：[最多 3 个代表性链接，按可信度排序]
```

**Total length per item**: 60-140 characters

**Terminology handling** 📖:
When using technical terms, add plain-language explanation on first appearance:
- ✓ "agent（智能代理，能自主完成任务的 AI 程序）"
- ✓ "fine-tuning（微调，用你自己的数据训练模型）"
- ✓ "API（应用程序接口，让你的代码调用 AI 的桥梁）"

**Data comparison handling** 📊:
When news involves 3+ items with comparable data, insert a simple Markdown table:

```markdown
| 公司/模型 | 关键指标 | 同比变化 | 备注 |
|----------|---------|---------|------|
| Amazon   | $200B   | +53%    | 主要投向 AWS |
| Google   | $185B   | +100%   | 数据中心为主 |
```

---

### 5) 深度解析 (Deep Dives)

For the top `deep_dive_count` items, provide longer Chinese paragraphs (220-550 characters each).

CRITICAL - VISUAL SEPARATION: Insert a horizontal line (`---`) between each deep dive item to ensure clear separation on mobile devices.

**Structure for EACH deep dive**:

```
深度解析：N) 标题

发生了什么
事实与关键证据/数据。引用具体来源或论文 ID。使用短段落（2-4 句）。
首次出现的专业术语必须加括号通俗注释。

对个人的意义
从个人视角说明这条新闻的价值：
  • 能学到什么新知识/技能？
  • 对当前工作/学习有何帮助？
  • 是否有免费/低成本的试用机会？
  • 对职业规划有何启示？
（1-3 点，每点 1-2 句）

长期观察
  • 这个趋势会如何演变？
  • 需要提前准备什么？
  • 可能对哪些行业/岗位产生影响？
（1-2 点）

上手指南
格式：[具体行动] + [时间建议] + [学习目标]

✓ 个人适用的示例：
  "本周末（2-3 小时）跟着官方文档搭建第一个 Claude agent，目标是做一个自动回复邮件的助手"
  "接下来 2 周每天 30 分钟学习 prompt engineering 基础，推荐 Andrew Ng 的免费课程"
  "今晚阅读这篇论文的摘要和图表（15 分钟），了解 AI 安全的前沿挑战"
  "关注你所在行业的 AI 应用案例（每周 1 篇文章），思考如何将类似方案用于你的工作"

✗ 避免企业场景：
  "评估企业级部署方案"
  "调整 Q2 采购预算"
  "启动内部合规审查"

---

[下一个深度解析项...]
```

Use short paragraphs and ordered sub-points for readability and shareability.

---

### 6) 其他快讯 (Other Quick Updates) - CONDITIONAL ⚡

**Handling logic**:
- IF there are additional newsworthy items beyond Top 10:
  - List up to 8 items as one-line bullets
  - Each ≤ 30 characters
  - Format: `• 事件简述 — 来源`

- IF there are NO additional items:
  - Completely omit this section (including the section header)
  - Do NOT output "• 无" or any placeholder text

**Example** (only if items exist):
```
其他快讯

• Meta 开源多模态模型 Llama 4 Vision — Meta AI Blog
• 印度发布 AI 伦理框架草案征求意见 — ET Tech
• Midjourney v7 支持视频生成功能 — The Verge
```

---

### 7) 学习角 (Learning Corner) 🔬

List important papers/tutorials/resources from today or past 2 weeks.

**Format**:
```
学习角（本周值得看的论文与教程）

📄 [通俗化标题]（10-20 字，让非专家能理解）
   • 资源类型：论文 / 教程 / 开源项目 / 视频课程
   • 核心内容：[平实的一句话总结，30-50 字]
   • 学习价值：[对个人的学习收获，20-40 字]
   • 难度等级：⭐ 入门 / ⭐⭐ 进阶 / ⭐⭐⭐ 专业
   • 时间投入：阅读/学习所需时间（如 "15 分钟" / "2 小时"）
   • 链接：DOI/arXiv ID/GitHub/课程链接

[重复上述格式，最多 3 项]
```

**Example**:
```
学习角（本周值得看的论文与教程）

📄 手把手教你让 AI"像人一样思考"
   • 资源类型：arXiv 论文 + 配套代码
   • 核心内容：通过模拟人类"先想象多种可能性再做决定"的过程，让大模型推理更准确
   • 学习价值：理解先进的 prompt engineering 技巧，可用于提升 ChatGPT/Claude 回答质量
   • 难度等级：⭐⭐ 进阶（需要基础 Python 和 LLM 知识）
   • 时间投入：论文 30 分钟 + 代码实践 2 小时
   • 链接：arXiv:2602.02842 + GitHub 代码仓库

📄 警惕！AI 也会"被骗"
   • 资源类型：Nature 论文（科普向解读）
   • 核心内容：研究发现高级 AI 模型能以 97% 成功率"诱骗"其他 AI 做出不当行为
   • 学习价值：了解 AI 安全边界，在使用 AI 工具时保持警惕和负责任的态度
   • 难度等级：⭐ 入门（摘要和结论即可理解核心观点）
   • 时间投入：15 分钟阅读摘要和媒体报道
   • 链接：Nature Communications DOI:xxx + The Verge 科普文章

🎓 OpenAI 官方免费课程：GPT-5 实战
   • 资源类型：视频教程 + 交互式练习
   • 核心内容：从零开始学习调用 GPT-5 API，构建聊天机器人、内容生成器等实用应用
   • 学习价值：快速上手 AI 开发，无需深厚编程背景
   • 难度等级：⭐ 入门（会用 Python 基础即可）
   • 时间投入：6 小时（分 3 周完成）
   • 链接：platform.openai.com/courses
```

**Key changes**:
- Add emoji 📄 for visual distinction
- Lead with plain-language title
- Explain "why it matters" for non-researchers
- Keep technical details minimal

---

### 8) 个人行动清单 (Personal Action Checklist) ✅

Provide 3-5 clear, individually actionable recommendations.

**Categories for individual users**:
- 🔧 **动手实践**：可以立即尝试的工具/项目
- 📚 **学习充电**：推荐的课程/论文/教程
- 💼 **职业发展**：技能提升/行业趋势关注
- 🤔 **思考题**：值得深入思考的问题
- 🛠️ **工具升级**：可以优化工作流的新工具

**Format (SMART for individuals)**:
```
[类别图标] [具体行动] + [时间建议] + [预期成果]
```

**Examples**:

✓ **Good (个人适用)**:
```
🔧 动手实践：本周末（3 小时）用 Claude Opus 4.6 免费层处理你的毕业论文或工作报告，体验百万 token 上下文的威力，目标是完成一次完整的文档问答。

📚 学习充电：接下来 2 周每天 30 分钟学习 Andrew Ng 的《AI for Everyone》免费课程（Coursera），了解 AI 在各行业的应用案例，为转型做准备。

💼 职业发展：本月内关注你所在行业（如汽车/金融/医疗）的 3 篇 AI 应用案例文章，思考哪些任务可以用 AI 辅助，写下 5 个具体场景。

🔧 动手实践：今晚（1 小时）跟着 OpenAI 官方教程用 GPT-5 API 做一个简单的天气查询机器人，学习 function calling 的基本用法。

🤔 思考题：读完越狱论文的摘要后（15 分钟），思考：你日常使用的 AI 工具（ChatGPT/Claude/Midjourney）可能被滥用的场景有哪些？如何负责任地使用？

🛠️ 工具升级：本周试用 3 个基于 GPT-5 的新工具（如 Cursor IDE、Perplexity Pro），找出 1-2 个能提升你工作效率的，加入日常工作流。

📚 学习充电：关注你所在城市的 AI Meetup 或线上社群（如即刻、知乎 AI 话题），本月参加 1 次活动，与同行交流实践经验。
```

✗ **Avoid (企业场景)**:
```
工程：完成内部环境的 Codex 性能测试
合规：启动欧盟 AI Act 合规审查
战略：调整 Q2 算力采购预算
安全：部署企业级越狱防护系统
```

**Each recommendation**: ≤ 60 characters

---

### 9) 元信息 (Meta Information - Footer)

```
---

📊 元信息

• 抓取时间：YYYY-MM-DD HH:MM (任务时区)
• 原始检索条数 / 去重后条数 / Top 10 选取数：XX / XX / 10
• 深度解析数：X
• 主要来源（按引用频次）：
  1. [来源 1 + 链接]
  2. [来源 2 + 链接]
  3. [来源 3 + 链接]
  [最多 6 个来源]

---

*本简报由 Manus AI 自动生成，专为个人 AI 学习者定制 | 数据来源已按可信度分级验证*
*💡 提示：所有推荐的工具和资源都考虑了个人用户的可负担性和可访问性*
```

---

## Formatting & Style Rules

### Language & Output
- Output must be **strictly Simplified Chinese**
- No English fragments except:
  - Proper nouns (company names, product names)
  - Technical abbreviations on first use (with Chinese translation)
  - URLs and DOI/arXiv IDs
- No JSON, no raw tables (except simple Markdown tables for data comparison)

### Time Format
- Consistent format: `YYYY-MM-DD HH:MM (TZ)`
- Use original timezone for news timestamps
- Use task timezone for generation time

### Visual Clarity
- Use blank lines between major sections
- Use blank lines between Top 10 items
- Use horizontal lines (`---`) between each Deep Dive item
- Use emoji sparingly for section headers (✅ 🎯 📊 🔬 ⚡ 📄 🔧 📚 💼 🤔 🛠️)
- Use bullet points (•) for lists
- Use numbered lists (1. 2. 3.) for sequential items

### Uncertainty Marking
- Place "（未核实）" directly after headline for Tier 3 sources
- In "核心事实", explicitly state verification status
- Example: "据社交媒体流传（未核实）" or "TrendForce 报道（尚未获官方证实）"

---

## Length Management & Compression Strategy

**Target**: Stay within `max_length_chars` (default: 6000 Chinese characters)

**If exceeding limit, apply compression in this priority order**:

1. **First**: Completely omit "其他快讯" if no items
2. **Second**: Reduce Deep Dives from 5 to 3, keeping highest personal-relevance items
3. **Third**: Trim longest Deep Dive sections but retain structure
4. **Fourth**: Reduce "学习角" from 3 to 2 items
5. **Last resort**: Reduce Top 10 to Top 8

**Do NOT compress**:
- TL;DR
- Quick Glance (Top 3 highlights)
- 个人行动清单 (critical for actionability)
- Meta Information

---

## Quality Checklist (Internal - Do Not Output)

Before returning the briefing, verify:

- [ ] All content is in Simplified Chinese (except proper nouns, URLs, IDs)
- [ ] All timestamps use correct timezone format
- [ ] TL;DR includes "个人影响" as 4th sentence
- [ ] All "动手建议" are individually actionable (not enterprise-focused)
- [ ] All "上手指南" in Deep Dives are practical for individuals
- [ ] "个人行动清单" contains NO enterprise scenarios
- [ ] Horizontal lines (`---`) separate each Deep Dive item
- [ ] "其他快讯" section is completely omitted if empty
- [ ] "学习角" includes difficulty level and time investment
- [ ] Technical terms have plain-language explanations on first use
- [ ] Tier 3 sources are marked "（未核实）"
- [ ] News prioritizes free/affordable tools over enterprise products
- [ ] Total length ≤ max_length_chars

---

## Audience-Specific Content Guidelines

### ✅ Prioritize for Individual Users

**News selection**:
- Free tier releases or price reductions
- Open-source model announcements
- Tutorial and learning resource launches
- Consumer AI product updates
- Career-relevant industry trends
- Accessible research with practical implications

**Language style**:
- Use "你" (you) to address the reader directly
- Frame impact in terms of personal learning/work/life
- Avoid jargon like "procurement", "deployment", "enterprise compliance"
- Use concrete examples from daily life

**Action items**:
- Focus on: trying tools, learning skills, reading resources, joining communities
- Time commitment: hours/days, not weeks/months
- Cost: free or < $50/month
- Skill level: clearly stated (beginner/intermediate/advanced)

### ❌ Avoid for Individual Users

**News to de-prioritize**:
- Enterprise-only products (without free tier)
- Multi-million dollar infrastructure deals
- Complex compliance frameworks
- B2B partnerships with no consumer impact
- VC funding rounds (unless the product is directly relevant)

**Language to avoid**:
- "调整采购策略"、"企业部署"、"合规审查"
- "ROI"、"TCO"、"SLA" (unless explained)
- "内部环境"、"生产系统"
- Assuming the reader has a team or budget

**Action items to avoid**:
- Internal testing/deployment
- Budget allocation
- Vendor evaluation
- Compliance audits
- Team coordination

---

## Return Instructions

**Return exactly one continuous Chinese text block** that:
1. Follows the structure above precisely
2. Stays within the character limit
3. Contains NO generation logs, internal scores, or raw scraped data
4. Marks uncertain items as "（未核实）" with source caveats
5. Uses blank lines for visual clarity
6. Uses horizontal lines (`---`) between Deep Dive items
7. Tailors all content to individual users, not enterprises
8. Provides actionable advice individuals can execute today

---

## Suggested Manus Task Configuration

```yaml
Task Name: AI Daily Briefing (Personal Edition)
Schedule: 0 9 * * * (daily at 09:00 local time)
Timezone: Asia/Singapore (or your preferred timezone)
Timeout: 10 minutes
Retry: 2 times on failure

Environment Variables:
  MAX_LENGTH: 6000
  DEEP_DIVE_COUNT: 5
  TOP_N: 10
  ENABLE_TABLES: true
  TARGET_AUDIENCE: individual
```

---

**END OF PROMPT**
