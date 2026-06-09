# 接续对话 — QwenPaw 跨会话记忆桥接

Created by 一懒众衫小 — 随便用，记得留个名就行

## 这是什么

QwenPaw 的对话 session 文件天然存储了全部历史对话原文和摘要。本 skill 通过 `chats.json` 索引 + 子 agent 提炼结构化摘要，实现跨会话无缝接续。**不装任何插件，不依赖第三方工具。**

## 安装

将 `skills/接续对话.md` 放到 QwenPaw workspace 的 `skills/` 目录下：

```
cp skills/接续对话.md $HOME/.qwenpaw/workspaces/default/skills/
```

## 使用

在对话中输入「**接续对话**」，按提示操作即可。

## 原理

```
用户输入「接续对话」
    ↓
列出所有历史对话（chats.json）
    ↓
用户指定要续接的对话
    ↓
子 agent 读取 session JSON 文件
    ↓
提炼六项结构化摘要（基本信息/主要话题/关键结论/待办事项/上次聊到哪/文件路径）
    ↓
加载摘要到当前上下文
    ↓
无缝续接 ✅
```

## 依赖

- QwenPaw（任意版本）
- 无需安装任何额外工具
