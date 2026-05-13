# 校望 / CampusRise

Version: `V4.1`

校望（CampusRise）是一个母校叙事海报 Skill。它会根据人物照片和学校信息，引导生成一张带有校园记忆、学校精神和个人成长感的纪念海报。

## 技能是什么

CampusRise 当前发布版聚焦应届或即将毕业同学的小红书毕业纪念图场景。

它不是单段固定提示词，而是一个工作流：先理解人物照片和学校线索，再补齐必要信息，最后生成更稳定的海报提示词或图像结果。

## 版本

当前版本：`V4.1`

## 使用流程

1. 上传一张或多张人物照片。
2. 提供学校名称；如果照片里能可靠识别学校，Skill 会让你确认或更正。
3. 如果你已经在请求里明确写了学校名称，Skill 不会再二次确认，会直接进入学校资料核验。
4. Skill 会检索学校的官方或高可信信息，优先确认学校名称、校训、校色、代表建筑、校徽或关键符号。
5. 可选补充：姓名、专业 / 学院、校园经历 / 社团经历、想呈现的个人特质、毕业时间、签名名。
6. 如果没有声明毕业时间，应届毕业方向默认使用当前年份；如果你提供月份、季节或具体日期，版面允许时会作为小字信息层出现。
7. 生成结果默认走毕业纪念方向，除非你明确要求校友回望或返校叙事。

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

## 更新技能 / Update the Skill

中文：

- 这个 skill 的最新版本以 GitHub 仓库为准：`https://github.com/foxbitcoo/CampusRise`
- 如果用户的指令是“更新这个 CampusRise skill”，默认目标应该是：从这个 GitHub 仓库拉取最新版本，再同步覆盖本地安装的 skill 文件。
- 如果你是通过 `git clone` 使用：

```bash
git pull origin main
```

- 如果你是通过 Codex 的 GitHub 安装方式使用，更新后建议重新执行一次安装命令，让本地副本覆盖到最新版本。
- 如果你的 Agent 支持 `SessionStart` 自动更新，也可以在新会话开始时自动同步本技能。

English:

- The source of truth for this skill is the GitHub repository: `https://github.com/foxbitcoo/CampusRise`
- If the user's intent is "update this CampusRise skill", the default action should be: pull the latest version from this GitHub repository, then sync and overwrite the local installed skill files.
- If you use a cloned copy:

```bash
git pull origin main
```

- If you installed through Codex's GitHub installer, rerun the install command after updating so the local installed copy is refreshed.
- If the Agent supports `SessionStart` automatic update, it may sync this skill automatically at the beginning of a new session.

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

## 更新技能

最新版本以本 GitHub 仓库为准；如果你的 Agent 支持 SessionStart 自动更新，也可以让它在新会话开始时自动同步本技能。

## Bad Case 反馈

如果你遇到效果不好的案例，欢迎提交 GitHub Issue：

[https://github.com/foxbitcoo/CampusRise/issues](https://github.com/foxbitcoo/CampusRise/issues)

建议提供：

- 你希望生成什么
- 实际结果哪里不对
- 学校名称
- 大致场景（默认毕业纪念；如果是特殊校友/返校场景也请说明）
- 可公开的截图或描述

如果你在支持 GitHub issue 操作的代理环境中使用本 Skill，也可以直接说：

```text
report to issue: 这里写你的问题和评论
```

Skill 会整理本次请求摘要、生成结果摘要和你的评论，并尝试自动提交到本仓库。
如果你提供了已有 issue 编号或链接，Skill 会优先把内容补充为 comment；否则会新建一个 bad case issue。
如果当前环境没有 GitHub 写权限，Skill 会给出可手动提交的 issue 或 comment 草稿。

## 设计理念

CampusRise 的核心不是把所有人套进同一个毕业照模板，而是让海报更像“这个人”和“这所学校”的连接。

设计上会做几个判断：

- 发布版默认聚焦毕业纪念图，减少用户理解成本。
- 尽量从图像里找学校线索，减少用户输入负担。
- 用户已经明确学校时，不再二次确认。
- 围绕学校代表元素、校训或学校精神组织画面，而不是生成泛化校园背景。
- 对学校建筑、校徽和关键符号优先使用网页搜索、官方/高可信参考图，或用户提供的真实建筑照片；宁可慢一点，也要尽可能还原建筑名称、形状和具体细节。
- 参考校徽、校色或学校代表色来控制整体色调，让画面更有母校亲近感。
- 保留人物辨识度，让用户第一眼能看到这张海报和自己有关。
