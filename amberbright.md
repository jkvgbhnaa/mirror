# Yidian News Resource Aggregator

Yidian News Resource Aggregator is a structured external news link aggregation platform designed to collect, organize, and present distributed news content from multiple subdomains of the yidianmeii.cn network. This project serves as a centralized navigation and reference hub for journalists, content researchers, and news analysts who need to track and access news articles published across the yidianmeii.cn ecosystem on specific dates.

The aggregator provides a predictable URL mapping system, content metadata extraction utilities, and a lightweight static site generation workflow that transforms raw news link collections into browsable HTML indexes. It is not a web scraper nor a full-text search engine; rather, it is a curation-oriented link management toolkit that assumes the underlying news sources remain accessible and stable. The project targets users who require a reproducible, version-controlled record of news article URLs for archival, citation, or monitoring purposes.

## 功能概览

- **Multi-subdomain URL Normalization** – Automatically parses and normalizes news article URLs from 3g.yidianmeii.cn, wap.yidianmeii.cn, and h5.yidianmeii.cn into consistent relative path formats for deduplication and grouping.

- **Date-based Article Indexing** – Organizes all collected URLs by publication date using the YYYYMMDD pattern extracted from URL path segments, enabling chronological browsing and filtering.

- **Static HTML Manifest Generation** – Generates a single-page HTML manifest with all aggregated links rendered as a sortable, searchable table that includes subdomain origin, article ID, and access status.

- **Link Validity Health Check** – Provides a built-in HEAD request verification tool that tests each URL for HTTP 200 OK responses and flags broken or redirected links in the manifest.

- **Batch Import from Plain Text** – Supports importing URL lists from plain text files where each line contains one news article URL, with automatic validation against allowed yidianmeii.cn subdomains.

- **Export to JSON and CSV** – Exports the aggregated URL catalog in both JSON (structured metadata) and CSV (flat table) formats for integration with external data analysis pipelines.

- **Configurable Base Path Mapping** – Allows administrators to redefine the base domain for each subdomain via a configuration file, enabling the aggregator to work with mirror domains or staging environments.

- **Cron-ready Update Script** – Includes a shell script that can be scheduled via cron to periodically fetch fresh URL lists and regenerate the manifest without manual intervention.

## 应用场景

- **Newsroom Content Monitoring** – Editorial teams can use the aggregator to maintain a daily log of all news articles published across the yidianmeii.cn mobile and WAP subdomains, facilitating cross-team content review and duplication checks.

- **Academic Research and Citation Archiving** – Researchers studying Chinese digital news distribution can leverage the aggregator to build a reproducible URL corpus for longitudinal analysis of article volume, subdomain usage patterns, and content lifespan.

- **SEO and Traffic Referral Auditing** – Digital marketing professionals can feed the aggregated URL list into their own analytics or crawler systems to audit which articles receive referral traffic from yidianmeii.cn subdomains and identify high-value content clusters.

- **Internal Link Database Construction** – Organizations building internal knowledge bases can use the exported JSON output as a seed dataset for populating a dedicated news link table, with each URL tagged by subdomain and date for fast SQL queries.

- **Automated Broken Link Detection Pipeline** – DevOps teams can integrate the health check function into their monitoring stack to receive alerts when any yidianmeii.cn news article URL becomes inaccessible, enabling rapid manual verification and fallback content discovery.

## 快速开始

