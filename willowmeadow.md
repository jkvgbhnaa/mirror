# LinkGuardian 外链资产聚合系统

LinkGuardian 是一个面向技术内容运营团队、个人站长及自动化采集系统的开源外链资产汇总与管理平台。项目定位于将分散于多端、多域名、多批次的内容发布链接进行结构化采集、归档与状态监控，解决人工复制粘贴易出错、链接失效难追踪、多源数据无法统一检索的痛点。本仓库为第 25/90 批次资源导入基线，包含 10 条来自 yidianmeii.cn 域族的内容分发链路，覆盖 3g、wap、h5 三种移动端入口。项目内置链接解析器、HTTP 状态探测器、批量重定向跟踪器以及简单的静态看板生成脚本，适用于小规模外链资产的中继管理与健康巡检。

## 功能概览

- 批量链接导入解析：支持纯文本列表、CSV 及行分隔 URL 文件的批量读入，自动去重并校验协议头。
- 多协议状态探测：并发 HEAD/GET 请求检测链接可访问性，记录状态码、响应时间和最终重定向目标。
- 域名与路径归类：自动提取二级域名、发布路径（/jnews/YYYYMMDD/）及文件名，生成层级标签。
- 存活与失效报表输出：生成 Markdown 格式的巡检报告，按 HTTP 状态分组，标注 4xx/5xx 及超时链接。
- 变更监控与历史快照：每日定时任务（cron 可配）对比当前批次与前一日批次，输出新增/下架/状态变更列表。
- 静态看板生成器：基于 Jinja2 模板渲染 HTML 概览页，含响应时间趋势 mini 图（基于 matplotlib）。
- 元数据扩展字段：支持为每条链接手动备注来源批次、发布日期、责任编辑或审核状态，数据持久化为 SQLite。
- RESTful 查询接口（可选）：通过 Flask 提供 /api/links、/api/status 端点，便于集成到上游编排系统。

## 应用场景

1. 内容发布后的外链归档审计：编辑团队每日发布多端文章后，将各端生成的链接汇总导入系统，自动检查是否所有预期入口均成功发布且可访问，防止因发布流程遗漏导致用户无法访问特定终端的新闻页。

2. 域名迁移或改版时的重定向验证：当站点从旧域名切换至新域名，或 URL 路径规则由 /news/ 变更为 /jnews/ 时，系统可通过历史快照比对待重定向链接是否配置 301/302，并输出未跳转或跳转错误的异常列表。

3. 第三方采集源质量监控：对于依赖外部新闻源或合作方内容链接的业务，定期通过本系统探测对方接口返回的链接可用性，若发现大面积超时或 404，及时触发告警通知，避免空采或脏数据进入生产库。

4. 移动端差异化内容校验：针对 3g、wap、h5 三种用户代理终端，分别部署探测任务，对比同一新闻 ID 在不同终端下的响应体大小与关键字段（如标题、发布时间），识别终端适配缺失或内容截断问题。

5. 合规性链接快照留存：在监管审查或版权追溯场景下，系统可周期性地对指定批次链接做响应体摘要（MD5）记录，配合原始 URL 列表，形成不可篡改的审计凭证，供法务或运营调取。

## 快速开始

以下命令适用于 Linux/macOS 环境，Python 3.9 及以上版本。

