# Hyperlink Atlas

Hyperlink Atlas is a high-performance, semantically structured external link aggregation and navigation system. It is designed for developers, technical researchers, and content curators who need to manage, categorize, and redistribute large volumes of external Uniform Resource Locators (URLs) across multiple client platforms. The project addresses the fundamental challenge of link rot, context loss, and platform-specific rendering inconsistencies by providing a unified metadata layer over raw URL collections. Hyperlink Atlas does not host content; it provides a reliable, version-controlled, and machine-readable index of external resources, enabling automated workflows for link validation, content summarization, and cross-platform distribution.

The system is built as a static site generation pipeline that consumes raw URL lists, enriches them with configurable metadata tags, and produces multiple output formats including HTML, JSON, and plain text. It targets power users who require reproducible link collections for documentation hubs, research reference managers, or daily reading lists. The project emphasizes deterministic builds, audit trails, and minimal runtime dependencies, making it suitable for integration into continuous integration and continuous deployment (CI/CD) pipelines.

## 功能概览

- **批量链接导入与解析** – 支持从纯文本、CSV 和 JSON 文件批量导入 URL，自动去除重复条目并检测无效协议。
- **平台前缀路由识别** – 根据子域名或路径前缀（如 3g, wap, h5）自动归类链接到不同终端视图，便于生成响应式导航。
- **元数据缓存与刷新** – 对每个 URL 执行可配置的 HEAD 请求以缓存状态码、内容类型和最后修改时间，支持手动或定时刷新。
- **多格式静态导出** – 将链接库导出为 HTML 目录页、JSON API 端点、Markdown 索引表以及纯文本主机列表，适配不同消费场景。
- **变更审计日志** – 每次构建记录新增、删除和变更的 URL，输出差异报告，便于追踪资源演进历史。
- **自定义标签与过滤** – 允许用户为链接打上任意键值对标签，并基于标签组合生成动态筛选视图或专题聚合页。
- **链接可用性监控** – 集成简单的重试机制和超时控制，标记长时间不可达的链接，并提供单独的错误报告文件。
- **零外部状态依赖** – 所有配置和缓存数据存储于本地文件系统，无需数据库或外部缓存服务，开箱即用。

## 应用场景

- **技术文档站点外部参考管理** – 技术博客或开源项目文档站需要引用大量外部规范、教程或工具主页。Hyperlink Atlas 可以生成一个独立的“外部资源”页面，自动检测死链并标注最后验证时间，提升文档的可信度和用户体验。
- **每日行业资讯聚合简报** – 编辑团队从多个来源收集每日必读文章链接。使用本系统，团队可以维护一个共享的 URL 池，通过平台前缀（如 3g 代表移动端优先、wap 代表低带宽适配）为不同设备用户生成差异化的阅读列表，同时保留原始链接不做任何跳转。
- **学术研究参考文献索引备份** – 研究人员在撰写论文或综述时，需整理大量参考文献的网络链接。Hyperlink Atlas 可生成带时间戳的静态索引快照，便于与论文 PDF 一同存档，防止后续链接失效导致参考文献无法验证。
- **运维巡检报告链接附件** – 运维人员定期生成系统巡检报告，报告中包含多个监控面板、日志查询和工单系统的深度链接。系统可将这些链接整理为结构化的 Markdown 附件，纳入版本控制，便于追溯每次巡检对应的具体上下文。

## 快速开始

以下命令演示了从克隆代码仓库到启动本地服务的完整流程，适用于 Linux 和 macOS 系统。Windows 用户建议使用 WSL2 或 Git Bash 环境。