```bash
# Step 1: Clone the repository
git clone https://github.com/your-org/yidian-news-aggregator.git
cd yidian-news-aggregator

# Step 2: Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Step 3: Run the aggregation pipeline with a sample URL list
python aggregator.py --input samples/urls-20260814.txt --output manifests/ --format html,json,csv

# Alternative: Use the interactive shell for single URL addition
python cli.py add --url "http://3g.yidianmeii.cn/jnews/0814/383937.shtml"
python cli.py generate --output index.html
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 或更高 | 核心运行时，用于解析URL、生成manifest和执行健康检查 |
| requests | 2.28.0 或更高 | 用于发送HTTP HEAD请求以验证链接有效性 |
| click | 8.1.0 或更高 | CLI命令行交互框架，提供子命令解析和参数验证 |
| jinja2 | 3.1.0 或更高 | HTML模板引擎，用于渲染静态manifest页面 |
| pytest | 7.2.0 或更高 | 单元测试框架（仅开发环境需要） |
| black | 22.10.0 或更高 | 代码格式化工具（仅开发环境需要） |
| 操作系统 | Linux / macOS / Windows WSL2 | 支持主流POSIX环境，Windows原生未经过充分测试 |
| 磁盘空间 | 至少 50 MB | 用于存储URL缓存、manifest文件和日志 |
| 网络访问 | 出站HTTP/HTTPS可达 | 需要能够访问 yidianmeii.cn 域名下的所有子域名 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide.md | 如何安装聚合器、如何添加URL列表、如何生成并查看manifest文件、如何导出为不同格式 |
| 配置参考 | docs/configuration.md | 如何修改base_domain_mapping、如何调整健康检查超时和重试策略、如何自定义HTML模板 |
| 开发手册 | docs/development.md | 项目代码结构概览、如何添加新的URL解析器、如何扩展导出格式、如何运行测试套件 |
| 运维手册 | docs/operations.md | 如何使用systemd或cron设置定期更新、如何备份和恢复URL缓存数据库、如何迁移到新服务器 |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/383937.shtml
- http://3g.yidianmeii.cn/jnews/0814/980190.shtml
- http://wap.yidianmeii.cn/jnews/0814/0836.shtml
- http://h5.yidianmeii.cn/jnews/0814/5381.shtml
- http://h5.yidianmeii.cn/jnews/0814/711060.shtml
- http://3g.yidianmeii.cn/jnews/0814/7867.shtml
- http://wap.yidianmeii.cn/jnews/0814/971596.shtml
- http://wap.yidianmeii.cn/jnews/0814/03107.shtml
- http://3g.yidianmeii.cn/jnews/0814/0975698.shtml
- http://wap.yidianmeii.cn/jnews/0814/9156.shtml

## 项目结构

```
yidian-news-aggregator/
├── aggregator.py                 # 主入口脚本，协调URL导入、解析和manifest生成流程
├── cli.py                        # Click命令行接口实现，暴露add/generate/check/export子命令
├── requirements.txt              # 生产环境Python依赖列表（requests, click, jinja2）
├── dev-requirements.txt          # 开发环境额外依赖（pytest, black, mypy）
├── setup.py                      # setuptools打包配置，用于将项目安装为可编辑包
├── LICENSE                       # MIT许可证文件
├── README.md                     # 项目说明文档（即本文档）
├── .env.example                  # 环境变量示例，包含BASE_DOMAIN_MAPPING和TIMEOUT配置
│
├── src/                          # 核心源代码目录
│   ├── __init__.py               # 包初始化，导出Aggregator和UrlValidator类
│   ├── parser.py                 # URL解析器：提取子域名、日期、文章ID和扩展名
│   ├── validator.py              # 校验器：验证URL是否属于允许的子域名列表
│   ├── health.py                 # 健康检查器：并发HEAD请求检测链接可达性
│   ├── manifest.py               # Manifest生成器：构建内存中的URL索引结构
│   ├── exporters/                # 导出器子模块
│   │   ├── base.py               # 导出器抽象基类，定义export()接口
│   │   ├── html.py               # HTML导出器：渲染jinja2模板生成静态页面
│   │   ├── json.py               # JSON导出器：序列化完整元数据
│   │   └── csv.py                # CSV导出器：扁平化输出，适用于电子表格
│   └── templates/                # Jinja2 HTML模板目录
│       ├── base.html             # 基础布局模板，包含CSS样式和公共结构
│       └── manifest.html         # 主manifest页面模板，使用表格展示所有URL
│
├── tests/                        # 单元测试目录
│   ├── test_parser.py            # URL解析器测试用例（正常路径、边界、异常输入）
│   ├── test_validator.py         # 子域名白名单校验测试
│   ├── test_health.py            # 健康检查模拟测试（使用responses库模拟网络）
│   └── test_exporters.py         # 各导出器输出格式正确性测试
│
├── manifests/                    # 生成的manifest输出目录（默认）
│   ├── index.html                # 主HTML manifest文件
│   ├── catalog.json              # 完整URL元数据JSON导出
│   └── catalog.csv               # 扁平CSV导出
│
├── cache/                        # 本地缓存目录（存放最近导入的URL列表及时间戳）
│   ├── urls.db                   # SQLite轻量级数据库，存储URL历史记录
│   └── last_run.timestamp        # 上次成功运行的时间戳文件
│
├── samples/                      # 示例输入文件
│   └── urls-20260814.txt         # 包含10个yidianmeii.cn新闻URL的示例文本文件
│
├── scripts/                      # 运维辅助脚本
│   ├── update.sh                 # 拉取最新URL列表并重新生成所有manifest的shell脚本
│   ├── health_report.sh          # 生成当前所有链接健康状态摘要报告
│   └── migrate_cache.py          # 缓存数据库版本迁移工具
│
└── docs/                         # 详细文档目录（对应文档导航中的章节）
    ├── user-guide.md
    ├── configuration.md
    ├── development.md
    └── operations.md
