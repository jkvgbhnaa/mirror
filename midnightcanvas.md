# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, community-driven information indexing platform designed to catalog and organize distributed web content across multiple subdomains and deep-link structures. The project targets developers, data analysts, and content researchers who need a stable, queryable index over heterogeneous content sources that are otherwise difficult to discover or track due to inconsistent URL schemas, fragmented publication patterns, or missing cross-references.

Unlike traditional bookmark managers or web scrapers, Yidian Resource Aggregator does not store or replicate original content. Instead, it maintains a curated index table that normalizes source metadata, URL patterns, content type heuristics, and update timestamps. The aggregator provides a unified query interface, batch export capabilities, and extensible parser hooks to handle variations in DOM structures and semantic fields across different subdomain endpoints. The project is not a search engine, nor a crawler framework; it is a structured reference layer that helps users maintain situational awareness over a known set of information sources.

## 功能概览

- **Multi-Subdomain Source Normalization** – Automatically detects and normalizes URL variants across wap, h5, and 3g subdomains, extracting content identifiers and type flags from path segments.

- **Semantic Field Extraction** – Parses title, publication date, content category, and author information from HTML meta tags and structured data blocks using configurable XPath and CSS selector rules.

- **Index Snapshot Management** – Creates timestamped snapshots of the index state, allowing users to compare changes in source availability, content drift, and structural modifications over time.

- **Batch Query DSL** – Provides a domain-specific query language for filtering index records by subdomain, content type, date range, and identifier pattern, with output formatting in JSON, CSV, and plain text.

- **Extensible Parser Pipeline** – Supports user-defined parser modules through a hook-based architecture, enabling custom processing for non-standard content layouts without modifying core code.

- **Integrity Verification Mode** – Performs periodic HEAD and GET checks on indexed URLs, recording HTTP status codes and response time metrics to detect broken links or content redirection.

- **Export and Sync Adapters** – Includes adapters for exporting index data to external systems such as Elasticsearch, SQLite, and static site generators, with incremental update support.

- **Minimal Configuration Overhead** – Operates with a single YAML configuration file and zero runtime database dependencies, using file-based JSON storage for portability and version control compatibility.

## 应用场景

**Content Audit for Multi-Source News Aggregators** – A team managing a daily news digest needs to verify the availability and topic distribution of content from multiple subdomains. The aggregator provides a normalized index and status check reports to support editorial planning and source quality assessment.

**Historical Link Recovery for Research Projects** – A research group studying online discourse patterns has collected thousands of deep-link URLs over several years. Using the aggregator, they can map old identifiers to current content paths, detect moved resources, and filter out permanently inaccessible entries before analysis.

**Quality Assurance for Frontend Integration** – A frontend engineering team integrates third-party content modules from heterogeneous endpoints. The aggregator serves as a staging validation tool, allowing them to test URL patterns, content type expectations, and parsing rules against a controlled index before deploying to production.

**Personal Knowledge Base Curation** – An independent researcher maintains a curated collection of reference articles scattered across different subdomain versions. The aggregator helps them assign tags, track publication dates, and export structured bibliographic entries without manually editing spreadsheets.

**Monitoring for Domain Migration** – During a domain consolidation project, operators need to monitor traffic redirection and content duplication across legacy and active subdomains. The aggregator logs redirection chains and highlights conflicts based on content identifier overlaps.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/yidian-dev/resource-aggregator.git
cd resource-aggregator

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Copy example configuration and adjust source list
cp config/example.yaml config/production.yaml
vim config/production.yaml

# Run the indexer with default settings
python -m aggregator.cli --config config/production.yaml --action full-index

# Start the query interface
python -m aggregator.server --port 8080
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 核心运行时，类型注解使用 PEP 585 泛型语法 |
| pip | 21.0+ | 包管理工具，用于安装 requirements 中的依赖项 |
| lxml | 4.9.0+ | HTML/XML 解析引擎，提供高性能 XPath 和 CSS 选择器支持 |
| requests | 2.28.0+ | HTTP 客户端库，用于资源可用性探测和响应头分析 |
| pyyaml | 6.0+ | YAML 配置解析器，用于读取用户自定义配置和源映射表 |
| pytest | 7.0+ | 仅开发环境依赖，用于单元测试和集成测试套件 |
| black | 22.0+ | 仅开发环境依赖，代码格式化工具，保持代码风格一致 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user/quick-start.md | 如何安装、配置、运行聚合器并执行首次索引构建 |
| 用户手册 | docs/user/query-syntax.md | 如何使用 DSL 进行字段过滤、排序和导出格式控制 |
| 开发者指南 | docs/dev/parser-api.md | 如何编写自定义解析器模块并注册到管道中 |
| 开发者指南 | docs/dev/snapshot-format.md | 索引快照的 JSON 架构、版本演进和兼容性策略 |
| 运维参考 | docs/ops/monitoring.md | 如何配置健康检查、日志轮转和性能计数器 |
| 运维参考 | docs/ops/migration.md | 在子域名变更或路径重写时如何迁移现有索引数据 |

## 资源列表

- http://wap.yidianmeii.cn/snews/86001.shtml
- http://h5.yidianmeii.cn/snews/691871.shtml
- http://wap.yidianmeii.cn/snews/8116.shtml
- http://wap.yidianmeii.cn/snews/6695.shtml
- http://3g.yidianmeii.cn/snews/7191.shtml
- http://3g.yidianmeii.cn/snews/15686.shtml
- http://h5.yidianmeii.cn/snews/830398.shtml
- http://wap.yidianmeii.cn/snews/31161.shtml
- http://wap.yidianmeii.cn/snews/0196.shtml
- http://h5.yidianmeii.cn/snews/55136.shtml

