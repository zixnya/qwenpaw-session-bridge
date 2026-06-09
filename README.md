# QwenPaw 零依赖跨会话接续

Created by 一懒众衫小 — 随便用，记得留个名就行

## 这是什么

QwenPaw 的对话 session 文件天然存储了全部历史对话原文和摘要。本 skill 通过 `chats.json` 索引 + 子 agent 提炼结构化摘要，实现跨会话无缝接续。**不装任何插件，不依赖第三方工具。**

支持：控制台、微信、QQ 等多通道对话的跨会话接续。

## 安装

**方法一：直接复制**

将 `skills/接续对话.md` 放到 QwenPaw workspace 的 `skills/` 目录下：

```bash
# Linux/macOS
cp skills/接续对话.md $HOME/.qwenpaw/workspaces/default/skills/

# Windows PowerShell
copy skills\接续对话.md %USERPROFILE%\.qwenpaw\workspaces\default\skills\
```

**方法二：通过 QwenPaw 注册（推荐）**

将文件放入 `skills/` 目录后，更新 `skill.json` 中的注册时间戳，或在对话中让 QwenPaw 执行：

> 把这个 skill 注册一下：`skills/接续对话.md`

## 使用

在对话中输入「**接续对话**」，按提示操作即可。

支持指定时间范围，如「接续最近3天的」。

## 原理

```
用户输入「接续对话」
    ↓
列出所有历史对话（chats.json）
    ↓
用户指定要续接的对话
    ↓
定位 session JSON 文件（控制台/微信/QQ 自动识别）
    ↓
派分身（spawn_subagent）读取文件
    ├─ 分身指令包含精确文件结构指引，不用自己猜
    ├─ 出错 → 自动重试一次
    └─ 重试失败 → 汇报用户
    ↓
分身返回六项结构化摘要
    ↓
清理分身记录（Python 脚本操作 chats.json，禁止文本替换）
    ↓
加载摘要到当前上下文
    ↓
无缝接续 ✅
```

## V2 更新内容

| 改动 | 说明 |
|:----|:------|
| **文件结构指引** | 分身指令写死 JSON 路径 `data["agent"]["memory"]["content"]` 和字段说明，不再让分身自己猜 |
| **提取规则精确化** | user→取 text、assistant→只取回复内容、system→跳过 |
| **自动重试机制** | 分身失败自动重试一次，不再半途而废 |
| **Python 清理分身** | 操作 chats.json 必须用 Python 脚本，禁止 edit_file 文本替换（防止搞崩 JSON 导致系统崩溃） |
| **质量红线** | 分身失败必须重试、修改 JSON 必须用 Python |

## 依赖

- QwenPaw（任意版本）
- 无需安装任何额外工具
