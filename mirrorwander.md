# Yidianmeii Resource Aggregator

Yidianmeii Resource Aggregator is a specialized technical documentation and news resource indexing system designed for developers, technical researchers, and content curators who need to systematically organize and access distributed web content. The project addresses the fundamental challenge of managing fragmented information sources across multiple subdomains and content paths, providing a structured metadata layer over raw HTML resources.

The system operates as a lightweight resource discovery and navigation layer, ingesting URL patterns from the yidianmeii.cn domain family and presenting them through a unified query interface. It is not a crawler or a full-text search engine, but rather a curated index that maintains stable references to valuable technical articles, news updates, and documentation pages spread across h5, 3g, and wap subdomains. The project is particularly suited for teams that need to preserve access to legacy mobile-optimized content while maintaining modern development workflows.

Primary target users include backend engineers integrating external content sources, technical writers managing reference links, and DevOps personnel who require reproducible resource lists for environment provisioning. By treating each URL as a first-class entity with metadata annotations, the aggregator enables batch operations, link validation, and formatted export without modifying the original content sources.

## 功能概览

- **Multi-subdomain Resource Indexing**: Automatically categorizes incoming URLs by their subdomain prefix (h5, 3g, wap) and extracts the numeric content identifier from the snews path segment, enabling rapid filtering and grouping operations.

- **Structured Metadata Extraction**: Parses each URL to derive content type hints, potential publication timestamps from identifier patterns, and subdomain-specific access characteristics such as mobile optimization levels.

- **Batch Validation Pipeline**: Performs periodic HEAD requests against all indexed resources to detect broken links, returning HTTP status codes and response times without downloading full page payloads.

- **Canonical URL Preservation**: Maintains the exact original URL strings as provided, with no automatic protocol upgrading, www normalization, or trailing slash enforcement, ensuring faithful representation of source references.

- **Export Formatters**: Generates machine-readable output in plain list format, JSON arrays, or environment-variable ready syntax, supporting integration with CI/CD pipelines and documentation generators.

- **Resource Change Detection**: Compares successive index snapshots to identify newly added or removed URLs, producing delta reports that highlight content churn across the yidianmeii.cn ecosystem.

- **Tag Association Engine**: Allows manual annotation of each URL with arbitrary key-value tags, enabling custom categorization schemes such as "backend", "frontend", "security", or "release-notes".

- **Query Filtering Interface**: Supports substring-based filtering on the full URL string, numeric range queries on the snews identifier, and boolean combinations of tag conditions.

## 应用场景

- **Technical Documentation Reference Management**: Documentation teams can maintain a verified list of external references within product manuals. When a new version of the documentation is released, the aggregator validates that all yidianmeii.cn source links remain accessible and returns warnings for any expired resources, preventing broken reference issues in published materials.

- **Content Migration Auditing**: Organizations migrating from legacy mobile subdomains (3g and wap) to a unified responsive platform can use the aggregator to inventory all existing content paths. The system produces a complete manifest of 10+ resource identifiers, which can be mapped against new routing tables to ensure zero content loss during transition.

- **Automated News Feed Assembly**: News aggregation services can consume the URL list as a seeding source. The aggregator provides a stable, versioned reference set that can be refreshed on a schedule, allowing downstream processors to fetch and parse the underlying HTML content without hardcoding individual subdomain patterns.

- **CI/CD Environment Validation**: In continuous deployment workflows, the aggregator serves as a pre-deployment check tool. Before rolling out updates, the pipeline queries the aggregator to confirm that all external content dependencies referenced by the application are still reachable, reducing runtime failures caused by inaccessible third-party resources.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/your-organization/yidianmeii-aggregator.git
cd yidianmeii-aggregator

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Initialize the resource index from the bundled URL list
python manage.py init-index --source data/urls.txt

# Run the validation scan against all indexed resources
python manage.py validate --timeout 5 --retries 2

# Generate a formatted report in plain list format
python manage.py export --format list --output resources.md

# Start the query interface (local development server)
python manage.py serve --host 127.0.0.1 --port 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 或更高 | 核心运行时，用于解析 URL、执行 HTTP 验证和生成导出格式 |
| requests | 2.28.0 或更高 | HTTP 客户端库，用于执行资源可达性检测和响应头抓取 |
| click | 8.1.0 或更高 | 命令行接口框架，提供子命令解析和参数验证能力 |
| pytest | 7.0.0 或更高 | 单元测试框架，仅在开发环境中需要，用于执行测试套件 |
| flake8 | 6.0.0 或更高 | 代码风格检查工具，用于保持提交代码符合 PEP 8 规范 |
| python-dotenv | 1.0.0 或更高 | 环境变量加载器，用于从 .env 文件读取超时和重试配置 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|-----|------|-----------|
| 用户指南 | docs/user-guide.md | 如何添加新资源、如何运行验证扫描、如何导出不同格式的列表 |
| 运维手册 | docs/operations.md | 如何部署到生产环境、如何配置日志轮转、如何设置监控告警 |
| API 参考 | docs/api-reference.md | 内部函数签名是什么、各模块的输入输出类型、扩展点在哪里 |
| 设计文档 | docs/design.md | 为什么选择这种数据结构、URL 规范化策略的考量、性能瓶颈分析 |

