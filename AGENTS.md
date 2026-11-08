# AGENTS.md — brick_bookmark_profile

公开门面仓库：落地页、隐私政策、下载入口、公开 issue。**这里的内容所有人可见，不要写入内部仓库细节、私有 issue 链接或内部计划。**

## 文档导航

`index.md` / `index.en.md`（中英文落地页）→ `privacy/`（隐私政策）→ `docs/ISSUE_TRIAGE.md`（公开 issue 分诊）

## 行为约束

- 公开仓库只写公开信息：不出现内部仓库名、私有 issue 链接、未发布的内部计划。
- 改 `index.md` 必须同步 `index.en.md`；改隐私政策必须同步中英两版。
- 下载列表跟扩展端最新**正式** Release；rc / 预览版不放主下载列表。
- 隐私政策路径为 `privacy/<版本>/<浏览器>/PRIVACY.md`；改完要验证对应 Pages URL 返回 200。
- 公开 issue 分诊遵循 `docs/ISSUE_TRIAGE.md`：先打 `needs-triage`，确认类型后转 `bug` / `enhancement` / `question`；修复后回复版本号再关闭，不删除 issue。
- 不在本仓库直接改扩展端 / 服务器端 / 管理后台代码；跨仓库通过 issue 和 PR。
- 需要内部修复的公开 issue，只在公开 issue 回复进展，不贴内部 issue 链接。
- Pages 由 `main` 分支自动部署；合并后检查 `https://brick9527.github.io/brick_bookmark_profile/`。
