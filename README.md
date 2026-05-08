# 校望 / CampusRise

Version: `1.1`

校望（CampusRise）是一个母校叙事海报 Skill。它会根据人物照片和学校信息，引导生成一张带有校园记忆、学校精神和个人成长感的纪念海报。

## 技能是什么

CampusRise 适合两类场景：

- 给应届或即将毕业的同学生成更有仪式感的毕业纪念海报。
- 给毕业多年的校友生成带有母校回望、精神传承和继续向上感的纪念海报。

它不是单段固定提示词，而是一个工作流：先理解人物照片和学校线索，再补齐必要信息，最后生成更稳定的海报提示词或图像结果。

## 版本

当前版本：`1.1`

## 安装

### Codex

在 Codex 中使用内置 skill installer，从 GitHub 安装：

```bash
python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo foxbitcoo/CampusRise --path .
```

如果你在 Windows 上使用 Codex，脚本通常位于：

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" --repo foxbitcoo/CampusRise --path .
```

安装后重启 Codex，让新 Skill 生效。

### Claude Code

Claude Code 目前没有和 Codex 完全相同的 Skill Installer。可以用兼容方式使用：

1. 克隆仓库：

```bash
git clone https://github.com/foxbitcoo/CampusRise.git
```

2. 在 Claude Code 中打开该目录，或把 `SKILL.md` 和 `references/` 放到你的项目中。

3. 让 Claude Code 按 `SKILL.md` 执行，例如：

```text
Read SKILL.md and use CampusRise to generate an alma-mater narrative poster workflow.
```

## 自动更新

CampusRise follows the repository `main` branch as its lightweight release channel. When automatic updates are enabled, keep `main` clean: every merge can become the version that users receive.

The recommended setup is to run updates from the agent harness before any skill is loaded, not from inside `SKILL.md`. For Claude Code-compatible environments, add a `SessionStart` hook to your settings:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "npx skills update -g -y 2>/dev/null"
          }
        ]
      }
    ]
  }
}
```

An example file is provided at [`hooks/session-start-update.example.json`](hooks/session-start-update.example.json).

Why this is outside `SKILL.md`:

- `SKILL.md` should describe the poster workflow itself.
- The hook updates the local skill copy before the agent reads skill context.
- The update command consumes startup time, not task-time tokens.
- `matcher: "startup"` avoids updating on every resume, clear, or compact event.

Security notes:

- Enable automatic updates only for repositories you trust.
- If you want a frozen version, install or pin a tag instead of following `main`.
- If this skill later adds executable files such as `scripts/`, treat auto-update as a software supply-chain decision and keep a rollback tag.

## Bad Case 反馈

如果你遇到效果不好的案例，欢迎提交 GitHub Issue：

[https://github.com/foxbitcoo/CampusRise/issues](https://github.com/foxbitcoo/CampusRise/issues)

建议提供：

- 你希望生成什么
- 实际结果哪里不对
- 学校名称
- 大致场景（更偏毕业纪念，还是校友回望）
- 可公开的截图或描述

如果你在支持 GitHub issue 操作的代理环境中使用本 Skill，也可以直接说：

```text
report to issue: 这里写你的问题和评论
```

Skill 会整理本次请求摘要、生成结果摘要和你的评论，并尝试自动提交到本仓库。
如果你提供了已有 issue 编号或链接，Skill 会优先把内容补充为 comment；否则会新建一个 bad case issue。

## 设计理念

CampusRise 的核心不是把所有人套进同一个毕业照模板，而是让海报更像“这个人”和“这所学校”的连接。

设计上会做几个判断：

- 先从人物状态和整体气质判断更适合毕业纪念感，还是校友回望感。
- 尽量从图像里找学校线索，减少用户输入负担。
- 围绕学校代表元素、校训或学校精神组织画面，而不是生成泛化校园背景。
- 参考校徽、校色或学校代表色来控制整体色调，让画面更有母校亲近感。
- 保留人物辨识度，让用户第一眼能看到这张海报和自己有关。