## 资源列表

- http://h5.yidianmeii.cn/snews/195888.shtml
- http://3g.yidianmeii.cn/snews/1891.shtml
- http://3g.yidianmeii.cn/snews/1361.shtml
- http://3g.yidianmeii.cn/snews/9167191.shtml
- http://h5.yidianmeii.cn/snews/366858.shtml
- http://wap.yidianmeii.cn/snews/9111636.shtml
- http://3g.yidianmeii.cn/snews/8068.shtml
- http://wap.yidianmeii.cn/snews/115539.shtml
- http://wap.yidianmeii.cn/snews/8311733.shtml
- http://3g.yidianmeii.cn/snews/6660.shtml

## 项目结构

```
yidianmeii-aggregator/
├── src/                                   # 核心源码目录
│   ├── core/                              # 基础数据模型和类型定义
│   │   ├── resource.py                    # Resource 类，封装 URL 和元数据
│   │   └── index.py                       # Index 类，管理资源集合与快照
│   ├── parsers/                           # URL 解析与信息提取模块
│   │   ├── subdomain.py                   # 提取 h5/3g/wap 子域名标识
│   │   └── identifier.py                  # 从 snews 路径提取数字 ID
│   ├── validators/                        # HTTP 验证与状态检查
│   │   ├── http_checker.py                # 执行 HEAD 请求并返回状态码
│   │   └── batch_runner.py                # 并发控制与重试逻辑
│   └── exporters/                         # 输出格式化器
│       ├── list_export.py                 # 纯文本列表导出
│       └── json_export.py                 # JSON 结构化导出
├── tests/                                 # 单元测试与集成测试套件
│   ├── test_parsers.py                    # 解析器模块的测试用例
│   └── test_validators.py                 # 验证器模块的测试用例
├── data/                                  # 静态数据与资源清单
│   └── urls.txt                           # 默认初始资源列表（10 个 URL）
├── docs/                                  # 项目文档（用户指南与设计文档）
│   ├── user-guide.md                      # 面向使用者的操作手册
│   └── design.md                          # 架构决策与数据流说明
├── scripts/                               # 辅助运维脚本
│   └── daily_scan.sh                      # 每日自动验证的 cron 包装脚本
├── requirements.txt                       # 生产环境 Python 依赖清单
├── requirements-dev.txt                   # 开发环境额外依赖（测试、lint）
├── manage.py                              # 统一命令行入口点
├── .flake8                                # flake8 代码检查配置文件
└── README.md                              # 项目首页文档（本文件）
```

## 贡献指南

1. 在 GitHub Issues 中查找标记为 "good-first-issue" 或 "help-wanted" 的任务，或创建新 Issue 描述你希望添加的功能或修复的问题，等待维护者确认需求合理性。

2. Fork 主仓库到个人账户，克隆 fork 后的仓库到本地开发环境，并按照快速开始章节的步骤安装所有开发依赖。确保使用 `requirements-dev.txt` 安装额外测试工具。

3. 创建新的功能分支，分支名称遵循 `feature/描述` 或 `fix/描述` 格式，例如 `feature/add-json-export`。所有代码提交需要附带清晰的提交信息，说明变更的原因和内容。

4. 在提交 Pull Request 之前，运行 `pytest tests/` 确保所有已有测试通过，并针对新增代码补充对应的单元测试。执行 `flake8 src/` 检查代码风格，确保无 PEP 8 违规项。

5. 提交 Pull Request 到主仓库的 main 分支，在描述中关联相关的 Issue 编号，并详细列出变更内容、测试结果以及任何可能影响现有功能的破坏性改动。等待至少一位维护者的代码审查。

## 常见问题

**问：为什么 URL 列表中的资源有些无法访问？**

答：该聚合器仅作为资源索引层，不保证外部源服务器的可用性。部分资源可能因源站维护、内容下架或网络策略变更而暂时或永久不可达。建议运行 `manage.py validate` 命令获取当前可达性状态，并参考返回的 HTTP 状态码判断具体原因。对于持续不可用的资源，可以从索引中移除或替换为替代来源。

**问：如何向索引中添加新的 yidianmeii.cn 资源？**

答：直接将新 URL 追加到 `data/urls.txt` 文件中，每行一个 URL，格式需与现有条目保持一致。添加完成后，运行 `manage.py init-index --force` 重新构建索引快照。系统会自动解析子域名和标识符，并在后续验证扫描中检查新资源。注意不要修改已有 URL 的原始字符串形式，包括协议和子域名部分。

**问：该聚合器是否会自动抓取并存储 HTML 页面内容？**

答：不会。该工具明确限定为轻量级索引和验证层，不执行任何页面内容的抓取、解析或持久化存储。验证过程仅使用 HTTP HEAD 方法获取响应头信息，不下载响应体。这一设计保证了低资源消耗，并避免了潜在的版权或法律合规风险。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
