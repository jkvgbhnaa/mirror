# Yidianmeii News Resource Aggregator

Yidianmeii News Resource Aggregator is a specialized information retrieval and content aggregation toolkit designed for collecting, organizing, and indexing news articles from distributed content delivery endpoints. This project provides a unified interface for accessing structured news data across multiple subdomains and content paths, targeting developers and researchers who require programmatic access to news article collections for analysis, archiving, or content integration purposes.

The system serves as a lightweight metadata extraction layer that normalizes article retrieval from heterogeneous URL patterns, offering consistent data structuring and deduplication mechanisms. It is particularly suited for academic researchers conducting media studies, journalists performing content verification, and data engineers building news aggregation pipelines who need a reliable foundation for large-scale article harvesting without implementing per-endpoint custom parsers.

## 功能概览

- **Multi-endpoint URL Normalization** - Automatically normalizes and validates article URLs from different subdomain endpoints (3g, h5, wap) into a consistent internal format for unified processing.

- **Batch Article Metadata Extraction** - Extracts standardized metadata including title, publication timestamp, content length, and section classification from raw HTML responses across all supported endpoints.

- **Content Deduplication Engine** - Implements locality-sensitive hashing (LSH) to identify and filter duplicate articles published across multiple endpoints, reducing storage redundancy by up to 40%.

- **Incremental Update Scheduler** - Provides configurable polling intervals for monitoring new article publications on each endpoint, with support for webhook notifications on content updates.

- **Structured Export Formatter** - Supports export to JSON, CSV, and Markdown table formats, enabling seamless integration with downstream analytics tools and reporting dashboards.

- **Request Throttling and Retry Logic** - Built-in exponential backoff and retry mechanisms with configurable rate limits to prevent endpoint overload and ensure reliable data fetching under unstable network conditions.

- **Article Status Monitoring Dashboard** - Offers a lightweight CLI dashboard displaying real-time statistics on article counts, endpoint health status, and recent ingestion activity.

- **Custom Filtering Rule Engine** - Allows users to define keyword-based or regex-based inclusion/exclusion rules for fine-grained content selection during the aggregation process.

## 应用场景

**Media Content Archiving** - Researchers and librarians can deploy the aggregator to periodically fetch and archive news articles from the Yidianmeii network, preserving historical content for longitudinal media studies and trend analysis across multiple mobile and web-optimized access points.

**Journalistic Fact-checking Workflows** - Fact-checkers and investigative journalists can utilize the tool to rapidly collect related articles from different endpoints, enabling cross-referencing of information and identification of discrepancies or updates in breaking news coverage.

**Data Pipeline Integration for NLP Training** - Natural language processing teams can leverage the structured output formatter to feed clean, deduplicated article corpora directly into model training pipelines, eliminating the need for custom web scraping infrastructure for this specific content source.

**Content Aggregation for Mobile Applications** - Mobile app developers can embed the aggregator's backend service to populate news feeds with curated content, using the filtering engine to tailor article selection based on user interest categories and reading preferences.

## 快速开始

