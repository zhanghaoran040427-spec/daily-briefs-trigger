# daily-briefs-trigger

公开的空壳仓库，只做一件事：每天定时触发私有仓库 **daily-briefs** 工作流。

- 每天 07:30（北京时间）：触发四封晨报（.github/workflows/morning.yml）
- 每天 08:00（北京时间）：再触发一次晨报兜底；私有仓库会查“今天发过没有”，发过就跳过
- 每天 21:35（北京时间）：触发持仓报告（.github/workflows/evening.yml）
- 每天 22:05（北京时间）：再触发一次持仓报告兜底，同样不会重复发

本仓库**不包含任何个人数据**（持仓、邮箱等都在私有仓库里）。
触发用的 token 存在本仓库的 Actions secret `DISPATCH_TOKEN` 中，不会公开。

## 关于 dry_run

本仓库的 `dry_run` 输入**已经被忽略**：无论请求里写没写，都会触发“真发送”。
这样即使外部定时器（cron-job.org）的 body 里还留着 `{"dry_run":true}`，也不会导致“跑了但没发邮件”。
想试运行请到私有仓库 daily-briefs 的 Actions 里手动 Run workflow，勾上 `dry_run`。

晨报工作流每天会提交一个 `.last-run` 小文件，避免公开仓库 60 天没有提交导致定时任务被 GitHub 自动暂停。
