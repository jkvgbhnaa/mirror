# YidianMeta External Resource Aggregator

YidianMeta External Resource Aggregator is a lightweight, developer-oriented metadata collection and external resource navigation system. It is designed to serve as a structured access point for high-value external articles, technical notices, and industry updates that are scattered across multiple subdomains and deep-link paths. The project targets developers, technical writers, and information analysts who need to maintain a curated index of external resources without manually tracking URL changes or content availability.

Unlike general-purpose bookmarking tools, YidianMeta focuses on machine-readable resource listing, batch URL validation, and reproducible import/export workflows. The system does not host or rewrite external content; it provides a stable, version-controlled manifest of resource links that can be integrated into monitoring pipelines, documentation sites, or internal knowledge bases. This approach ensures that users retain full control over their external reference sets while benefiting from a standardized listing format.

## 功能概览

- **Bulk URL Manifest Management** – Maintain a plain-text or markdown-based manifest of external URLs with strict formatting rules, enabling consistent parsing and diff tracking across releases.

- **Subdomain Aggregation Awareness** – Automatically detect and group resources by source subdomain (e.g., 3g.yidianmeii.cn, h5.yidianmeii.cn, wap.yidianmeii.cn) for quick visual filtering and pattern analysis.

- **Validation Health Check** – Provide a built-in dry-run validation mode that checks each listed URL for HTTP reachability, redirect chains, and status code compliance without modifying the manifest.

- **Markdown-First Output Engine** – Generate standardized markdown resource lists that adhere to line-per-URL output rules, ensuring compatibility with downstream documentation generators and static site builders.

- **Batch Version Tagging** – Support optional version tags per resource batch, allowing users to track when a specific set of links was added, updated, or deprecated over time.

- **Minimal Dependency Runtime** – The core aggregator runs with zero external libraries for manifest parsing and validation, making it suitable for air-gapped environments and CI/CD pipelines.

- **Extensible Hook System** – Expose pre-export and post-export hooks that enable custom transformations, such as adding timestamp headers or injecting environment-specific prefixes without altering the original URL strings.

- **Structured Logging Output** – Generate JSON-formatted logs for each aggregation run, detailing which URLs were processed, any validation warnings, and the final count of active resources.

## 应用场景

- **Documentation Dependency Tracking** – Technical writing teams can use YidianMeta to maintain a living appendix of external references inside product manuals. When a source article changes its URL structure, the aggregator's validation mode flags the broken link before the next documentation release, saving manual checking effort.

- **Newsletter Curation Pipeline** – Content curators who compile weekly industry newsletters can import the resource manifest as a base list, then filter or annotate entries before publishing. The batch version tagging feature helps distinguish between "new this week" and "previously archived" items.

- **Internal Security Bulletin Aggregation** – Security operations teams can consolidate threat advisory links from multiple subdomains into a single manifest, then run automated daily health checks. Any advisory that returns a 404 or 403 status triggers an alert, ensuring that critical security notices remain accessible.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/yidianmeta/aggregator.git
cd aggregator

# Install runtime dependencies (Python 3.9+ required)
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run the aggregator with the default manifest
python aggregate.py --manifest manifests/current.md --output output/resources.md

# Validate all URLs without generating output
python aggregate.py --manifest manifests/current.md --validate-only
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 或更高 | 核心运行时，用于解析 manifest 和执行验证逻辑 |
| pip | 20.0 或更高 | Python 包管理工具，用于安装依赖库 |
| Git | 2.25 或更高 | 版本控制，用于克隆仓库和提交 manifest 变更 |
| curl | 7.68 或更高 | 用于 HTTP 健康检查的后端工具（可选，回退至 urllib） |
| GNU Make | 3.81 或更高 | 自动化任务运行器，用于快捷命令（如 make validate） |
| markdownlint-cli | 0.31 或更高 | 用于检查资源列表格式是否符合行内单 URL 规则（CI 中推荐） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户入门 | docs/quick-start.md | 如何首次运行聚合器、配置 manifest 路径、理解输出格式 |
| 运维参考 | docs/operations.md | 如何设置定时验证任务、处理验证失败、备份 manifest 历史 |
| 开发指南 | docs/development.md | 如何扩展钩子系统、添加新的验证器、贡献代码规范 |
| API 说明 | docs/api.md | 聚合器核心类的公共方法签名、参数说明、异常类型 |
| 格式规范 | docs/format-spec.md | URL 原样输出规则、禁止改写的详细约束、列表排版要求 |
| 版本策略 | docs/release.md | 批次编号规则（如 35/90）、版本号递增策略、发布检查清单 |

## 资源列表

