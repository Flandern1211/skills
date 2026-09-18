# Skills

[English](README.md) | 中文

可复用的 Agent Skills 集合。Agent 可以直接读取本仓库，本地安装只是可选方式。

## 使用方式

### 通过 GitHub 使用

把仓库地址和任务发给能够读取 GitHub 仓库的 Agent。

Agent 应当：

1. 检查仓库，并读取 `skills/*/SKILL.md` 中的 Skill 描述；
2. 选择匹配的 Skill，完整读取其说明和引用资源；
3. 按照 Skill 完成任务，并说明使用了哪个 Skill。

可复制提示词：

```text
请把 https://github.com/Flandern1211/skills 作为 Agent Skills 仓库读取。
检查所有 skills/*/SKILL.md，选择与我的任务匹配的 Skill，完整读取该
Skill 及其引用资源，然后按照说明完成任务：<描述任务>
```

适用于 Codex、Claude Code、Gemini CLI、GitHub Copilot、OpenCode，以及其他能够读取仓库的 Agent。如果无法直接访问远端仓库，让 Agent 自行克隆到工作区。

### 本地安装（可选）

只有运行时需要自动发现或离线使用时才需要安装。按照对应 Agent 的文档，把完整 Skill 目录复制到它支持的位置，例如 `~/.agents/skills/` 或 `~/.codex/skills/`。

## 已收录 Skill

| Skill | 用途 | 文件 |
| --- | --- | --- |
| `submitting-pull-requests` | 通过 Fork 和 Pull Request 向 GitHub 仓库贡献，覆盖验证、同步、CI 和审查。 | [英文](skills/submitting-pull-requests/SKILL.md) · [中文](skills/submitting-pull-requests/SKILL.zh-CN.md) |
| `project-governance-kit` | 通过对话使用 Project Governance Kit 创建、接入或恢复治理项目。 | [英文](skills/project-governance-kit/SKILL.md) · [中文](skills/project-governance-kit/SKILL.zh-CN.md) · [可选 UI 元数据](skills/project-governance-kit/agents/openai.yaml) |

## 仓库结构

```text
skills/
  <skill-name>/
    SKILL.md              # 规范入口
    SKILL.<locale>.md     # 可选本地化版本
    agents/               # 可选 UI 元数据
    references/           # 可选文档
    scripts/              # 可选脚本
    assets/               # 可选资源
```

`SKILL.md` 是规范入口。Agent 可以优先读取对应语言版本；如果内容冲突，应以 `SKILL.md` 为准并说明差异。

## 贡献

在 `skills/<skill-name>/` 下新增 Skill，提供有效的 `SKILL.md`，并同步更新中英文 README。不要提交凭据、私有配置、会话记录或机器专用文件。

## 许可证

本仓库采用 [MIT License](LICENSE) 授权。