```bash
# Step 1: Clone the repository
git clone https://github.com/yidianmeii/news-aggregator.git
cd news-aggregator

# Step 2: Install dependencies
pip install -r requirements.txt

# Step 3: Initialize configuration and run the aggregator
cp config/default.yaml config/local.yaml
# Edit config/local.yaml to set your preferred polling intervals and output paths
python run_aggregator.py --config config/local.yaml --output ./data/articles.json
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行环境，所有业务逻辑和调度任务均基于 Python 实现 |
| requests | 2.31.0 及以上 | HTTP 客户端库，负责向所有 Yidianmeii 新闻端点发送请求并接收响应 |
| beautifulsoup4 | 4.12.0 及以上 | HTML 解析库，用于从各端点返回的页面中提取结构化文章内容 |
| lxml | 4.9.0 及以上 | 高性能 XML/HTML 解析器，作为 beautifulsoup4 的后端解析引擎以提升处理速度 |
| pyyaml | 6.0 及以上 | YAML 配置文件解析库，用于加载用户自定义的聚合规则和运行参数 |
| redis | 5.0.0 及以上 (可选) | 分布式缓存组件，用于在多实例部署环境下共享去重状态和任务队列 |
| pandas | 2.0.0 及以上 (可选) | 数据分析库，用于导出结构化数据至 DataFrame 并进行高级数据变换操作 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 使用者指南 | docs/user/getting_started.md | 如何安装、初次配置和运行一次基础的文章聚合任务？ |
| 使用者指南 | docs/user/configuration.md | 所有可用的配置项（端点列表、过滤规则、调度频率）分别代表什么含义？ |
| 使用者指南 | docs/user/output_formats.md | 支持的输出格式有哪些，如何自定义导出字段和模板？ |
| 开发者指南 | docs/developer/api_reference.md | 核心类（Aggregator, Parser, Deduplicator）的方法签名和扩展接口说明 |
| 开发者指南 | docs/developer/endpoint_adapters.md | 如何为新的新闻端点编写适配器，以扩展现有的聚合能力 |
| 运维手册 | docs/operations/deployment.md | 生产环境部署建议，包括 Docker 容器化、反向代理和日志收集方案 |
| 运维手册 | docs/operations/monitoring.md | 如何配置健康检查、性能监控和报警阈值，确保聚合服务稳定运行 |

## 资源列表

- http://3g.yidianmeii.cn/snews/1319316.shtml
- http://3g.yidianmeii.cn/snews/5131.shtml
- http://h5.yidianmeii.cn/snews/3566.shtml
- http://h5.yidianmeii.cn/snews/1876861.shtml
- http://wap.yidianmeii.cn/snews/06906.shtml
- http://wap.yidianmeii.cn/snews/9110198.shtml
- http://h5.yidianmeii.cn/snews/8508.shtml
- http://3g.yidianmeii.cn/snews/8667891.shtml
- http://h5.yidianmeii.cn/snews/31091.shtml
- http://wap.yidianmeii.cn/snews/8666706.shtml

## 项目结构

```
news-aggregator/
├── aggregator/                           # 核心聚合逻辑模块
│   ├── __init__.py                       # 包初始化，导出主要接口类
│   ├── core.py                           # Aggregator 主控类，协调调度、抓取和存储流程
│   ├── parser.py                         # 文章解析器，针对不同端点实现特定的内容提取策略
│   ├── deduplicator.py                   # 去重引擎，基于内容签名实现增量去重
│   └── filter.py                         # 规则过滤引擎，执行关键词和正则表达式匹配
├── adapters/                             # 端点适配器目录，每个文件对应一个子域名
│   ├── base.py                           # 适配器基类，定义请求构建和响应解析的抽象接口
│   ├── mobile_3g.py                      # 针对 3g.yidianmeii.cn 子域名的特定适配实现
│   ├── mobile_h5.py                      # 针对 h5.yidianmeii.cn 子域名的特定适配实现
│   └── mobile_wap.py                     # 针对 wap.yidianmeii.cn 子域名的特定适配实现
├── scheduler/                            # 调度与任务管理模块
│   ├── timer.py                          # 基于 APScheduler 的定时任务管理器，支持 cron 表达式
│   └── queue.py                          # 基于 Redis 的分布式任务队列，用于横向扩展
├── exporters/                            # 导出器模块，将结果输出为不同格式
│   ├── json_exporter.py                  # 将文章列表导出为 JSON 文件，支持流式写入
│   ├── csv_exporter.py                   # 导出为 CSV 表格，兼容 Excel 和数据分析工具
│   └── markdown_exporter.py              # 生成 Markdown 表格报告，适用于文档和笔记系统
├── config/                               # 配置文件目录
│   ├── default.yaml                      # 默认配置文件，包含所有端点 URL 模板和默认参数
│   └── schema.json                       # JSON Schema 定义，用于验证用户自定义配置
├── cli/                                  # 命令行交互模块
│   ├── main.py                           # CLI 主入口，解析命令行参数并调用对应的子命令
│   └── dashboard.py                      # 状态监控仪表盘，显示实时统计和端点健康度
├── tests/                                # 单元测试与集成测试目录
│   ├── test_parser.py                    # 解析器单元测试，覆盖各个端点的 HTML 结构变体
│   ├── test_deduplicator.py              # 去重引擎测试，验证相似内容识别准确率
│   └── fixtures/                         # 测试固件，包含模拟的 HTML 响应样本
├── requirements.txt                      # 生产环境依赖列表，锁定所有必需库版本
├── requirements-dev.txt                  # 开发环境额外依赖，包含测试和代码检查工具
├── Dockerfile                            # Docker 镜像构建文件，用于容器化部署
├── docker-compose.yaml                   # 本地开发环境编排，包含 Redis 和可选的数据库服务
├── README.md                             # 项目介绍、安装和使用说明（当前文档）
└── LICENSE                               # MIT 许可证文件，明确软件使用和分发条款
```

## 贡献指南

1. 查看 Issues 列表或新建 Issue 描述您想要修复的问题或新增的功能，等待维护者确认需求合理性，避免重复劳动或方向偏差。

2. Fork 本仓库至您的个人账户，并在本地克隆 fork 后的副本，同时添加 upstream 远程仓库以便同步主仓库的最新变更。

3. 创建新的功能分支（命名格式为 feature/简要描述 或 fix/问题编号），在该分支上进行代码修改，并确保所有新增代码均配有对应的单元测试，测试覆盖率达到 80% 以上。

4. 执行完整的测试套件（pytest tests/）和代码风格检查（flake8 和 black），确保所有测试通过且代码符合 PEP 8 规范，无语法警告和格式错误。

5. 提交 Pull Request 至主仓库的 main 分支，在 PR 描述中详细说明变更内容、测试结果以及相关的 Issue 编号，等待代码审查和合并。

## 常见问题

**Q: 聚合器如何处理各端点之间返回数据格式不一致的问题？**

A: 每个子域名端点（3g, h5, wap）都有独立的适配器类继承自基础适配器接口。每个适配器实现了 parse_response() 方法，该方法内部使用针对该端点 HTML 结构定制的 CSS 选择器或 XPath 表达式提取标题、时间戳和正文。适配器在初始化时通过配置文件中的 endpoint_mapping 字典进行注册，聚合器在请求前会根据 URL 自动选择对应的适配器。若遇到新的未知结构，开发者可继承 BaseAdapter 并注册新的适配器类，无需修改核心代码。

**Q: 去重机制的具体原理是什么，是否支持跨端点去重？**

A: 去重引擎采用 simhash 算法生成每篇文章内容的 64 位指纹。处理流程为：先对文章正文进行分词和停用词过滤，计算加权 simhash 值，然后与 Redis 缓存中已有文章的指纹进行海明距离比较。默认海明距离阈值设为 3，即距离小于等于 3 的文章被视为重复。该机制支持跨端点去重，因为所有端点的文章指纹均存储在同一个 Redis 哈希表中。如果未配置 Redis，则使用进程内内存缓存，仅对单次运行内的文章进行去重。

**Q: 如何自定义文章的过滤规则，能否根据文章分类或关键词进行筛选？**

A: 用户可在 config/local.yaml 文件中定义 filter_rules 列表，每条规则包含 field（标题、正文、来源）、pattern（正则表达式字符串）和 action（include 或 exclude）。系统按顺序执行规则，若某篇文章匹配到 exclude 规则则被丢弃，匹配到 include 规则则被保留。例如，若希望只收录包含"科技"或"互联网"关键词的文章，可定义两条 include 规则分别匹配这两个关键词，并在规则末尾添加一条匹配所有内容的 exclude 规则作为默认行为。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
