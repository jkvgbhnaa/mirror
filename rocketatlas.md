# Yidianmeii News Aggregator

Yidianmeii News Aggregator is a lightweight, open-source information aggregation toolkit designed to collect, normalize, and redistribute news content from distributed mobile gateways. The project targets developers, data analysts, and content researchers who need to systematically access region-specific news feeds without relying on proprietary SDKs or closed-source scrapers. By providing a unified interface over heterogeneous origin endpoints, the aggregator reduces integration friction and enables reproducible data collection workflows.

The system operates as a URL-centric metadata harvester that extracts structured fields—title, publish timestamp, section classification, and raw HTML fingerprints—from semi-structured mobile news pages. It is not a full-text search engine nor a replacement for official news APIs; rather, it serves as a deterministic fetch-and-parse layer that turns volatile mobile pages into stable JSON lines suitable for downstream ETL pipelines. All core logic is implemented in portable Python with minimal external dependencies, ensuring compatibility across Linux, macOS, and Windows-based deployment environments.

## 功能概览

- **Multi-origin HTTP fetcher** – Concurrently requests pages from distinct subdomains (3g, h5, wap) with configurable timeout, retry, and user-agent rotation policies.

- **Adaptive HTML normalizer** – Strips inline scripts, comments, and attribution noise while preserving article body, heading, and metadata containers using XPath and CSS selector fallbacks.

- **Timestamp extraction heuristics** – Parses publication dates from multiple Chinese date formats (YYYY年MM月DD日, MM-DD, relative strings) and normalizes to ISO-8601.

- **Deduplication by content fingerprint** – Computes Simhash-based signatures for each article to filter near-duplicates across different mobile entry points within the same batch.

- **Export formatters** – Supports JSONL, CSV, and SQLite output schemas, with optional field projection for lightweight logging or full-payload archiving.

- **Batch job scheduler** – Built-in cron-like runner that executes collection cycles at fixed intervals and writes rotation-aware log files.

- **Health probe endpoint** – Exposes a simple /status endpoint when run in daemon mode, reporting last successful fetch count and average latency per origin.

## 应用场景

1. **Regional news trend monitoring** – Researchers can schedule hourly collection jobs to track how specific topics evolve across different mobile gateway versions, observing differences in headline phrasing and editorial emphasis between 3g, h5, and wap channels.

2. **Data pipeline seeding** – Data engineers can use the aggregator as a source connector in Airflow or Prefect, feeding raw article metadata into a centralized data lake for further NLP processing, sentiment analysis, or named-entity recognition.

3. **Content availability auditing** – Site reliability teams can deploy the tool as a synthetic monitoring probe to verify that critical news paths remain accessible from various mobile user-agent profiles, detecting 4xx or 5xx responses before they affect end-user experience.

4. **Historical archive construction** – Archivists can run backfill jobs over date-ranged URL patterns to build a time-series corpus of mobile news snapshots, preserving content that may be subject to rotation or deletion after 72 hours.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/your-org/yidianmeii-aggregator.git
cd yidianmeii-aggregator

# Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the default collection job (fetches all URLs from the resource list)
python run_aggregator.py --source resources.txt --output data/out.jsonl

# Or run with built-in scheduler (every 30 minutes)
python run_aggregator.py --daemon --interval 30
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.8 或更高 | 核心运行时，类型注解依赖 3.8+ 特性 |
| requests | 2.28.0 以上 | HTTP 会话管理与连接池复用 |
| lxml | 4.9.0 以上 | 高性能 HTML/XML 解析器，用于 XPath 求值 |
| simhash | 2.1.0 以上 | 用于生成 64 位内容指纹，实现去重 |
| tzlocal | 4.0 以上 | 将 UTC 时间戳转换为本地时区，用于日志时间戳 |
| pytest | 7.0.0 以上 | 仅开发环境需要，用于运行单元测试和集成测试 |
| black | 22.0.0 以上 | 仅开发环境需要，代码格式化工具 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user_guide.md | 如何配置源文件、调整并发数、选择输出格式以及解读运行日志？ |
| 架构设计 | docs/architecture.md | 各模块（fetcher, parser, deduper, exporter）如何协作？状态机设计是怎样的？ |
| API 参考 | docs/api_reference.md | 哪些函数是可外部调用的？参数类型和返回值结构分别是什么？ |
| 部署指南 | docs/deployment.md | 如何通过 systemd 或 Docker 将聚合器部署为常驻服务？环境变量如何注入？ |
| 故障排查 | docs/troubleshooting.md | 常见 HTTP 错误（429, 503）、解析空内容和指纹碰撞问题如何定位和修复？ |
| 性能调优 | docs/performance.md | 如何根据 CPU 核数和内存限制调整工作线程数、缓存大小和 GC 参数？ |

