# Skills

[English](README.md) | 中文

个人维护的 Agent Skills 集合。每个 Skill 都放在 `skills/<skill-name>/` 下，并以 `SKILL.md` 作为入口文件。

## 已收录

### submitting-pull-requests

面向 GitHub Fork 与 Pull Request 贡献流程的中文 Skill，覆盖：

- 从真实用户视角安装、运行和验证项目；
- Issue 选择、项目官方提交规范与沟通流程；
- 分支、上游同步、测试、敏感信息和发布检查；
- 近期 Issue/PR 格式参考与历史 PR 成败复盘；
- PR 创建、CI、审查和合并后收尾；
- 输入、流程、输出和验收标准。

文件：

- [英文入口 SKILL.md](skills/submitting-pull-requests/SKILL.md)
- [中文完整版 SKILL.zh-CN.md](skills/submitting-pull-requests/SKILL.zh-CN.md)

### project-governance-kit

Project Governance Kit（PGK）的对话式入口，覆盖：

- 创建新治理项目，并停在 draft 需求确认；
- 对已有仓库先执行只读接入检查，确认前不写入；
- 恢复已治理项目，不重复初始化或创建重复记录；
- 明确的触发场景、输入、流程、输出、验收标准和授权门禁。

文件：

- [英文入口 SKILL.md](skills/project-governance-kit/SKILL.md)
- [中文完整版 SKILL.zh-CN.md](skills/project-governance-kit/SKILL.zh-CN.md)
- [Codex UI 元数据](skills/project-governance-kit/agents/openai.yaml)

## 目录约定

```text
skills/
  <skill-name>/
    SKILL.md              # 规范入口，默认英文
    SKILL.<locale>.md     # 可选的其他语言完整版本
    agents/       # 可选
    references/   # 可选
    scripts/      # 可选
    assets/       # 可选
```

每个 Skill 应保持自包含，并在提交前验证 YAML frontmatter、触发描述、流程完整性和配套资源。

## 语言约定

Agent Skills 运行时默认识别 `SKILL.md`。本仓库将英文版作为规范入口，将其他语言的完整版本保存为 `SKILL.<locale>.md`，避免运行时同时加载两种语言、增加上下文成本。

如果需要在本机使用中文版，请先把整个 Skill 目录复制到本地 skills 目录，再将 `SKILL.zh-CN.md` 调整为入口文件。PowerShell 示例：

```powershell
Copy-Item -Recurse .\skills\submitting-pull-requests $env:USERPROFILE\.codex\skills\submitting-pull-requests
Move-Item $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.md $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.en.md
Move-Item $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.zh-CN.md $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.md
```

macOS 或 Linux 使用对应的 `cp` 和 `mv` 命令。同一目录中不要同时保留两个 `SKILL.md`。

## 使用

将需要的 Skill 目录复制到本地 Codex Skills 目录：

```text
~/.codex/skills/<skill-name>/
```

也可以放入支持 Agent Skills 约定的其他运行时目录，例如：

```text
~/.agents/skills/<skill-name>/
```

## 后续维护

新增 Skill 时，在 `skills/` 下创建独立目录，并同时更新中英文 README 的“已收录/Included Skills”列表。提交前不要包含凭据、私有配置、会话记录或个人环境文件。

## 许可证

本仓库采用 [MIT License](LICENSE) 授权。
