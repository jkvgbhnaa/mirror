# NewsLink Aggregate Service

NewsLink Aggregate Service 是一个面向技术内容聚合与新闻外链统一管理的开源工具集。项目定位于为中小型内容运营团队、独立开发者以及技术资讯站点提供标准化的新闻数据源接入方案，通过统一的数据管道将分散在不同移动端子域下的新闻资源进行结构化整理与持久化存储。

本项目不提供新闻内容本身，而是聚焦于外链资源的采集、校验、分类和批量导出能力。目标用户包括：需要构建内部新闻监控系统的开发团队、希望快速搭建垂直领域资讯聚合站的产品经理、以及对特定新闻源进行长期存档分析的研究人员。通过本工具，用户可以将非结构化的新闻分发链接转化为可查询、可过滤、可定时更新的结构化数据集，显著降低人工整理外链的时间成本。

## 功能概览

- 批量链接抓取：支持从多个子域名批量拉取新闻链接，自动识别链接状态码与内容类型，过滤无效或已下架的资源。

- 结构化元数据提取：从每条新闻链接中自动解析发布时间、文章标题、来源子域名、路径层级等关键字段，生成标准化的 JSON 元数据记录。

- 去重与增量更新：基于链接 URL 和发布时间构建去重索引，仅对新增或变更的链接执行拉取操作，避免重复处理历史数据。

- 自定义过滤规则：允许用户通过正则表达式或关键字列表过滤不感兴趣的新闻分类，仅保留符合业务域的资源条目。

- 多格式导出支持：内置 CSV、JSON Lines、Markdown 表格三种导出格式，便于下游系统（如数据库、CMS、静态站点生成器）直接消费。

- 定时任务集成：提供基于 cron 表达式的调度接口，可与系统计划任务（如 crontab、systemd timer）配合实现自动化更新。

- 访问日志与监控：记录每次聚合任务的执行耗时、成功数、失败数及异常堆栈，支持接入 Prometheus 或自定义日志分析工具。

## 应用场景

- 内部新闻监控看板搭建：运维或市场团队可利用本工具每日定时拉取指定新闻源的最新链接，结合可视化面板（如 Grafana）实时展示热点趋势，无需手动浏览多个移动端页面。

- 垂直领域资讯聚合站点开发：开发者可基于导出的 JSON 数据快速构建一个按日期、分类、来源归类的技术新闻列表页，用于个人博客或团队知识库的内容补充。

- 新闻数据长期存档与分析：研究人员可配置增量更新策略，将历史链接及元数据持续追加至本地数据库，用于后续的传播路径分析、关键词频度统计或时间序列建模。

## 快速开始

