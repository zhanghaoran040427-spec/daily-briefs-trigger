# daily-briefs-trigger

公开的空壳仓库，只做一件事：每天定时触发私有仓库 **daily-briefs** 工作流。

- 每天 07:30（北京时间）：触发四封晨报（.github/workflows/morning.yml）
- 每天 21:35（北京时间）：触发持仓报告（.github/workflows/evening.yml）

本仓库**不包含任何个人数据**（持仓、邮箱等都在私有仓库里）。
触发用的 token 存在本仓库的 Actions secret `DISPATCH_TOKEN` 中，不会公开。

晨报工作流每天会提交一个 `.last-run` 小文件，避免公开仓库 60 天没有提交导致定时任务被 GitHub 自动暂停。
