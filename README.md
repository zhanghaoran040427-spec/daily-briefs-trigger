# daily-briefs-trigger

公开的空壳仓库，只做一件事：被外部定时服务叫醒后，触发私有仓库 **daily-briefs** 的工作流。

## 触发方式：只靠外部定时器（cron-job.org）

| 北京时间 | 触发谁 | 发出什么 |
|---|---|---|
| 每天 07:30 | `.github/workflows/morning.yml` | 四封晨报（金融晨报 / 每日一书 / 英语任务卡 / AI 日报） |
| 每天 21:35 | `.github/workflows/evening.yml` | 持仓报告（非交易日私有仓库会自己跳过） |

cron-job.org 里是两个任务，URL 形如：

```
https://api.github.com/repos/zhanghaoran040427-spec/daily-briefs-trigger/actions/workflows/morning.yml/dispatches
```

POST，body `{"ref":"main"}`，Headers 带 `Authorization: Bearer <细粒度 token>`。

## 为什么不用 GitHub 自带的 schedule

GitHub 的 `schedule` 是 best-effort，实测每天迟到 2–8 小时（07:30 拖到 11:58、21:35 拖到次日 06:26）。
所以本仓库的 `schedule` **已全部删除**，定时只由 cron-job.org 负责，避免 GitHub 那次迟到或提前执行抢先把当天的额度用掉。

## 想加“半小时后补一次”

在 cron-job.org 里把两个任务各复制一份，时间改成 08:00 与 22:05 即可。
私有仓库会查 `state/sent.json`，同一场已经发过就直接跳过，**不会重复发**。

## 关于 dry_run

本仓库的 `dry_run` 输入**已被忽略**：请求里写没写，都会触发“真发送”。
想试运行请到私有仓库 daily-briefs 的 Actions 里手动 Run workflow，勾上 `dry_run`。

本仓库**不包含任何个人数据**（持仓、邮箱等都在私有仓库里）。
触发用的 token 存在本仓库的 Actions secret `DISPATCH_TOKEN` 中，不会公开。