```bash
# 1. 克隆项目仓库
git clone https://github.com/your-org/newslink-aggregate.git

# 2. 进入项目根目录
cd newslink-aggregate

# 3. 安装 Python 依赖（推荐使用虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 4. 运行示例聚合任务（使用内置测试数据）
python cli.py aggregate --source config/sources.example.yaml --output ./output
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 及以上 | 核心运行环境，低于此版本将无法解析类型注解及异步语法 |
| requests | 2.28.0 及以上 | 用于发起 HTTP 请求，处理重定向及超时重试逻辑 |
| pyyaml | 6.0 及以上 | 解析配置文件（YAML 格式），定义数据源及过滤规则 |
| click | 8.1.0 及以上 | 提供命令行交互接口，支持子命令分组与参数校验 |
| pytest | 7.0.0 及以上 | 仅开发测试时需要，用于执行单元测试与集成测试用例 |
| croniter | 1.3.0 及以上 | 用于解析和校验 cron 表达式，支撑定时调度模块 |
| lxml | 4.9.0 及以上 | 可选依赖，用于解析新闻页面中的结构化数据（如微格式） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user-guide.md | 如何配置数据源、设置过滤规则、执行聚合命令以及导出结果文件 |
| 开发指南 | docs/development.md | 如何扩展新的内容解析器、添加自定义导出格式或调整去重策略 |
| API 参考 | docs/api-reference.md | 各核心模块（fetcher、parser、deduplicator、exporter）的类与方法说明 |
| 部署运维 | docs/deployment.md | 如何以 systemd 服务或 Docker 容器方式长期运行，以及日志轮转配置 |

## 资源列表

- http://wap.yidianmeii.cn/jnews/0814/88035.shtml
- http://h5.yidianmeii.cn/jnews/0814/4881.shtml
- http://h5.yidianmeii.cn/jnews/0814/6477681.shtml
- http://3g.yidianmeii.cn/jnews/0814/2366.shtml
- http://3g.yidianmeii.cn/jnews/0814/87820.shtml
- http://wap.yidianmeii.cn/jnews/0814/0587.shtml
- http://h5.yidianmeii.cn/jnews/0814/940055.shtml
- http://wap.yidianmeii.cn/jnews/0814/87599.shtml
- http://wap.yidianmeii.cn/jnews/0814/3600.shtml
- http://3g.yidianmeii.cn/jnews/0814/191442.shtml

## 项目结构

```
newslink-aggregate/
├── cli.py                      # 命令行入口，注册 aggregate、export、validate 子命令
├── config/
│   ├── sources.example.yaml    # 示例数据源配置，包含子域名、路径前缀与更新频率
│   └── filters.example.yaml    # 示例过滤规则，展示关键字黑白名单用法
├── core/
│   ├── fetcher.py              # HTTP 请求模块，处理连接池、重试与 SSL 校验
│   ├── parser.py               # 元数据解析模块，从 URL 及页面标题提取结构化字段
│   ├── deduplicator.py         # 去重引擎，基于内存或 SQLite 持久化索引
│   └── exporter.py             # 导出模块，支持 CSV、JSONL、Markdown 三种格式
├── schedules/
│   ├── cron_loader.py          # 从 YAML 加载 cron 表达式并转换为调度任务
│   └── task_runner.py          # 调度执行器，调用核心模块完成一次完整聚合
├── tests/
│   ├── test_fetcher.py         # 模拟 HTTP 响应的单元测试
│   ├── test_parser.py          # 验证不同子域名路径下的解析正确性
│   └── test_deduplicator.py    # 去重逻辑的边界条件测试（空集、重复、过期）
├── output/                     # 默认导出目录，按日期自动生成子文件夹
├── logs/                       # 运行日志目录，按天轮转，保留最近 30 天
├── requirements.txt            # 生产环境依赖列表
├── requirements-dev.txt        # 开发环境额外依赖（pytest、black、mypy）
└── README.md                   # 本文件
```

## 贡献指南

1. 阅读开发指南文档 docs/development.md，了解项目整体架构、代码风格规范以及测试要求。

2. 从 Issues 列表中选择标注为 good-first-issue 或 help-wanted 的任务，在 Issue 下回复确认认领，避免多人重复工作。

3. 创建以 feature/ 或 fix/ 为前缀的分支，例如 feature/add-rss-exporter，在该分支上完成代码实现及对应的单元测试。

4. 提交前执行 make lint 和 make test 确保代码格式通过静态检查且所有测试用例保持通过状态。

5. 发起 Pull Request 到 main 分支，在 PR 描述中关联对应的 Issue 编号，并简要说明改动点与测试覆盖情况。

## 常见问题

Q: 聚合任务执行时出现大量 HTTP 超时错误，如何调整超时参数？

A: 您可以在 config/sources.yaml 中对每个数据源单独设置 timeout 字段（单位秒），或在全局配置文件 config/app.yaml 中设置 default_timeout。建议根据目标服务器的响应速度将值设置在 10 到 30 秒之间。若仍频繁超时，请检查网络环境或调整 fetcher 模块中的重试次数参数。

Q: 导出的 JSON 文件中包含重复的链接条目，但去重功能已开启，如何排查？

A: 请确认 deduplicator 模块使用的索引存储是否持久化。默认使用内存索引，每次重启 cli 进程后索引会重置。若需跨任务去重，请修改 deduplicator.py 中的存储后端为 SQLite，并指定数据库文件路径。同时检查不同子域名的相同文章是否使用了不同的 URL 参数（如 utm_source），可配置规范化规则去除无关查询参数。

Q: 能否只拉取特定时间范围内发布的新闻，而忽略更早的历史链接？

A: 可以。在 sources.yaml 中为数据源添加 since 字段，支持绝对日期（如 2026-08-01）或相对天数（如 -7d）。parser 模块会根据 URL 路径中的日期段或页面响应头中的 Last-Modified 进行过滤，仅保留符合时间窗口的链接。若路径中不含日期信息，则可启用 html_parse 模式从页面内容中提取发布时间。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
