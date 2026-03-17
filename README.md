# skills-demo

Claude Code Skills 示例集合。每个子目录是一个独立的 skill，可以直接加载到 Claude Code 中使用。

## 什么是 Skill？

Skill 是一个 Markdown 文件（`SKILL.md`），包含：
- **YAML frontmatter**：`name` 和 `description`，用于匹配用户意图自动触发
- **正文**：给 Claude 的详细操作指令、格式规范、示例等

Claude 会根据用户输入自动识别并调用对应的 skill。

## Skills 列表

### [git-commit](./git-commit/)

自动生成高质量的 Git commit 消息，遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范。

**触发词：** "帮我写 commit"、"生成提交信息"、"commit 这些改动"

**功能：**
- 自动读取 `git diff` 分析代码变更
- 识别变更类型（feat / fix / refactor / docs 等）
- 生成 1-3 个候选 commit 消息
- 支持中英文，参考已有项目风格

## 目录结构

```
git-commit/
├── SKILL.md      # Skill 主文件（给 Claude 的指令）
└── evals.json    # 测试用例
```

## 如何使用

将 `SKILL.md` 放入 Claude Code 的 skills 目录，或通过 skill-creator 插件加载。