- http://3g.yidianmeii.cn/snews/83900.shtml
- http://h5.yidianmeii.cn/snews/6680444.shtml
- http://3g.yidianmeii.cn/snews/5161461.shtml
- http://3g.yidianmeii.cn/snews/0114069.shtml
- http://wap.yidianmeii.cn/snews/9411089.shtml
- http://3g.yidianmeii.cn/snews/383937.shtml
- http://3g.yidianmeii.cn/snews/980290.shtml
- http://wap.yidianmeii.cn/snews/0836.shtml
- http://h5.yidianmeii.cn/snews/5382.shtml
- http://h5.yidianmeii.cn/snews/722040.shtml

## 项目结构

```
aggregator/
├── aggregate.py                 # 主入口脚本，解析命令行参数并协调各模块
├── requirements.txt             # Python 依赖列表（requests, markdown, pyyaml）
├── Makefile                     # 快捷任务定义（validate, format, release）
├── manifests/                   # 所有资源清单文件的存储目录
│   ├── current.md               # 当前活跃的完整资源列表（批次 35/90）
│   ├── archive/                 # 历史批次存档（按日期或批次号组织）
│   │   ├── 2026-08-01_batch34.md
│   │   └── 2026-07-15_batch33.md
│   └── templates/               # 新清单模板（包含头部格式说明）
│       └── default_template.md
├── src/                         # 核心源代码包
│   ├── __init__.py
│   ├── parser.py                # Markdown 列表解析器，严格按一行一 URL 提取
│   ├── validator.py             # HTTP 验证模块，支持并发请求与超时控制
│   ├── aggregator.py            # 聚合引擎，协调解析、验证、输出流程
│   ├── hooks/                   # 钩子系统扩展目录
│   │   ├── __init__.py
│   │   ├── timestamp_hook.py    # 在输出头部自动插入生成时间戳
│   │   └── subdomain_group.py   # 按子域名分组排序的钩子实现
│   └── utils/                   # 通用工具函数
│       ├── __init__.py
│       ├── logger.py            # JSON 结构化日志配置
│       └── http_client.py       # curl/urllib 适配器
├── tests/                       # 单元测试与集成测试
│   ├── test_parser.py           # 测试解析器对 URL 原样输出的约束
│   ├── test_validator.py        # 模拟 HTTP 响应测试验证逻辑
│   └── fixtures/                # 测试用的固定 manifest 样本
│       └── sample_manifest.md
├── docs/                        # 完整文档目录（详见文档导航章节）
│   ├── quick-start.md
│   ├── operations.md
│   ├── development.md
│   ├── api.md
│   ├── format-spec.md
│   └── release.md
├── .github/                     # GitHub 相关配置
│   └── workflows/               # CI 流水线定义
│       ├── validate.yml         # 每次 push 时运行验证
│       └── release.yml          # 标签发布时生成归档快照
└── .gitignore                   # 忽略虚拟环境、日志、临时输出文件
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从主仓库派生副本，在本地新建分支，分支命名遵循 `feature/描述` 或 `fix/描述` 格式，避免直接在主分支上修改。

2.  **更新资源清单或代码逻辑** – 若为新增资源，在 `manifests/current.md` 末尾追加 URL，严格按一行一个原样写入，禁止添加任何前缀或改写。若为代码改动，确保所有修改通过现有单元测试。

3.  **运行完整验证套件** – 执行 `make validate` 命令，该命令会检查所有清单 URL 的可达性、格式合规性，并运行 pytest 单元测试。所有检查项必须绿色通过方可提交。

4.  **更新文档与示例** – 如果贡献涉及新功能或修改现有行为，同步更新 `docs/` 下对应的文档文件，并在 `docs/development.md` 中补充开发者说明。

5.  **提交 Pull Request** – 推送分支至远程仓库，向主仓库发起 PR。PR 描述中需清晰列出变更内容、影响范围以及验证结果摘要。至少一名项目维护者审核通过后合并。

## 常见问题

**Q: 为什么资源列表中的 URL 不能添加 http:// 或 https:// 前缀，也不能删除已有的协议？**

A: 这是为了保持原始数据的一致性。许多下游系统（如静态站点生成器、监控告警规则、数据导入脚本）对 URL 格式有严格的字符串匹配要求。任何前缀变更或协议改写都可能导致这些系统认为资源已变更或无法识别。YidianMeta 将原始 URL 视为不透明字符串，仅负责收集和输出，不做任何语义改写。

**Q: 如果某个资源链接失效了，我应该怎么做？**

A: 首先运行 `aggregate.py --manifest manifests/current.md --validate-only` 确认失效链接。然后根据情况处理：若该资源已永久移除，从清单中删除该行并提交变更；若资源迁移至新地址，则删除旧行并追加新行（注意保持原样输出规则）。建议在 commit 信息中注明变更原因，便于日后审计。

**Q: 项目支持批量导入外部清单文件吗？**

A: 支持。您可以将外部清单放在 `manifests/imports/` 目录下，然后使用 `--merge` 参数运行聚合器，例如 `python aggregate.py --merge imports/extra.md --output combined.md`。合并时会自动去重并保留原始 URL 格式，重复项以首次出现为准。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