## 资源列表

- http://3g.yidianmeii.cn/vnews/0814/3257650.shtml
- http://3g.yidianmeii.cn/vnews/0814/3711.shtml
- http://h5.yidianmeii.cn/vnews/0814/4944.shtml
- http://wap.yidianmeii.cn/vnews/0814/69731.shtml
- http://3g.yidianmeii.cn/vnews/0814/26280.shtml
- http://wap.yidianmeii.cn/vnews/0814/639139.shtml
- http://h5.yidianmeii.cn/vnews/0814/66244.shtml
- http://wap.yidianmeii.cn/vnews/0814/660038.shtml
- http://h5.yidianmeii.cn/vnews/0814/8054.shtml
- http://3g.yidianmeii.cn/vnews/0814/922224.shtml

## 项目结构

```
yidianmeii-aggregator/
├── run_aggregator.py          # 入口脚本，解析命令行参数并启动调度器或单次运行
├── requirements.txt           # 生产环境依赖锁定文件
├── dev-requirements.txt       # 开发环境额外依赖（pytest, black, mypy）
├── src/
│   ├── __init__.py            # 包初始化，导出主要类 Fetcher, Parser, Deduper
│   ├── fetcher.py             # 异步并发请求器，包含重试策略和 User-Agent 轮换
│   ├── parser.py              # 基于 lxml 的 HTML 解析器，提取标题、时间、正文片段
│   ├── deduper.py             # Simhash 指纹计算与 Hamming 距离去重引擎
│   ├── exporter.py            # JSONL / CSV / SQLite 输出格式化器
│   └── scheduler.py           # 基于 schedule 库的周期任务管理，支持暂停和恢复
├── tests/
│   ├── test_fetcher.py        # 模拟 HTTP 响应的单元测试，覆盖超时和 5xx 重试
│   ├── test_parser.py         # 使用固定 HTML 样本验证 XPath 选择器正确性
│   └── test_deduper.py        # 验证相似文章间的 Simhash 碰撞检测阈值
├── config/
│   ├── default.yaml           # 默认配置：并发数 8，超时 15s，指纹长度 64
│   └── production.yaml        # 生产配置：并发数 32，启用压缩日志和循环缓冲区
├── logs/                      # 运行时日志存储目录，按日期轮转，保留最近 7 天
├── data/                      # 默认输出目录，存放 JSONL 和 CSV 结果文件
├── docs/                      # 完整文档目录，包含架构图、API 参考和部署示例
│   ├── user_guide.md
│   ├── architecture.md
│   ├── api_reference.md
│   ├── deployment.md
│   ├── troubleshooting.md
│   └── performance.md
└── LICENSE                    # MIT 许可证文本
```

## 贡献指南

1. 阅读 docs/architecture.md 和 docs/api_reference.md 了解核心数据流和接口契约，确保更改不会破坏现有导出格式的兼容性。

2. 在 issue 列表中认领或提交一个新的 issue 描述你计划修复的问题或新增的功能，等待维护者确认方向以避免重复劳动。

3. 派生仓库并创建功能分支，分支命名遵循 `feature/功能名称` 或 `fix/问题编号` 格式，提交信息使用约定式提交规范（feat, fix, docs, test）。

4. 编写对应的单元测试和集成测试，确保测试覆盖率不低于 85%，并在提交前运行 `pytest tests/` 验证本地所有测试通过。

5. 提交 pull request 时附带详细的变更说明，包括影响范围、性能影响以及手动测试步骤，等待至少一位维护者审阅后合并。

## 常见问题

**问：为什么某些 URL 返回 403 或 404 错误，但直接在浏览器中访问是正常的？**

答：部分移动网关会检查请求头中的 User-Agent 和 Accept-Language。请检查 config/default.yaml 中的 user_agent_list 配置项，确保包含常见的移动端 UA（如 iPhone、Android）。同时，确认 URL 中的日期路径（如 /0814/）是否已经过期——部分旧链接在 48 小时后会被服务器移除。你可以在 fetcher.py 中启用 `--verbose` 标志查看详细请求和响应头信息。

**问：采集结果中出现大量重复文章，即使 deduper 已经启用，如何调整去重敏感度？**

答：默认的 Simhash 汉明距离阈值为 3，即差异少于 3 位即判定为重复。如果误判过多，可以在 config/default.yaml 中调低 `deduper.threshold` 至 2 或 1；如果漏判严重，则可以提高到 4 或 5。建议先运行 `scripts/analyze_dedup.py --sample 100` 对一批数据进行分布分析，再选择合适的阈值。同时，注意 parser 模块中正文提取的长度——过短的正文片段（少于 100 字符）会降低指纹质量，可调整 `min_content_length` 参数过滤噪声。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
