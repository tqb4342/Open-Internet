# AI 编程工具选型：Claude Code、OpenCode 与三种形态

> 本文整理自 [tanqingbo.cn](https://tanqingbo.cn/) 的 AI 编程工具选型指南，去营销化技术版。工具的模型调用大多依赖海外线路（Claude / OpenAI 系），网络前提见本仓库 [科学上网完全指南](科学上网完全指南.md)；ChatGPT / Gemini 的基础注册使用见 [ChatGPT国内注册与使用](ChatGPT国内注册与使用.md) 与 [Gemini国内使用](Gemini国内使用.md)。

---

2026 年的 AI 编程，「代码补全」已经是上一个时代的东西。现在的 Agent 自己开终端、装依赖、跑测试、改 bug，一条指令丢过去，回来一个能跑的项目——写代码从「逐行敲」变成了「指挥干活」。

但工具也多到眼花：Claude Code、OpenCode、Codex、豆包 Agent、Hermes、Harness、rmux……每篇教程都只讲自己那个，没人说清它们互相什么关系、你该从哪个下手。这篇就是选型笔记：什么形态、谁免费、钱花在哪、网络怎么解决。

## 30 秒速答

- **形态**：AI 编程工具分三种——终端 CLI Agent（专业主力）、桌面/云端 Agent（不开终端也能干活）、知识库自生长（让 AI 管理你的笔记）
- **零成本入门**：豆包 Agent 免费用、手机遥控电脑；Kimi K3 免费额度 + Kimi Code 写代码
- **专业主力**：Claude Code 体验最好但要订阅；OpenCode 开源免费、接便宜 API，是国产模型党的主流选
- **多 Agent 进阶**：rmux + Hermes + Harness，让一个 Agent 指挥一群 Agent
- **网络前提**：国产服务直连能用，Claude/OpenAI 系要稳定出海线路，没解决的先看本仓库 [科学上网完全指南](科学上网完全指南.md)

## 全景一览

| 工具 | 形态 | 免费吗 | 适合谁 |
|---|---|---|---|
| Claude Code | 终端 CLI | 订阅制 | 想要最完整体验的开发者 |
| OpenCode | 终端 CLI | 开源免费 | 想接国产 API 省钱的人 |
| GPT-6 Astra Codex | 云端 + CLI | 订阅制 | 追前沿模型能力的开发者 |
| 豆包 Agent | 桌面 + 云电脑 | 免费额度 | 不写代码但要电脑干活的人 |
| 小米 MiMo Desktop | 桌面 | 免费邀测 | 想白嫖「小米版 Codex」的人 |
| Kimi Code（K3.1 Pro） | 终端 CLI | 免费额度 | 零成本入门写代码 |
| DeepSeek Harness | Agent 运行时 | 开源免费 | 爱折腾插件化的玩家 |
| Hermes Agent | 终端 CLI | 开源免费 | 要在多家模型间切换的人 |
| rmux | 终端复用器 | 开源免费 | 搞多 Agent 协作的进阶玩家 |
| LLM Wiki | 知识库 | 开源免费 | 想让笔记自己进化的人 |

表先放这。往下把三种形态讲清，你回头对着表选，基本一眼就能锁定。

## 三种形态：先搞清你要的是哪类

### 终端 CLI：Agent 的主战场

真正的 AI 编程主力都在终端里。Claude Code 是 Anthropic 官方出品，工程质量、上下文管理、权限模式都是标杆，缺点是订阅贵（$20/月起）且国内访问需要稳定线路——好在它支持通过配置接入国产大模型降成本。

OpenCode 是开源免费的那一类：支持自定义 API、Copilot 账号登录、VS Code 和 Cursor 集成，想用 DeepSeek 这种便宜模型自己拼方案的，它是主流选择。接自定义 API 的报错九成出在 baseURL 和模型名这两个字段。

再往上还有一层进阶玩法：rmux 是专为 Agent 打造的终端复用器，能让 Claude Code 当总指挥、向 Codex 和 Gemini CLI 指派任务；Hermes 解决的是多家模型来回切换的配置问题；DeepSeek Harness 干脆把模型、工具、界面全部插件化，随换随用。这三个适合已经在终端里跑 Agent 的人，新手先跳过。

### 桌面和云端 Agent：不开终端也能干活

不是所有人都写代码。豆包 Agent（工作任务模式）已经能让你在地铁上用手机遥控家里的电脑：点按钮、填表格、做表格、剪视频，全程可视。两个细节提前知道：Turbo 模式够日常用，Pro 模式额度消耗快；GUI 操控目前 Windows 全量、Mac 只能操控浏览器。

小米 MiMo Desktop 是「小米版 Codex」，免费邀测中，一张参考图自动调 Blender 建模 90 多个零件的键盘这种活它真干得出来。

想看上限，GPT-6 Astra 两条线都值得看：Codex 18 分钟生成 3D 程序化城市、19 分钟做全栈应用；Computer Use 则是 AI 自己看屏幕、动鼠标、敲键盘，填表单、更新 CRM 全程不喊人。

### 知识库自生长：让笔记自己进化

LLM Wiki 是另一条路：不是帮你写代码，而是把散落的原始资料（Raw）自动整理成结构化知识（Wiki），接上 Obsidian 就是一套会进化的第二大脑。和 RAG 的本质区别：RAG 是检索时临时拼上下文，Wiki 是提前把知识沉淀成结构。

## 按需求选：四条路线

**路线一：零成本试水**。先豆包 Agent——不用配任何 API，手机装 App、电脑装客户端就能用，额度免费；想试写代码，Kimi K3 免费额度加 Kimi Code。先跑通一个真实小任务再谈投入，免得订阅买了吃灰。

**路线二：省钱主力开发**。OpenCode 接 DeepSeek API，成本压到每月几块钱；要图形界面就集成进 VS Code 或 Cursor。对模型有执念的，用 Hermes 在 DeepSeek、Gemini、Kimi 之间随时切换。这条路线全程国内直连，不用碰翻墙问题。

**路线三：上官方完整体验**。Claude Code 订阅（或 GPT-6 Astra），体验上限最高，代价是月费加稳定的出海线路。账号怎么买便宜不用外币卡，见 [AI大模型购买指南](AI大模型购买指南.md)。

**路线四：多 Agent 协作**。已经在用 Claude Code 的人，加 rmux 做终端编排、Harness 做插件运行时，让一个 Agent 指挥一群 Agent 干活——适合有明确多任务并行需求的进阶玩家。

## 模型和账号从哪来

工具本身大多免费，真正花钱的是背后的模型。模型从哪来，直接决定你每个月的账单长什么样：

- **国产 API 直连**：DeepSeek、Kimi 官方 API 便宜且国内直连，OpenCode、Hermes、Harness 都支持，开发党性价比首选
- **合租/镜像账号**：ChatGPT Plus、Claude 这些海外订阅，官方要 $20/月加外币卡；合租拼车价低得多，镜像站连翻墙都省了，方案对比见 [AI大模型购买指南](AI大模型购买指南.md)
- **聚合中转**：OpenRouter 一个 key 调多家模型，适合对比党，走 Hermes 配置最顺

## 工具之前，先解决网络

把表拉一遍你会发现：国产系（豆包、Kimi、DeepSeek）国内直连就能用，但 Claude Code 官方安装源、OpenAI Codex、Gemini CLI 这些海外服务，从下载到调用 API 全程都要稳定的出海线路——国内直连要么超时要么被墙，线路不稳的时候 Agent 跑一半断线，比不跑还难受。

网络怎么解决，按你的情况对号入座：

- **从零开始**：本仓库 [科学上网完全指南](科学上网完全指南.md)，选方案、装客户端、导入订阅一条龙
- **只要 Windows**：[Windows翻墙三方案对比](Windows翻墙三方案对比.md)
- **主要用手机**：[手机科学上网教程](手机科学上网教程.md)

国产直连路线（路线一、二）可以跳过这节；要上官方体验（路线三）和多 Agent 协作（路线四），这关绕不过去。

## 避坑清单

1. **别一上来就买订阅**。先用免费路线跑通一个真实任务，确认自己的使用频率，再决定要不要为 Claude Code 付费——多数人的使用强度，免费额度就够了
2. **模型配置看清 baseURL 和模型名**。OpenCode 这类工具接自定义 API 的报错，九成出在这两个字段
3. **网络不稳别跑长任务**。Agent 任务一跑几十分钟，中途断线前功尽弃，海外线路先测稳再开工
4. **镜像站和官方账号别混淆**。镜像站便宜免翻墙，但对话记录不绑定你的账号；重度使用建议正经账号
5. **Windows 装 Claude Code 先备好 Git Bash**。它原生为 Linux/macOS 设计
6. **额度比你想的烧得快**。Agent 是按 token 计费的干活模式，跑重活（建模、长动画、全栈生成）一次任务就能吃掉不少额度——跑批量任务前先看计费看板

## 常见问题

### 零基础该从哪个 AI 编程工具入手？

先用免费的。豆包 Agent 在手机上就能遥控电脑干活，不用配任何 API；想正经写代码，Kimi K3 免费额度加 Kimi Code，或者 OpenCode 接国产 API（DeepSeek 便宜），都是零成本起步路线。跑通一个真实小项目，再考虑上 Claude Code 这类付费方案。

### Claude Code 和 OpenCode 哪个好？

Claude Code 是官方出品、工程质量高，但订阅贵且国内访问要稳定线路；OpenCode 开源免费、支持自定义 API 和 Copilot 账号登录，适合想用便宜模型自己拼方案的人。预算充足追求体验选前者，想省钱爱折腾选后者，两者不冲突，先跑通一个再说。

### 用这些工具需要翻墙吗？

分情况。豆包、Kimi、DeepSeek 系国产服务国内直连就能用；Claude Code 官方源、OpenAI Codex、Gemini CLI 这些海外服务，下载安装和调用 API 都需要稳定的出海线路，国内直连要么超时要么被墙。网络没解决的，从本仓库 [科学上网完全指南](科学上网完全指南.md) 起步配置，再回来装工具。

### 模型 API 从哪里弄便宜？

三条路：一是国产 API（DeepSeek、Kimi），官方直连便宜，OpenCode、Hermes、Harness 都支持；二是合租平台买 ChatGPT、Claude 账号或镜像站，不用外币卡；三是 OpenRouter 这种聚合中转，一个 key 调多家模型。方案对比见 [AI大模型购买指南](AI大模型购买指南.md)。

### AI 编程工具会取代程序员吗？

短期不会取代，但会拉开差距。现在的 Agent 能独立完成模块级任务（写页面、跑测试、改 bug），但架构设计、需求拆解、技术选型还是要人拍板。现实是：会用 Agent 的人一天干出三天的量，不会用的人迟早被会用的人卷，工具学不学，答案是早学晚学的问题。

### rmux、Hermes、Harness 这些是干嘛的？

都是围绕 Agent 的进阶工具。rmux 是专为 Agent 打造的终端复用器，让 Claude Code 指挥 Codex、Gemini CLI 干活；Hermes Agent 解决多模型切换配置问题；DeepSeek Harness 是插件化 Agent 运行时，模型、工具、界面全都能换。它们适合已经在用 Agent、想搞多 Agent 协作的人，新手先跳过。

## 总结

选来选去其实就三个问题：你习惯在哪干活（终端还是桌面）、愿意花多少钱（国产 API 还是海外账号）、网络什么条件（直连够用还是得出海）。这三个答案一定，表格里该选哪个自己就跳出来了。零基础从豆包和 Kimi 免费起步，开发党走 OpenCode 加 DeepSeek 的省钱路线，追求上限的上 Claude Code 和 GPT-6 Astra。

> 各工具的完整安装配置教程（Claude Code / OpenCode / 豆包 Agent / MiMo / Hermes / Harness / rmux 实测图文）见博客：[AI 编程工具怎么选（2026）](https://tanqingbo.cn/ai-coding-agent-guide/)
