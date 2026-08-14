# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, open-source information aggregation and navigation system designed to systematically organize and present distributed web resources from the Yidian network ecosystem. This project serves as a structured cataloging layer that transforms scattered content URLs into a maintainable, human-readable knowledge base with version tracking, content categorization, and quick-access navigation capabilities.

The aggregator is built for content curators, data analysts, and developer teams who need to manage large collections of external article links, monitor content updates across multiple subdomains, and maintain an auditable history of resource changes. By providing a standardized interface over raw URL lists, the project reduces the cognitive overhead of bookmark management and enables collaborative curation workflows.

## 功能概览

- **Bulk URL Import and Validation** – Automatically parse and validate large lists of resource links with format checking and duplicate detection.
- **Categorization by Subdomain Patterns** – Group resources based on source subdomain (wap, h5, 3g) for structured browsing and filtering.
- **Temporal Indexing and Sorting** – Extract and index date-based identifiers from URLs to enable chronological resource discovery.
- **Markdown-Based Documentation Pipeline** – Generate clean, consistent markdown documentation from raw resource data with minimal configuration.
- **Integrity Verification Dashboard** – Verify resource accessibility and response status with scheduled or on-demand health checks.
- **Export and Interoperability Support** – Export resource collections in multiple formats including plain text lists, JSON metadata, and CSV tables.
- **Version Control Integration** – Track additions, removals, and changes to the resource list through standard Git commit history.

## 应用场景

- **Content Archiving Projects** – When building a historical archive of published articles, the aggregator provides a structured way to catalog and reference each piece of content without manual spreadsheet management.
- **Monitoring Content Updates Across Subdomains** – Teams responsible for tracking content distribution across wap, h5, and 3g platforms can use the aggregator to maintain a unified view of all active resources and detect new publications.
- **Research and Data Collection** – Researchers collecting sample data from the Yidian network can utilize the aggregator to organize their link sets, annotate resources, and share collections with collaborators through a version-controlled repository.
- **Documentation Generation for External Systems** – Development teams integrating with third-party APIs can use the aggregator to generate up-to-date reference documentation that lists all relevant resource endpoints with clear source attribution.

## 快速开始

Clone the repository and run the included setup script to initialize the project environment.

```bash
git clone https://github.com/yidian-resource-aggregator/yra-core.git
cd yra-core
npm install --production
npm run build
./bin/aggregate.js --input resources.txt --output docs/resources.md
```

The aggregation pipeline reads a plain-text list of URLs from the input file, validates each entry, applies categorization rules, and produces a structured markdown document in the output location. For continuous usage, integrate the command with a cron job or CI pipeline to refresh the documentation regularly.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Node.js | 16.x 或更高 | 运行时环境，用于执行聚合脚本和依赖管理 |
| npm | 8.x 或更高 | 包管理器，用于安装项目依赖 |
| Git | 2.30 或更高 | 版本控制系统，用于克隆仓库和提交变更 |
| markdownlint-cli | 0.33.x | 可选工具，用于验证生成的 markdown 文档格式 |
| httpie | 任意版本 | 可选工具，用于手动测试资源可达性 |
| jq | 1.6 或更高 | 可选工具，用于 JSON 数据流处理 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|----------|------------|
| 用户手册 | docs/user-guide.md | 如何使用聚合器导入资源、生成文档、导出数据 |
| 配置参考 | docs/configuration.md | 有哪些可调整的配置项，如何自定义分类规则 |
| 开发者指南 | docs/development.md | 如何扩展聚合器，添加新的输入解析器或输出格式 |
| API 参考 | docs/api-reference.md | 核心模块和函数接口的详细说明 |

## 资源列表

- http://wap.yidianmeii.cn/vnews/0814/9220298.shtml
- http://h5.yidianmeii.cn/vnews/0814/8508.shtml
- http://3g.yidianmeii.cn/vnews/0814/8467891.shtml
- http://h5.yidianmeii.cn/vnews/0814/32092.shtml
- http://wap.yidianmeii.cn/vnews/0814/8646704.shtml
- http://wap.yidianmeii.cn/vnews/0814/965627.shtml
- http://wap.yidianmeii.cn/vnews/0814/95029.shtml
- http://wap.yidianmeii.cn/vnews/0814/11183.shtml
- http://h5.yidianmeii.cn/vnews/0814/6991196.shtml
- http://3g.yidianmeii.cn/vnews/0814/22402.shtml

## 项目结构

```
yra-core/
├── src/
│   ├── parsers/                 # 输入解析器模块
│   │   ├── url-validator.js    # URL 格式验证与标准化
│   │   └── plain-text-parser.js # 纯文本列表解析器
│   ├── categorizers/            # 分类与分组逻辑
│   │   ├── subdomain-grouper.js # 按子域名分组
│   │   └── date-indexer.js      # 从 URL 提取日期索引
│   ├── generators/              # 文档生成器
│   │   ├── markdown-builder.js  # Markdown 文档构建器
│   │   └── table-formatter.js   # 表格格式化工具
│   ├── checkers/                # 可用性检查
│   │   └── health-checker.js    # HTTP 状态检查器
│   └── cli/                     # 命令行接口
│       └── aggregate.js         # 主入口脚本
├── config/
│   ├── default.json             # 默认配置
│   └── schema.json              # 配置 JSON Schema
├── docs/
│   ├── user-guide.md            # 用户手册
│   ├── configuration.md         # 配置说明
│   └── api-reference.md         # API 文档
├── tests/
│   ├── unit/                    # 单元测试
│   └── integration/             # 集成测试
├── resources.txt.example        # 示例输入文件
├── package.json                 # npm 依赖清单
└── README.md                    # 本文件
```

## 贡献指南

1. 在 GitHub 上 fork 本仓库，并在本地克隆你的 fork 版本，确保使用最新的 main 分支作为基准。
2. 创建以 `feature/` 或 `fix/` 为前缀的分支进行开发，使用 `npm test` 运行现有测试用例，确认没有回归问题。
3. 为新功能或修复内容编写对应的单元测试和集成测试，测试覆盖率达到 80% 以上方可提交。
4. 提交代码时使用语义化提交信息格式，例如 `feat: add date-indexer module` 或 `fix: handle empty input gracefully`。
5. 向主仓库发起 Pull Request，在描述中详细说明改动目的、实现方案和测试结果，等待至少一位维护者审核。

## 常见问题

**问：输入的 URL 列表需要遵循什么格式？**

答：聚合器接受纯文本文件，每行一个 URL。空行和以井号开头的注释行会被自动忽略。所有 URL 按原样保留，不进行任何自动修正或跳转跟随。建议使用 `resources.txt` 作为固定文件名并将其置于项目根目录。

**问：如何更新已经生成的文档？**

答：在更新资源列表后，重新运行聚合命令 `npm run aggregate` 即可完全重新生成文档。聚合器会覆盖目标输出文件，建议使用 Git 管理变更历史。若需要保留多个版本，可结合构建流水线将输出文件命名为带时间戳的格式，例如 `docs/resources-2026-08-14.md`。

**问：是否可以自定义分类规则？**

答：可以。编辑 `config/default.json` 中的 `categorization.rules` 字段，添加或修改正则表达式模式即可定义新的分组逻辑。修改配置后需要重启聚合进程。详细配置语法请查阅 `docs/configuration.md` 文档中的规则示例。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