```bash
# 克隆仓库
git clone https://github.com/yourorg/linkguardian.git
cd linkguardian

# 安装依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 将本批次 10 条链接导入数据目录（示例：生成 links.txt）
cat > data/batch_25_90.txt << EOF
http://3g.yidianmeii.cn/jnews/0814/9677241.shtml
http://wap.yidianmeii.cn/jnews/0814/122826.shtml
http://3g.yidianmeii.cn/jnews/0814/1344.shtml
http://h5.yidianmeii.cn/jnews/0814/9309703.shtml
http://h5.yidianmeii.cn/jnews/0814/2990188.shtml
http://3g.yidianmeii.cn/jnews/0814/087179.shtml
http://wap.yidianmeii.cn/jnews/0814/622361.shtml
http://wap.yidianmeii.cn/jnews/0814/29885.shtml
http://3g.yidianmeii.cn/jnews/0814/56992.shtml
http://h5.yidianmeii.cn/jnews/0814/2596365.shtml
EOF

# 运行批量探测任务
python cli.py probe --input data/batch_25_90.txt --output reports/batch_25_90_report.md

# 启动静态看板（本地预览）
python cli.py dashboard --input reports/batch_25_90_report.md --serve
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 及以上 | 核心运行环境，3.11 推荐以获得最佳 asyncio 性能 |
| requests | 2.31.0+ | 同步 HTTP 探测库，用于重定向跟踪与状态码获取 |
| aiohttp | 3.9.0+ | 异步并发探测引擎，用于批量快速扫描 |
| Jinja2 | 3.1.0+ | 看板页面模板渲染引擎 |
| matplotlib | 3.7.0+ | 可选依赖，用于生成响应时间趋势图（若不安装则看板跳过绘图） |
| SQLite3 | 系统自带 | 元数据存储与历史快照比对，无需额外安装 |
| pytest | 7.4.0+ | 仅开发测试需要，生产环境可不安装 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 入门 | docs/quickstart.md | 如何配置首次运行、如何导入自定义批次、如何理解输出报表 |
| 运维 | docs/operations.md | 如何设置定时巡检、如何对接告警通道（钉钉/邮件）、如何清理历史快照 |
| 开发 | docs/development.md | 如何扩展新的探测协议（如 FTP/RTMP）、如何增加自定义报表字段 |
| API | docs/api_reference.md | RESTful 接口的鉴权方式、分页参数、错误码定义及示例响应 |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/9677241.shtml
- http://wap.yidianmeii.cn/jnews/0814/122826.shtml
- http://3g.yidianmeii.cn/jnews/0814/1344.shtml
- http://h5.yidianmeii.cn/jnews/0814/9309703.shtml
- http://h5.yidianmeii.cn/jnews/0814/2990188.shtml
- http://3g.yidianmeii.cn/jnews/0814/087179.shtml
- http://wap.yidianmeii.cn/jnews/0814/622361.shtml
- http://wap.yidianmeii.cn/jnews/0814/29885.shtml
- http://3g.yidianmeii.cn/jnews/0814/56992.shtml
- http://h5.yidianmeii.cn/jnews/0814/2596365.shtml

## 项目结构

```
linkguardian/
├── cli.py                      # 命令行入口，聚合 probe/dashboard/compare 子命令
├── requirements.txt            # 生产环境依赖锁定（不含 dev 包）
├── setup.py                    # setuptools 配置，声明入口点与包元数据
├── data/                       # 外部数据挂载目录（不纳入版本控制）
│   ├── raw/                    # 原始导入的 .txt / .csv 文件，按批次命名
│   └── snapshots/              # 每日探测结果的 JSON 快照，用于历史比对
├── src/                        # 核心源码包
│   ├── __init__.py
│   ├── fetcher.py              # 异步与同步 HTTP 探测实现，含重定向策略
│   ├── parser.py               # URL 解析器，提取域名、路径、扩展名及查询参数
│   ├── storage.py              # SQLite 初始化、插入、更新及查询封装
│   ├── reporter.py             # 报表生成器，输出 Markdown 表格与摘要统计
│   ├── dashboard.py            # 静态 HTML 看板装配器，调用 matplotlib 绘图
│   └── scheduler.py            # 简易调度器（APScheduler 封装），定义每日 02:00 巡检
├── tests/                      # 单元测试与集成测试
│   ├── test_fetcher.py         # 模拟各类 HTTP 状态码的探测用例
│   ├── test_parser.py          # 边界 URL（含特殊字符、IP 直连）的解析覆盖
│   └── fixtures/               # 测试用样本数据（模拟响应体与头部）
├── docs/                       # 完整文档源文件
│   ├── quickstart.md
│   ├── operations.md
│   ├── development.md
│   └── api_reference.md
├── reports/                    # 默认输出目录，存放每次运行的 .md 报表
│   └── .gitkeep
├── templates/                  # Jinja2 模板文件
│   └── dashboard_template.html # 看板主样式与布局，内嵌 CSS 轻量化
└── .github/                    # 社区规范
    ├── ISSUE_TEMPLATE.md       # 缺陷报告与功能请求模板
    └── PULL_REQUEST_TEMPLATE.md
```

## 贡献指南

1. 查阅 issue 列表中的 `help wanted` 或 `good first issue` 标签，认领任务后在 issue 下回复“我将处理该任务”以避免重复工作。若为新功能提议，请先创建 feature request 并附上用例说明。

2. 派生本项目至个人账户，基于主分支 `main` 新建功能分支，分支命名遵循 `feat/描述` 或 `fix/描述`，例如 `feat/add-ftp-support`。

3. 开发前激活虚拟环境并执行 `pip install -e .[dev]` 安装可编辑模式及开发依赖（pytest, flake8, mypy）。所有新增或修改的代码需在 `tests/` 目录下补充对应测试用例，确保 `pytest --cov=src` 覆盖率不低于 80%。

4. 提交前运行 `flake8 src/ tests/` 和 `mypy src/` 进行静态检查，修复所有警告和类型错误。commit message 遵循 Conventional Commits 规范（`feat:`, `fix:`, `docs:`, `chore:` 等）。

5. 发起 Pull Request 至主分支，描述中需引用关联 issue，并附上手动测试结果（如命令行输出或报表截图）。至少一位 maintainer 审阅通过后方可合并。

## 常见问题

Q: 探测时大量链接返回 403 或 429，如何调整并发与重试策略？

A: 系统默认并发度为 20，重试 3 次（间隔 1s, 2s, 4s）。若目标站点对高频请求敏感，可在 `config.yaml` 中下调 `concurrency` 至 5，并开启 `random_delay` 选项（0.5~1.5s 随机间隔）。对于 429 响应，系统会自动进入指数退避重试，最长等待 30s。若持续失败，建议联系目标站点运维确认访问策略。

Q: 如何导入历史批次数据并进行跨批次对比？

A: 将历史批次的 URL 列表按同样格式存入 `data/raw/` 目录，例如 `batch_24_90.txt`，然后运行 `python cli.py compare --baseline data/raw/batch_24_90.txt --target data/raw/batch_25_90.txt`。系统会输出新增链接、失效链接以及状态码变化表。若需要与 SQLite 中的持久化快照比对，可指定 `--snapshot` 参数。

Q: 看板页面显示无图表，提示 matplotlib 未安装，是否影响核心功能？

A: 不影响。图表仅作为辅助可视化，看板的核心表格与状态摘要仍可正常渲染。若需图表支持，执行 `pip install matplotlib` 即可。若在无 GUI 环境（如纯命令行服务器）下运行，请预先设置 `MPLBACKEND=Agg` 环境变量，避免图形界面依赖报错。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
