# 参与贡献

[English](CONTRIBUTING.md)

## 环境搭建

需要 Docker、Node 22、pnpm 和 Python 3.9 以上。

```bash
docker compose up -d postgres storage
python3 scripts/fetch_cases.py
cd app && pnpm install && pnpm dev    # http://localhost:5173
```

其余的见 [docs/self-hosting.md](docs/self-hosting.md#working-on-the-site) 的 "Working on the site" 一节：怎么同时跑一台沙箱机，客户方的人和评分需要的模型密钥，以及怎么用单个进程运行。

## 提 PR 之前

把 CI 跑的东西在本地跑一遍（`.github/workflows/checks.yml`）：

```bash
cd app && pnpm typecheck && pnpm test && pnpm build
python3 -m compileall -q harness scripts
```

- 涉及数据记录的测试会在本地 PostgreSQL 里建自己的库，没有数据库时会跳过。CI 里有数据库，连不上就算失败。
- harness 和脚本只用标准库，并且承诺支持 Python 3.9：不要加依赖，也不要用 3.9 之后才有的写法。
- 如果改了交付代码的启动方式、沙箱机镜像，或者某道题的评分，请对着一个正在运行的站点跑 `python3 scripts/smoke_cases.py`（用法见文件开头）。得 0 分本身说明不了什么，要确认交付的系统确实加载了。

## 分支和 PR

- 从 `dev` 拉分支，PR 提到 `dev`。`main` 只接收来自 `dev` 的改动。
- 两个分支都有保护：`app` 和 `harness` 两项检查必须通过，不能直接推送。
- 一个 PR 只做一件事。写清楚它做了什么、你是怎么验证的；页面上的改动最好附一张截图。

## 题目

题目不在这个仓库里，也不应该进来：不要提交题目文件和参考解，代码、测试、截图和录屏里也不要出现能泄露题目答案的内容。配图和文档里请用三道公开的示例题（`campus_booking`、`oncall_incident`、`robot_cell_rollout`）。

## 许可

你贡献的内容和仓库其余部分一样，采用 Apache License 2.0（见 `LICENSE`）。
