# 设置与维护

本模板用于 `github.com/{{GITHUB_USERNAME}}` 的 **Profile README**。独立 GitHub Pages 网站不在本次范围内，无需配置 Pages、域名或前端构建。

## 1. 创建个人主页仓库

1. 登录 GitHub，在个人账号下选择 **New repository**。
2. 仓库名填写自己的 GitHub 用户名 `{{GITHUB_USERNAME}}`，可见性选择 **Public**。
3. 可勾选 **Add README** 后创建仓库；如已有同名公开仓库，直接使用并保留原有内容。
4. 将本模板放在仓库根目录，让非空的 `README.md` 位于根目录，而不是额外的子文件夹中。

GitHub 要求仓库与用户名同名、仓库公开、根目录有非空 `README.md`，才会在个人主页显示。企业托管用户账号不支持 Profile README；2020 年 7 月以前建立的同名公开仓库可能需要点击 **Share to profile**。参见 [官方显示规则](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme)。

## 2. 文件与设计

```text
README.md                         # 已提交的展示结果
profile.config.json               # 数组顺序决定展示顺序，enabled 决定开关
modules/
  intro.md                        # 一句话开发方向
  projects.md                     # 2–4 个精选项目，模板先提供 2 个
  contributions.md                # 精选贡献及可核实的讨论链接
  notes.md                        # 自己撰写的技术内容，默认关闭
scripts/
  build_readme.py                  # Python 标准库生成器
.github/
  workflows/
    update-profile.yml            # 默认分支自动生成与提交
docs/
  SETUP.md                        # 本文
.gitattributes                    # 固定文本换行
```

