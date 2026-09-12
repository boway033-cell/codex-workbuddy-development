# codex-workbuddy-development

一个面向个人开发者和小团队的 Codex Skill：让 **Codex 负责需求分析、任务编排、最终验收与 Git 交付**，让 **WorkBuddy 负责主要开发与迭代调试**。

它解决的不是“如何写一条 Git 命令”，而是如何把一个模糊想法稳定地推进为可验收、可提交、可发布的产品变更。

> English: A Codex skill that turns rough product requests into bounded WorkBuddy implementation prompts, then brings the result through independent review, latest-main integration, Git delivery, and optional release verification.

## 为什么需要它

在 Codex 与 WorkBuddy 混合开发时，常见问题包括：

- 用户的原始想法未经澄清就直接进入编码；
- WorkBuddy 完成开发后，缺少独立验收；
- 修改长期堆在 `main`，或混入日志、备份和本地配置；
- 功能分支验收通过，但已经落后最新 `main`，直到最终合并才出现冲突；
- 只推了代码，却忘了同步 README、交接文档、CHANGELOG 和版本号；
- “开发完成、测试通过、已提交、已推送、已发布”被混成一句模糊结论。

本 Skill 把这些环节拆成有明确责任人和通过条件的门禁。

## 工作方式

```text
用户提出需求
    ↓
Codex：分析需求、澄清关键选择、定义验收标准
    ↓
Codex：生成可直接交给 WorkBuddy 的完整 Prompt
    ↓
WorkBuddy：主要开发、测试并留下交接记录
    ↓
Codex：独立审查、测试、产品验收和必要收口
    ↓
Codex：把最新 origin/main 合入功能分支并重新验收
    ↓
Codex：同步产品介绍、交接文档、CHANGELOG 和版本号
    ↓
Codex：精确暂存、提交并在获准后推送
    ↓
可选：PR → CI → merge → tag → GitHub Release ZIP → 部署
```

核心分工：

| 角色 | 主要职责 |
| --- | --- |
| 用户 | 产品目标、关键取舍、外部操作授权 |
| Codex | 需求塑形、WorkBuddy Prompt、独立验收、Git 与发布交付 |
| WorkBuddy | 主要实现、迭代调试、针对性测试、交接证据 |

## 安装

### Windows PowerShell

```powershell
git clone https://github.com/boway033-cell/codex-workbuddy-development.git "$env:USERPROFILE\.codex\skills\codex-workbuddy-development"
```

### macOS / Linux

```bash
git clone https://github.com/boway033-cell/codex-workbuddy-development.git ~/.codex/skills/codex-workbuddy-development
```

安装后新建一个 Codex 任务，让 Skill 目录被重新发现。也可以显式调用：

```text
$codex-workbuddy-development
```

如果目标目录已经存在，请先自行备份或更新现有副本；不要直接覆盖包含未保存修改的版本。

## 快速开始

### 1. 从模糊需求开始

```text
使用 $codex-workbuddy-development 帮我分析这个产品想法，
理清范围和验收标准，然后生成一份可直接交给 WorkBuddy 的开发 Prompt。
```

Codex 会先判断需求是否存在会改变产品行为、架构、隐私、费用或数据兼容性的关键歧义。必要时先向用户提问；其余细节以明确假设推进。

### 2. WorkBuddy 完成后交给 Codex

```text
WorkBuddy 已经完成本轮开发。
请使用 $codex-workbuddy-development 做最终监工：
审查全部修改，独立运行测试并完成范围内收口；先不要提交和推送。
```

### 3. 验收后提交并推送

```text
验收通过后，把最新 origin/main 合入当前功能分支并重新测试。
确认无误后更新产品文档和版本信息，提交并推送功能分支，但不要合并 main。
```

### 4. 完整发布

```text
使用 $codex-workbuddy-development 完成本版本的 GitHub 交付：
同步最新 main，完成发布文档和版本号收口，通过 PR/CI 后合并，
创建版本标签，并验证 GitHub Release 与产品 ZIP。
```

## 自动触发场景

Skill 默认允许隐式触发，适用于：

- 把产品想法整理成 WorkBuddy 开发任务；
- WorkBuddy 开发结果的监工、验收、复核或收口；
- WorkBuddy 把未提交修改留在 `main` 后的安全恢复；
- Codex 最终提交、推送、PR、CI 或版本交付；
- 读取相关 `.workbuddy` 交接、测试、截图或缺陷证据；
- 建立 Codex–WorkBuddy 分工明确的长期开发链条。

以下情况不会仅因仓库中存在 `.workbuddy` 而触发：

- 普通编程或 Git 知识问答；
- 与 WorkBuddy 交接无关的只读检查；
- Codex 独立完成的普通孤立修改；
- 用户明确指定另一套开发流程。

## 关键设计

### Codex 不是转发器

Codex 不会把用户的一句话原样丢给 WorkBuddy。它会先形成目标、当前行为、目标行为、范围、约束、验收标准、验证方案和交付边界，再生成不依赖聊天上下文的自包含 Prompt。

### WorkBuddy 的报告不是验收证明

WorkBuddy 应报告修改文件、测试命令、结果、假设和遗留风险；Codex 仍须独立检查 diff 并复现关键验证。

### “不要合并 main”不等于“不管 main”

功能分支验收后，应检查它是否落后远程主分支：

```bash
git fetch origin
git rev-list --left-right --count HEAD...origin/main
```

若已落后，保持在功能分支上，把 `origin/main` 合入功能分支：

```bash
git switch codex/<feature-branch>
git merge origin/main
```

方向是 `origin/main → 功能分支`，不是把功能分支提前合入 `main`。解决冲突后必须重新运行验收。

### 每一步都有独立授权边界

Skill 不会把这些状态混为一谈：

```text
实现 ≠ 测试 ≠ 提交 ≠ 推送 ≠ 合并 ≠ 发布 ≠ 部署
```

授权提交不自动授权推送；授权推送不自动授权合并或发布。

### 主动告诉用户下一步

每个停止点都会说明：当前阶段、门禁状态、下一步、责任人、可复制指令，以及暂时不会执行的外部操作。

## 仓库内容

```text
.
├── SKILL.md            Skill 的完整行为规范
├── agents/
│   └── openai.yaml     Codex 展示信息与隐式触发策略
├── README.md           安装与使用指南
└── LICENSE             MIT License
```

## 适用边界

这个 Skill 负责协调和质量门禁，不提供 WorkBuddy 本身，也不绕过 Codex、GitHub、云平台或生产环境的权限确认。不同仓库的测试、分支、版本和发布约定始终优先于示例命令。

## 贡献

欢迎通过 Issue 描述真实失败案例，或通过 Pull Request 改进触发边界、交接模板和验收门禁。建议每次修改聚焦一个可复现的问题，并说明修改前后的行为差异。

## License

[MIT](LICENSE)
