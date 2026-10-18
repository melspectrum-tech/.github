# 贡献指南 · Contributing

这份约定适用于 `melspectrum-tech` 组织下的所有仓库。如果某个仓库自己有 `CONTRIBUTING.md`，以那份为准，没写到的地方仍按本文执行。

## 1. 分支

`main` 必须始终处于可构建、可发布的状态。**不要直接往 `main` 推送，也不要对它 force push。**

新工作一律从最新的 `main` 拉分支，分支名用 `类型/简短描述`，全部小写，单词之间用连字符：

| 前缀 | 用途 | 例子 |
|---|---|---|
| `feat/` | 新功能 | `feat/ble-ota-resume` |
| `fix/` | 修 bug | `fix/bass-octave-error` |
| `docs/` | 只改文档 | `docs/pinmap-v2.1` |
| `refactor/` | 重构，不改变行为 | `refactor/foc-driver-split` |
| `test/` | 补测试，或调整测试台 | `test/hil-pitch-sweep` |
| `chore/` | 构建、依赖、CI、杂项 | `chore/bump-idf-6.1` |
| `exp/` | 实验或调参，不一定会合并 | `exp/stage2-tf-band` |

- AI agent 建的分支（`claude/…`、`codex/…`）也走同样的流程，合并或放弃后删掉。
- `archive/…` 和 `wip/…` 用来存放历史快照，**不要删除，也不要往里追加提交**。
- 分支要短命：超过一周没合并的分支，先 rebase 到最新的 `main`，或者干脆拆小。

## 2. 提交信息：Conventional Commits

```
<type>(<scope>): <subject>

<body，可选：为什么这么改、有什么取舍>

<footer，可选：BREAKING CHANGE: …，或者 Refs #123>
```

- `type` 取值：`feat` `fix` `docs` `refactor` `perf` `test` `build` `ci` `chore` `revert`。
- `scope` 写模块名，例如 `ble`、`foc`、`stage2`、`hil`、`ui`。
- `subject` 用中文或英文都可以，不加句号，写清楚“做了什么”。
- 破坏性改动（协议、存储格式、API）必须在 footer 写 `BREAKING CHANGE:`。

例子：

```
fix(ble): gate OTA characteristic writes on auth
feat(stage2): add event-level precision report
docs(hil): 十个坑补充说明
```

## 3. Pull Request 与合并

1. 推送自己的分支，然后向 `main` 开 PR，按 PR 模板填写。
2. **PR 标题必须是一条合格的 Conventional Commit**，因为 squash 合并后，它就是 `main` 上那条提交的信息。
3. 合并前至少要有**另一位成员** review 通过。紧急修复可以先自己合并，但要在 PR 里写明原因，事后补 review。
4. 仓库只开了 **Squash merge**，合并后分支会被自动删除。
5. 一个 PR 只做一件事。改动太大（比如超过约 400 行实际改动），先拆分。
6. 固件、硬件相关的改动，要在 PR 里写清楚在哪块板子、哪个版本上验证过。

> 当前组织是 GitHub Free 计划，私有仓库没法强制分支保护和必需 review。以上规则靠大家自觉遵守。

## 4. 绝不提交的东西

**密钥和凭据**：`.env`、API key/token、密码、私钥（SSH、TLS、WireGuard `PrivateKey`、签名证书）、云厂商凭据、Cloudflare token，以及 OTA、BLE 的设备密钥。

- 配置用 `.env.example` 加上被 git 忽略的 `.env`，生产密钥只放在服务器上。
- 提交前跑一次扫描：`gitleaks git --staged --pre-commit`，或者 `gitleaks dir .`。
- **一旦误提交了：第一时间轮换这个密钥，并通知仓库负责人。** 改写 git 历史并不能撤回已经推送出去的密钥。

**环境和产物**：

- 虚拟环境：`venv/`、`.venv/`、`env/`、conda 环境、`site-packages/`。
- 依赖目录：`node_modules/`、`Pods/`。
- 构建产物：`build/`、`.pio/`、`DerivedData/`、`.gradle/`、`dist/`。
- 个人和系统文件：`xcuserdata/`、`.DS_Store`、`.idea/` 等。

**数据和模型**：原始数据集（EDF、录音等）、训练缓存（`.npz`）、模型权重。

- 大文件放在训练服务器或专门的存储上，仓库里只放读取脚本和说明。
- 已经配置了 Git LFS 的仓库按它的 `.gitattributes` 来。
- 单个文件超过约 5MB 之前，先想清楚它是否真的需要进 git。

## 5. 本地工作区

- **不要把仓库放在 iCloud、Dropbox 等同步目录里。** 它们会产生 `main 2` 这类冲突文件，并且会把文件驱逐成占位符，从而损坏 `.git`。建议统一放在 `~/Code/`。
- 每个仓库只保留一份工作副本，需要并行工作就用 `git worktree`，不要复制整个目录。
- 提交身份用公司邮箱，并且在 GitHub 上完成验证。

## 6. 保密

除非仓库本身是公开的，否则这里的代码、文档和数据都属于公司保密信息。

- 不要复制到个人仓库、公开的 gist、论坛或其他公开渠道。
- 使用 AI 编程工具时，只能用公司认可的账号和工具。
- 对尚未公开或仍在申请专利的技术内容尤其要谨慎。
