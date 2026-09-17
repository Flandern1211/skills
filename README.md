# Skills

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

文件：[skills/submitting-pull-requests/SKILL.md](skills/submitting-pull-requests/SKILL.md)

## 目录约定

```text
skills/
  <skill-name>/
    SKILL.md
    agents/       # 可选
    references/   # 可选
    scripts/      # 可选
    assets/       # 可选
```

每个 Skill 应保持自包含，并在提交前验证 YAML frontmatter、触发描述、流程完整性和配套资源。

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

新增 Skill 时，在 `skills/` 下创建独立目录，并更新本 README 的“已收录”列表。提交前不要包含凭据、私有配置、会话记录或个人环境文件。