## 项目结构

```
resource-aggregator/
├── aggregator/                         # 主包目录
│   ├── __init__.py                     # 包初始化，暴露顶层 API
│   ├── cli/                            # 命令行接口子包
│   │   ├── __init__.py
│   │   ├── main.py                     # 主入口路由，解析子命令
│   │   ├── index_cmd.py                # full-index / incremental-index 命令实现
│   │   └── query_cmd.py                # query / export 命令实现
│   ├── core/                           # 核心数据模型和抽象类
│   │   ├── __init__.py
│   │   ├── models.py                   # SourceRecord, Snapshot, ParserResult 等数据类
│   │   ├── exceptions.py               # 自定义异常层级 (ParseError, NetworkError)
│   │   └── registry.py                 # 解析器注册表和类型工厂
│   ├── parsers/                        # 内置解析器实现
│   │   ├── __init__.py
│   │   ├── base.py                     # BaseParser 抽象基类，定义 parse() 契约
│   │   ├── yidian_news.py              # 针对 yidianmeii.cn 子域名的专用解析器
│   │   └── generic_html.py             # 通用后备解析器，基于常见 meta 标签
│   ├── storage/                        # 存储层
│   │   ├── __init__.py
│   │   ├── index_store.py              # 索引快照的读写、合并与版本化
│   │   └── serializers.py              # JSON / CSV / YAML 序列化器
│   ├── network/                        # 网络请求和探测
│   │   ├── __init__.py
│   │   ├── fetcher.py                  # 异步/同步 HTTP 获取器，含重试和超时策略
│   │   └── verifier.py                 # HEAD 检查、状态码统计和重定向跟踪
│   └── server/                         # 轻量级 Web 查询接口
│       ├── __init__.py
│       ├── app.py                      # Flask/FastAPI 应用工厂
│       └── routes.py                   # /query, /status, /export 端点定义
├── config/                             # 配置文件目录
│   ├── example.yaml                    # 示例配置，含源列表和解析器映射
│   └── schema.json                     # JSON Schema 用于校验用户配置
├── tests/                              # 测试套件
│   ├── unit/                           # 单元测试，覆盖模型、解析器和工具函数
│   ├── integration/                    # 集成测试，需要网络访问和真实源探测
│   └── fixtures/                       # 测试用的静态 HTML 样本和预期输出
├── docs/                               # 用户文档和开发者文档
│   ├── user/                           # 面向最终用户的操作指南
│   └── dev/                            # 面向贡献者的 API 设计和设计决策说明
├── scripts/                            # 辅助脚本
│   ├── bootstrap.sh                    # 初始化开发环境，安装 pre-commit hooks
│   └── validate_sources.py             # 独立脚本，验证资源列表的 HTTP 可达性
├── requirements.txt                    # 生产依赖列表
├── requirements-dev.txt                # 开发额外依赖 (pytest, black, mypy)
├── pyproject.toml                      # 项目元数据，构建配置和工具设置
├── Makefile                            # 常用任务快捷命令 (make test, make run)
└── README.md                           # 本文件
```

## 贡献指南

1. 阅读开发者文档 docs/dev/parser-api.md 和 docs/dev/snapshot-format.md 以了解内部架构设计及数据契约。所有新增解析器必须继承 BaseParser 并实现 parse 方法，返回类型为 ParserResult 数据类。

2. 在 GitHub 上 fork 主仓库并创建功能分支，分支命名遵循 feature/ 或 fix/ 前缀加简短描述，例如 feature/add-weibo-parser。确保分支基于最新的 main 分支。

3. 编写或更新单元测试，覆盖新增代码的所有逻辑分支。对于解析器改动，必须在 tests/fixtures 中添加对应的样本 HTML 文件，并在测试中引用。运行 make test 确保全部测试通过且覆盖率不低于 85%。

4. 提交代码前运行 make format 和 make lint 以应用 black 格式化并执行 mypy 静态类型检查。提交信息使用约定式提交格式，如 feat(parser): add weibo mobile parser support。

5. 发起 pull request 至 main 分支，描述改动内容、影响范围以及测试结果。维护者将在 5 个工作日内审核。对于重大架构变更，需提前在 issue 中讨论并获得共识。

## 常见问题

**问：聚合器是否会缓存或存储原始 HTML 内容？**

答：不会。聚合器仅存储从内容中提取的元数据字段（标题、日期、类别等）以及 URL 索引记录。原始 HTML 内容仅在请求验证或解析期间驻留于内存，不写入磁盘。用户可以配置 cache 目录用于临时网络请求缓存，但默认关闭。

**问：如何处理子域名响应结构不一致的情况？**

答：聚合器采用分层解析策略。首先尝试使用针对特定子域名注册的专用解析器（如 yidian_news.py 中定义的规则）。如果专用解析失败，则回退到 generic_html 解析器，该解析器基于常见的 meta property="og:title"、meta name="pubdate" 等标准标签进行提取。用户也可通过配置文件为特定 URL 模式绑定自定义解析器类。

**问：索引快照支持哪些版本控制操作？**

答：每个快照均包含 manifest.json 记录版本号、创建时间和源列表哈希。支持 diff 比较两个快照之间新增、删除和修改的记录项。用户可通过 CLI 的 snapshot compare 命令查看差异。存储层保留最近 30 个快照，自动清理超过阈值的旧快照，清理策略可在配置中调整。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