参考 [superboySB 的主页](https://github.com/superboySB) 的清晰层次，采用短段落、标题和列表；技术随项目展示。README 解释作品，原生置顶区提供重点仓库入口。无图片、统计卡片、动态接口或独立技能墙，适配窄屏和明暗主题。

所有具体信息均为 `{{PLACEHOLDER}}`，这不是变量替换系统，脚本会原样合并。未知链接保留为纯文本占位，不填写虚构地址。后续补充真实内容时，项目链接优先指向 GitHub，贡献链接指向对应 PR、Issue 或 Discussion；外链仅用于直接相关的开源作品或自己撰写的技术文章。真实链接可写成 `[{{LINK_LABEL}}]({{VERIFIED_URL}})`，只在确认地址后使用这个 Markdown 链接格式。

GitHub 不会自动合并这些 Markdown 文件。浏览个人主页时读取的是已提交的根目录 README，不会执行 Python，也不依赖 Actions 正在运行。无需额外安装 Python 包。

## 3. 首次生成、上传与启用 Actions

本地需安装 **Python 3.9 或更高版本**；工作流使用 Python 3.13。在仓库根目录运行（部分系统命令为 `python3`，Windows 也可用 `py`）：

```sh
python scripts/build_readme.py
python scripts/build_readme.py --check
```

首次上传可选择以下一种方式：

- **网页上传**：打开仓库，选择 **Add file → Upload files**，按上述结构上传全部文件及生成的 README。检查隐藏目录 `.github/workflows/` 和文件 `.gitattributes` 也已上传；若上传器遗漏，可用 **Add file → Create new file** 按完整路径创建。确认差异后提交到默认分支。
- **Git 上传**：先克隆新建的仓库，将模板文件复制到克隆目录，生成 README 后执行下列命令。已有文件先检查差异，按需合并；不要用整目录覆盖已有仓库。

```sh
git clone https://github.com/{{GITHUB_USERNAME}}/{{GITHUB_USERNAME}}.git
cd {{GITHUB_USERNAME}}
# 此时复制模板文件到当前目录，然后生成并检查 README。
python scripts/build_readme.py
python scripts/build_readme.py --check
git status --short
git add README.md profile.config.json modules scripts/build_readme.py .github/workflows/update-profile.yml docs/SETUP.md .gitattributes
git diff --cached
git commit -m "docs: add modular profile template"
git push
```

`{{GITHUB_USERNAME}}` 等命令占位需先替换。首次上传已经包含生成的 README，Actions 未启用也能展示。

在仓库 **Settings → Actions → General** 确认 Actions 已启用，并允许 GitHub 官方的 `actions/checkout` 和 `actions/setup-python`。工作流只在更新任务中声明 `contents: write`，其他可配置权限关闭，使用仓库自动提供的 `GITHUB_TOKEN`，不用创建 PAT 或添加 Secret，也不用允许创建或审批 PR。权限设置与组织限制见 [官方设置说明](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository)。

进入 **Actions → Update profile README → Run workflow**，选择默认分支手动运行一次。工作流文件必须先存在于默认分支，才能正常使用手动入口；非默认分支的任务会跳过。[手动运行说明](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manually-run-a-workflow)

## 4. 头像、Bio 与置顶仓库

- 在个人头像菜单进入 **Settings → Public profile**，设置头像和简短 Bio；Bio 可按“专注于 {{PRIMARY_DEVELOPMENT_DIRECTION}}”补充真实信息。这些设置不由 README 脚本管理。[个人资料说明](https://docs.github.com/en/account-and-profile/tutorials/personalize-your-profile)
- 在个人主页的 **Popular repositories / Pinned → Customize your pins** 选择代表仓库并排序，然后 **Save pins**。建议先选与 README 中 2–4 个项目对应的仓库；原生置顶最多可选 6 个仓库或 gist，受 GitHub 的可选范围限制。[置顶操作说明](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/pinning-items-to-your-profile)

## 5. 修改、开关与排序

日常编辑 `modules/` 和 `profile.config.json`，不要直接维护生成的 README；直接改 README 会在下次生成时被覆盖。无关文件可照常保留。

- **修改内容**：编辑对应 `.md`。项目可复制一个项目块，使用新的占位编号，保持 2–4 个精选项目；贡献写清项目、问题、解决方式和证据链接。没有真实内容时继续保留占位。
- **关闭模块**：把该项 `enabled` 设为 JSON 布尔值 `false`；重新生成后该模块不会出现。notes 默认如此。
- **启用 notes**：编辑 `modules/notes.md`，将 notes 的 `enabled` 改为 `true`，仅收录自己撰写的技术文档、教程或社区文章。
- **调整顺序**：移动 `modules` 数组中的整个对象；文件名或磁盘顺序不会影响结果。
- **新增或拆分模块**：先创建 `modules/{{MODULE_NAME}}.md`，再在数组中加入 `{ "name": "{{MODULE_NAME}}", "enabled": true }`。名称实际填写小写字母开头、仅含小写字母/数字/下划线/连字符的唯一名称，不含路径和扩展名；展示内容仍保持 intro、projects、contributions、可选 notes 的范围。
- **移除模块**：从配置删除对象后再删除文件；仅关闭时仍保留文件。

配置根对象只包含 `modules` 非空数组，每项只允许 `name` 和 `enabled`；`enabled` 必须是 `true` 或 `false`，不能写字符串或数字。至少启用一个模块。所有已登记文件（包括关闭的模块）都必须存在、为非空 UTF-8 文本且不含 NUL；名称、键或模块重复都会报错。未登记的文件不会加入 README。

网页编辑时，打开对应模块，点击铅笔编辑并提交到默认分支。也可在分支编辑后通过 PR 合并；合并到默认分支才会自动生成。配置与新模块应一起提交，避免中间状态校验失败。提交后查看 Actions 结果，成功时机器人只会在 README 有变化时追加一个提交。

自动触发范围是 `modules/**`、配置、`scripts/**`、工作流自身和 `.gitattributes`。其他分支和标签即使产生工作流记录，也不会运行更新任务。工作流不会硬编码默认分支名。README 单独变化不触发；`GITHUB_TOKEN` 的推送也不会再次触发 push 工作流，因此不会循环。[事件规则](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/trigger-a-workflow)

## 6. 本地检查与恢复

```sh
git pull --ff-only
# 编辑 modules/ 或配置后：
python scripts/build_readme.py
python scripts/build_readme.py --check
git diff -- README.md modules profile.config.json
```

脚本从自身位置定位仓库，因此也可从其他目录用脚本完整路径运行。它按配置顺序合并启用内容，模块间空一行，输出 UTF-8（无 BOM）和 LF，末尾一个换行，不添加时间。内容字节相同就不写文件；所有输入校验完成后才写临时文件并原子替换 README，校验或替换失败时保留旧 README。输入可使用 UTF-8 BOM 和常见换行，输出始终规范化。

退出码：`0` 表示成功或检查一致；`--check` 返回 `1` 表示 README 缺失或过期；`2` 表示配置、模块或文件操作错误。`--check` 不写 README，也不会创建临时文件。只在内容改变后提交生成结果，无需反复生成。

若仅需撤销尚未提交的源文件修改，先查看 `git diff`，再对确认要丢弃修改的具体文件执行 `git restore -- {{SOURCE_PATH}}`，然后重新生成。

恢复已经提交的旧版时，先用 `git log --oneline -- modules profile.config.json scripts/build_readme.py` 找到目标；优先撤销引入错误的**源文件提交**：

```sh
git revert {{SOURCE_COMMIT_SHA}}
python scripts/build_readme.py
python scripts/build_readme.py --check
git diff -- README.md
# README 有变化才执行下面的 git add 和 git commit；无变化则直接 git push。
git add README.md
git commit -m "docs: regenerate restored profile"
git push
```

也可以 `git restore --source={{GOOD_COMMIT_SHA}} -- modules profile.config.json scripts/build_readme.py`，再生成、检查并把源文件与 README 一起提交。恢复脚本有变更时应同时检查相应的工作流。不要只回退机器人生成的 README 提交，否则当前源文件下次仍会生成新版。无需改写历史或强制推送。

## 7. 自动更新的边界与排错

更新任务通过同一个 concurrency 组串行运行，不取消正在执行的任务；默认队列可能合并等待中的运行，开始时始终检出默认分支最新内容。只暂存和提交 README，避免空提交，使用普通 `git push`。如果运行期间有人推送导致非快进拒绝，直接重新运行工作流，以最新源文件重新生成；不会通过 rebase 复用过期结果。[并发规则](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency)

| 现象 | 处理 |
| --- | --- |
| 个人主页没有 README | 核对仓库名与当前用户名一致、Public、根目录准确命名的 `README.md` 非空；不要把模板放进子目录。旧仓库检查 Share to profile，托管账号检查是否支持此功能。 |
| 页面还是旧内容 | 确认改的是默认分支源文件；检查 Actions 是否成功、生成提交是否进入默认分支。先运行 `--check`，需要时手动触发。 |
| 没有 Run workflow 按钮或运行被跳过 | 确认工作流已提交到默认分支且未禁用、有仓库写权限；手动选择默认分支。 |
| 配置错误或模块缺失 | 根据构建日志修正 JSON、模块名称、文件编码及非空内容；关闭的模块也需有效。失败时原 README 保留。 |
| 403 / permission denied | 查看任务日志中的 GITHUB_TOKEN Permissions 是否有 Contents: write，确认任务里的 `contents: write` 未移除；在 Settings → Actions → General 检查 Actions 权限与 Workflow permissions，若受组织/企业策略限制，请管理员允许所需权限。不要通过添加 PAT 处理。 |
| 受保护分支 / ruleset 拒绝机器人推送 | `contents: write` 不绕过保护规则。保留现有保护，改为在允许的分支本地生成并把源文件与 README 一起通过 PR 合并；默认分支已一致时工作流会跳过提交。必要时按仓库政策调整规则，不使用强制推送。 |
| 非快进推送失败 | 其他提交抢先进入了默认分支；重新运行工作流即可重新检出和生成。 |

分支保护的推送限制以 [GitHub 官方说明](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) 为准。组织策略无法调整或 Actions 不可用时，仍可按本地生成流程提交 README，主页显示不依赖自动化。

## 8. Actions 版本维护

已于 **2026-11-06** 核验官方文档与发布页：工作流使用 GitHub 官方维护的 [actions/checkout v7.0.1](https://github.com/actions/checkout/releases/tag/v7.0.1) 和 [actions/setup-python v7.1.0](https://github.com/actions/setup-python/releases/tag/v7.1.0)，均固定完整提交 SHA，注释记录版本，使用 GitHub 托管 Ubuntu runner。更新时核对官方发布说明、对应提交和 runner 要求，再同时修改 SHA 与版本注释。权限遵循 [GITHUB_TOKEN 最小权限说明](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token)。

## 9. 本地验证记录

Python 3.13.5 下，13 组验证全部通过；在临时副本中修改输入，未改变交付的占位内容。

| 验证 | 结果 |
| --- | --- |
| 模块排序 | 调整数组后，生成内容按新顺序排列。 |
| 模块关闭与启用 | 关闭 projects 后消失，启用 notes 后出现；交付配置仍为 notes 关闭。 |
| 重复生成 | 连续生成及一致性检查均保持 README 字节和修改时间不变。 |
| `--check` | 一致返回 0；过期或缺失返回 1，均不写文件。 |
| 异常保护 | 无效 JSON、重复项、缺失/空白/编码错误模块均返回 2；模拟临时文件、写入同步和替换失败，旧 README 均保留，临时文件清理。 |
| 编码与路径 | 输出为 UTF-8/LF；保留 Markdown 行尾空格，支持新增模块和从其他目录调用。 |
| 提交逻辑 | 在本地 Git 远端执行工作流中的提交步骤，验证无空提交、只提交 README、非快进推送被拒绝且远端历史保留。 |

以上验证在本地执行；首次上传后按第 3 节手动运行工作流，确认实际仓库权限及在线执行结果。