```

## 贡献指南

1.  **Fork 仓库并创建特性分支** – 从主仓库 fork 到个人账户，然后基于 main 分支创建 feature/your-feature-name 分支，避免直接在 main 分支上提交。

2.  **编写或更新单元测试** – 所有新增功能或修复必须包含对应的测试用例，放置在 tests/ 目录下，确保测试覆盖率达到 80% 以上。运行 pytest 验证所有测试通过。

3.  **遵循代码风格规范** – 使用 black 和 isort 自动格式化 Python 代码，确保符合 PEP 8 标准。提交前运行 black . --check 和 flake8 进行静态检查。

4.  **更新文档和示例** – 如果修改了命令行接口、配置项或导出格式，必须同步更新 docs/ 下的用户文档和 samples/ 中的示例文件，确保新用户能够按文档快速上手。

5.  **提交 Pull Request 并描述变更** – 提交 PR 时使用项目提供的 PR 模板，清晰说明变更内容、影响范围、测试结果以及是否打破向后兼容性。至少需要一位维护者 approve 后方可合并。

## 常见问题

**Q: 为什么健康检查显示某些 URL 返回 404 或超时，但这些链接在浏览器中能够正常打开？**

A: 健康检查工具默认使用 HTTP HEAD 方法，某些新闻服务器可能不支持 HEAD 请求或对 HEAD 返回不同的状态码。此外，部分服务器会检查 User-Agent 和 Referer 头，可能会拒绝来自自动化工具的请求。建议通过配置文件中的 `check.method` 切换为 GET 请求，并设置 `check.user_agent` 为常见浏览器的 UA 字符串。如果仍存在问题，可以增加 `check.timeout` 值并启用 `check.follow_redirects` 选项。

**Q: 如何添加新的 yidianmeii.cn 子域名支持，例如 m.yidianmeii.cn？**

A: 在项目根目录的 `.env` 文件中修改 `BASE_DOMAIN_MAPPING` 环境变量，按 JSON 对象格式添加新的子域名键值对，例如 `{"3g": "3g.yidianmeii.cn", "wap": "wap.yidianmeii.cn", "h5": "h5.yidianmeii.cn", "m": "m.yidianmeii.cn"}`。修改后需要重启聚合器进程。如果希望永久生效，还需要在 `src/validator.py` 中的 `ALLOWED_SUBDOMAINS` 列表里添加新的子域名，并提交代码变更。

**Q: 生成的 HTML manifest 页面很大，加载缓慢，有什么优化建议？**

A: HTML 导出器默认将所有 URL 渲染为一个完整表格。如果 URL 数量超过 2000 条，建议启用分页功能（在 `config.yaml` 中设置 `html.pagination.enabled: true` 和 `html.pagination.per_page: 50`）。另外，可以禁用健康状态列的实时检查，改为使用缓存的健康结果（设置 `html.show_health_status: false`），这能显著减少页面体积和渲染时间。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
