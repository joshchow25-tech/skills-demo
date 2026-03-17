---
name: git-commit
description: >
  为代码变更自动生成高质量的 Git commit 消息，遵循 Conventional Commits 规范。
  当用户说"帮我写 commit"、"生成提交信息"、"commit 这些改动"、"提交代码"，
  或者用户完成了一些代码修改并想要提交时，使用此 skill。
  也适用于用户询问"怎么写 commit message"或者描述了一些代码改动想要一个好的提交信息的情况。
  Use this skill whenever the user wants to commit code changes, generate commit messages,
  or asks for help with git commits.
---

# Git Commit 消息生成

## 目标

帮助用户为代码变更生成清晰、语义化的 Git commit 消息，遵循 Conventional Commits 规范。

## 工作流程

### 第一步：了解变更内容

运行以下命令获取变更信息：

```bash
# 查看暂存区的变更（即将提交的）
git diff --staged

# 如果暂存区为空，查看工作区变更
git diff

# 查看变更了哪些文件
git status

# 参考最近的 commit 风格
git log --oneline -10
```

如果用户直接描述了改动（而不是在 git 仓库里），基于他们的描述来生成。

### 第二步：分析变更，选择类型

根据变更内容判断 commit 类型：

| 类型 | 用途 | 示例 |
|------|------|------|
| `feat` | 新功能 | 添加用户登录功能 |
| `fix` | Bug 修复 | 修复空指针异常 |
| `refactor` | 重构（不改变行为） | 提取公共方法 |
| `style` | 代码格式（不影响逻辑） | 统一缩进 |
| `docs` | 文档变更 | 更新 README |
| `test` | 测试相关 | 添加单元测试 |
| `chore` | 构建/工具/依赖 | 升级依赖版本 |
| `perf` | 性能优化 | 优化数据库查询 |

### 第三步：生成 commit 消息

**格式：**
```
<type>(<scope>): <简短描述>

[可选：详细说明]

[可选：关联 issue，如 Closes #123]
```

**规则：**
- 第一行不超过 72 个字符
- 简短描述用祈使句、动词开头（"Add" 不是 "Added"）
- 中文项目可以用中文描述，英文项目用英文
- scope 是可选的，表示影响范围（如 `feat(auth):`）

### 第四步：输出结果

给出 1-3 个候选 commit 消息（按推荐度排序），并简要说明选择理由。
然后询问用户是否需要调整，或者是否直接执行提交。

如果用户确认，执行：
```bash
git add -A
git commit -m "<message>"
```

---

## 示例

### 新功能
**描述：** 添加了 JWT 认证，POST /api/login 返回 24 小时有效 token

```
feat(auth): add JWT authentication to login endpoint

POST /api/login now returns a signed JWT token with 24-hour expiry.
All protected routes require the token in subsequent requests.
```

### Bug 修复
**描述：** 修复购物车为空时结算报错

```
fix(cart): handle empty cart on checkout

Clicking checkout with empty cart threw TypeError.
Added empty-cart check that shows "请先添加商品" prompt.
```

### 重构
**描述：** 提取三个组件的重复日期格式化代码

```
refactor: extract formatDate utility to eliminate duplication

Move repeated date formatting logic from UserCard, PostList,
and EventCalendar into src/utils/formatDate.js.
```

---

## 注意事项

- **不要提交敏感信息**：如果 diff 里有 API key、密码等，提醒用户在提交前处理
- **Breaking Change**：如果改动不向后兼容，末尾加 `BREAKING CHANGE: <说明>`
- **参考已有风格**：用 `git log` 了解项目已有的 commit 风格，尽量保持一致
