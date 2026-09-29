# feishu-lark-skills

飞书 / Lark 的 Agent Skill **入口层**。一层皮：把请求路由到官方 skill，然后让官方 skill 带你执行。

## 安装

**前提：`lark-cli` ≥ 1.0.97**（只支持该版本及以上，不做老版本兼容）。

```bash
npm install -g @larksuite/cli@latest     # 前提，确保安装了最新的 lark-cli
npx skills add Thearas/feishu-lark-skills -y -g
```

## 原理

本 skill 只做一件事：查路由表拿到目标 skill 名，再 `lark-cli skills read <skill>` 读官方内容执行。**不需要把官方 skill 安装到本地**——如果你已经用别的方式装过，它们只是多一份干扰的副本。