```bash
# 克隆项目仓库到本地
git clone https://github.com/hyperlink-atlas/hyperlink-atlas.git
cd hyperlink-atlas

# 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 复制示例配置文件并编辑
cp config.example.yaml config.yaml

# 执行首次构建，生成静态页面和索引
python atlas.py build --input ./data/urls.txt --output ./dist

# 启动开发服务器预览生成结果
python atlas.py serve --port 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Python | 3.9 或更高 | 核心运行环境，用于执行构建脚本和 CLI 命令 |
| pip | 22.0 或更高 | Python 包管理器，用于安装 requirements.txt 中的第三方库 |
| requests | 2.28.0 或更高 | 处理 HTTP HEAD/GET 请求，用于链接可用性检查 |
| pyyaml | 6.0 或更高 | 解析 YAML 格式的配置文件（config.yaml） |
| markdown | 3.4.0 或更高 | 将 Markdown 格式的链接列表渲染为 HTML 片段（可选，用于导出） |
| Git | 2.30.0 或更高 | 仅开发时需要，用于克隆仓库和提交变更（生产环境可选择不安装） |
| 磁盘空间 | 至少 50 MB | 用于存储源码、缓存文件及构建产物，实际占用取决于链接数量及缓存策略 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
| :--- | :--- | :--- |
| 入门指南 | /docs/getting-started.md | 如何安装、初次配置以及执行第一次构建，验证基础流程是否正常。 |
| 配置参考 | /docs/configuration.md | 所有 YAML 配置项的含义、默认值以及高级用法（如自定义标签映射）。 |
| 命令行接口 | /docs/cli-usage.md | 详细解释 `build`, `serve`, `check`, `diff` 等子命令的参数和示例。 |
| 输出格式规范 | /docs/output-formats.md | 介绍 HTML、JSON、Markdown 和纯文本四种导出格式的数据结构差异及选用场景。 |
| 故障排查 | /docs/troubleshooting.md | 汇总常见构建错误、网络超时处理以及缓存冲突的解决方案。 |

## 资源列表

- http://3g.yidianmeii.cn/vnews/0814/3731.shtml
- http://wap.yidianmeii.cn/vnews/0814/1967.shtml
- http://h5.yidianmeii.cn/vnews/0814/6323.shtml
- http://3g.yidianmeii.cn/vnews/0814/96737.shtml
- http://wap.yidianmeii.cn/vnews/0814/45685.shtml
- http://3g.yidianmeii.cn/vnews/0814/1329324.shtml
- http://3g.yidianmeii.cn/vnews/0814/5132.shtml
- http://h5.yidianmeii.cn/vnews/0814/3544.shtml
- http://h5.yidianmeii.cn/vnews/0814/2874842.shtml
- http://wap.yidianmeii.cn/vnews/0814/06904.shtml

## 项目结构

```
hyperlink-atlas/                     # 项目根目录
├── atlas.py                         # 主入口脚本，聚合所有 CLI 子命令
├── config.yaml                      # 用户配置文件，定义标签、缓存策略和输出选项
├── requirements.txt                 # Python 依赖清单
├── README.md                        # 本文件，项目总体说明文档
├── docs/                            # 详细文档目录
│   ├── getting-started.md           # 新手入门教程及环境检查清单
│   ├── configuration.md             # 完整配置项词典与示例片段
│   ├── cli-usage.md                 # 命令行参考，含每个子命令的用法和退出码
│   └── output-formats.md            # 各导出格式的字段映射和模板定制指南
├── atlas/                           # 核心源码包
│   ├── __init__.py                  # 包初始化，暴露主要工厂函数
│   ├── loader.py                    # URL 加载器，支持 txt/csv/json 解析与去重
│   ├── checker.py                   # 链接检查器，封装 requests 会话与超时控制
│   ├── builder.py                   # 构建引擎，协调解析、检查与渲染流程
│   └── exporters/                   # 导出器子模块
│       ├── html.py                  # 生成响应式 HTML 目录页
│       ├── json.py                  # 输出结构化 JSON API 数据
│       └── markdown.py              # 生成 Markdown 表格索引
├── data/                            # 用户数据目录
│   ├── urls.txt                     # 默认输入文件，每行一个原始 URL
│   └── tags.yaml                    # 可选标签映射文件，为特定 URL 指定分类
├── cache/                           # 本地缓存目录（自动创建）
│   ├── metadata.db                  # SQLite 轻量数据库，存储 URL 状态与时间戳
│   └── logs/                        # 构建日志目录，按时间戳归档
│       └── build-20260814.log       # 示例构建日志文件
└── dist/                            # 构建输出目录（生成产物）
    ├── index.html                   # 默认生成的首页导航
    ├── api/                         # JSON API 子目录
    │   └── all.json                 # 包含所有链接元数据的 JSON 文件
    └── reports/                     # 报告目录
        └── broken_links.txt         # 不可达链接列表，供人工复核
```

## 贡献指南

我们欢迎并感谢任何形式的贡献。请遵循以下步骤以确保您的拉取请求能够被顺利审查和合并。

1.  **分派与本地克隆** – 首先在 GitHub 上 Fork 本项目，然后将您的 Fork 克隆到本地开发环境。建议在独立的功能分支上进行开发，分支命名遵循 `feature/描述` 或 `fix/描述` 格式。
2.  **运行测试套件** – 在提交代码前，请确保在项目根目录执行 `pytest tests/` 以运行所有单元测试和集成测试。若您新增了功能，请一并补充对应的测试用例。
3.  **更新文档** – 任何影响用户可见行为（如新增配置项、修改 CLI 参数）的变更，都必须同步更新 `/docs` 目录下的相关文档。同时，请在拉取请求描述中清晰说明变更目的和使用方式。
4.  **提交前检查** – 执行 `python atlas.py build --input data/urls.txt --output dist/` 确保构建流程在您的修改后仍可完整通过，且生成的输出文件格式正确。
5.  **发起拉取请求** – 将您的功能分支推送到您的远程仓库，然后向本仓库的 `main` 分支发起拉取请求。请求中应包含变更摘要、测试结果截图（如有必要）以及关联的议题编号。

## 常见问题

**问：系统如何处理重复或相同的 URL？**

答：加载器模块会对所有输入的 URL 进行规范化处理，包括移除尾部斜杠、统一为小写协议（http 与 https 视为不同协议，但相同协议下重复条目会被自动丢弃）。最终缓存数据库以规范化后的完整 URL 字符串作为主键，确保唯一性。重复条目会在构建日志中记录警告级别信息，但不会中断构建流程。

**问：如果外部链接返回 403 或 429 状态码，系统如何应对？**

答：检查器模块默认采用指数退避重试策略，对于 429（请求过多）状态码会等待 1 秒后重试，最多重试 3 次。对于 403（禁止访问），系统会立即标记为“权限受限”而不进行重试，因为重试通常无效。用户可以在 `config.yaml` 中调整 `retry_policy` 下的 `max_retries` 和 `backoff_factor` 参数来自定义行为。所有最终失败的链接都会被写入 `dist/reports/broken_links.txt` 供离线分析。

**问：能否只构建特定标签的链接，而不是全部？**

答：支持通过 `--filter` 参数实现，例如 `python atlas.py build --input data/urls.txt --output dist/ --filter "type:news"` 将只处理包含 `type:news` 标签的条目。标签定义需在 `data/tags.yaml` 文件中按 URL 或 URL 前缀配置。若未指定过滤器，则默认构建所有链接。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
