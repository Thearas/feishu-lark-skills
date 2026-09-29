---
name: feishu-lark-skills
version: 1.0.0
description: "用 lark-cli 操作飞书 / Lark 的唯一入口：消息、云文档、云空间、电子表格、多维表格（Base）、知识库、画板、幻灯片、邮件、日历、会议与妙记、任务、审批、考勤、OKR、通讯录、实时事件、妙搭应用开发。Use when the user mentions 飞书/Feishu/Lark or any of those areas, including feishu.cn / larksuite.com / doubao.com links carrying Feishu resources. 用户已给出该域的指令时不要用它替代对应 skill 的内容，本入口只做飞书/Lark 的路由，需 OpenAI/Notion 等其他 SaaS 时不要用本技能。"
metadata:
  category: "productivity"
  requires:
    bins: ["lark-cli"]
---

# 飞书 / Lark 入口

本入口**只做一件事**：把你的请求路由到官方 skill，然后让官方 skill 带你执行。它不复制任何用法、参数或流程——那些一律以官方内容为准，且官方内容与当前 CLI 版本严格对应。

**前提：安装 `lark-cli` ≥ 1.0.97（命令是 `npm install -g @larksuite/cli@latest`）。** 
本入口的路由表只记录到该版本的 `lark-cli skills read|list`。

## 怎么用（三步）

1. **查下面的路由表，拿到目标 skill 名。** 表里没有的，用 `lark-cli skills read` 搭配 jq/grep 命令从官方列表里筛一个出来，别整份 dump
2. **读该 skill 的官方内容**：`lark-cli skills read <skill>`。它自己的 SKILL.md 会告诉你接下来读哪个 `references/<file>.md`（用 `lark-cli skills read <skill> <path>` 读，同样支持子目录），**搭配 rg/grep 过滤输出**，减少 token 消耗。
3. **按官方内容执行。** 参数一律以 `--help` 或官方内容为准，不要凭本文件猜。

**只有 `lark-cli` 是前提**：不需要另外安装任何官方 skill 到本地。

## 路由表

| 场景关键词 | skill |
|---|---|
| 消息、群聊、聊天记录、群成员、发消息 | `lark-im` |
| 云文档、docx、写/读文档正文、思维笔记 | `lark-doc` |
| 云盘、云空间、文件夹、上传下载、文件权限、云空间搜索 | `lark-drive` |
| 电子表格、单元格、公式、图表、Excel/xlsx/csv | `lark-sheets` |
| 多维表格、Base、bitable、数据表、字段、视图、仪表盘 | `lark-base` |
| 知识库、知识空间、节点层级、空间成员 | `lark-wiki` |
| `/wiki/` 链接（背后类型不确定：可能是文档、表格、Base、幻灯片） | 先用 `lark-cli wiki +node-get` 换成真实类型与 token，再按结果落到对应 skill——直接把 wiki token 当 file_token 用是最常见的失败 |
| 画板、架构图、流程图、思维导图 | `lark-whiteboard` |
| 幻灯片、演示文稿、PPT | `lark-slides` |
| 原生 Markdown 文件、.md、patch/diff | `lark-markdown` |
| 日历、日程、参会人、忙闲、会议室 | `lark-calendar` |
| 已有会议记录、纪要、妙记、逐字稿、机器人入会 | `lark-meeting` |
| 任务、待办、清单、子任务、任务协作 | `lark-task` |
| 审批待办/实例、发起审批 | `lark-approval` |
| 邮件、邮箱、收件箱、草稿、附件 | `lark-mail` |
| 通讯录、联系人、姓名/邮箱 ↔ open_id | `lark-contact` |
| 考勤、打卡 | `lark-attendance` |
| OKR、目标、关键结果、对齐 | `lark-okr` |
| 实时事件、长连接订阅、NDJSON 流 | `lark-event` |
| 妙搭、应用开发与部署、创意设计、线上日志 | `lark-apps` |
| 配置、授权登录、身份、scope、权限不足 | `lark-shared` |
| 需求超出以上能力、要调原生 OpenAPI | `lark-openapi-explorer` |
| 把某组操作封装成自定义 skill | `lark-skill-maker` |
| 会议纪要汇总成报告 | `lark-workflow-meeting-summary` |
| 日程 + 待办摘要 | `lark-workflow-standup-report` |

## 跨域链路

- 发消息给人：`lark-contact` 取 open_id → `lark-im` 发送
- 会议要产物：`lark-calendar` 取 meeting_id → `lark-meeting` 取纪要/逐字稿
- 导出表格或 Base：先在 `lark-sheets`/`lark-base` 处理数据 → `lark-drive` 导出下载
- 文档里的画板：`lark-doc` 取出画板 token → `lark-whiteboard` 编辑
