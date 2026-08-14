# Yidian News Aggregator

Yidian News Aggregator is a lightweight, resource-oriented information aggregation middleware designed for content distribution networks and mobile-first news syndication systems. It serves as a structured gateway to time-sensitive news resources published through the YidianMeii content platform, providing developers, data analysts, and content integrators with a predictable, crawlable, and link-stable access pattern to daily news batches.

This project is not a rendering engine nor a full-featured CMS. It is a curated resource index that organizes and exposes raw news entry points from the YidianMeii mobile and H5 distribution channels. The primary target users are system integrators building news feed aggregators, archival systems, traffic analysis pipelines, and content monitoring tools that require consistent access to dated news item collections. By standardizing the presentation of these resource URLs, the project reduces the friction of manual link discovery and enables automated workflows around batch news processing.

## 功能概览

**Batch Resource Indexing** – Aggregates and categorizes news item URLs from multiple subdomains (h5, wap, 3g) into a single discoverable index, organized by publication date and internal item identifier.

**Channel Segmentation** – Separates resources by their originating distribution channel, allowing integrators to route traffic or apply channel-specific parsing rules based on the subdomain prefix.

**Raw Link Preservation** – Outputs all resource URLs in their original, unmodified form without protocol normalization, trailing slash enforcement, or query parameter stripping, ensuring compatibility with existing content delivery configurations.

**Static Index Generation** – Provides a lightweight, dependency-free static listing that can be hosted on any web server or object storage without requiring server-side rendering or database connectivity.

**Dated Directory Structure** – Organizes resources under date-based logical partitions (year/month/day), simplifying time-windowed retrieval and incremental update strategies.

**Mobile-Optimized Source Compatibility** – Maintains full compatibility with both mobile-first (wap) and responsive (h5, 3g) distribution endpoints, ensuring that integrators can consume content in its native presentation format.

**Extensible Link Catalog** – Designed as a modular resource list that can be programmatically extended to include additional news batches from subsequent publication cycles without altering the core indexing logic.

## 应用场景

**Automated News Archival Systems** – Organizations building long-term news archives can use this index to periodically fetch and store raw news content from the listed URLs. The predictable link format allows schedulers to automatically retrieve new items as they are published without manual intervention.

**Content Aggregation Platforms** – News aggregator services can integrate these resource URLs as upstream sources, merging them with feeds from other providers. The channel-specific subdomains enable fine-grained routing, such as sending H5 links to a desktop-friendly parser and WAP links to a mobile-optimized processor.

**Traffic Analytics and Monitoring** – Analytics pipelines can consume the URL list to verify link availability, measure response times, and track content update patterns across different distribution channels. This helps detect delivery anomalies or regional accessibility issues.

**Data Mining and NLP Research** – Researchers working on news corpus construction can utilize this index as a seed list for web scraping, enabling the collection of time-stamped news articles for linguistic analysis, topic modeling, or sentiment trend studies.

**Reverse Proxy and CDN Configuration Testing** – DevOps engineers can use the provided URLs to validate reverse proxy rules, cache policies, and CDN routing behaviors across different subdomains, ensuring that content is properly delivered from edge locations.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/yidian-resources/yidian-news-aggregator.git
cd yidian-news-aggregator

# Install dependencies (if any; this project is static, but we include a minimal Python environment for validation)
pip install -r requirements.txt  # optional, only for validation scripts

# Generate the latest resource index (optional, if you have the generation script)
python generate_index.py --date 2026-08-14 --output index.md

# Alternatively, serve the static index directly using any HTTP server
python -m http.server 8080
# Or use nginx, Apache, or any static hosting service
```

## 安装要求

| 依赖 | 必需 | 说明 |
|------|------|------|
| Python 3.8+ | 否 | 仅当使用附带验证脚本或生成工具时需要；运行时无需任何编程语言环境 |
| 静态文件服务器 (nginx/Apache/Caddy) | 否 | 推荐用于生产部署，但非强制；任何可托管静态 Markdown 或 HTML 的服务器均可 |
| 网络连接 | 是 | 访问资源列表中的 URL 需要稳定的互联网连接；所有链接指向外部内容源 |
| 文件系统权限 | 是 | 部署时需要读取静态文件目录的权限；写入权限仅当使用生成脚本时必需 |
| 内存 128 MB | 否 | 运行验证脚本的最小内存要求；索引本身占用空间小于 1 MB |
| 磁盘空间 10 MB | 否 | 存放索引文件、日志和可选脚本的存储空间；实际资源内容不本地存储 |
| Git 2.0+ | 否 | 仅当通过版本控制克隆仓库时需要；可直接下载 ZIP 压缩包 |
| 操作系统 | 否 | 跨平台兼容 (Linux, macOS, Windows)；所有脚本使用纯 Python 标准库 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户入门 | docs/quick-start.md | 如何快速获取资源列表并将其集成到我的应用中？索引的更新频率是多少？ |
| 运维部署 | docs/deployment.md | 如何在不同环境中托管此索引？有哪些推荐的服务器配置和缓存策略？ |
| 数据格式 | docs/resource-schema.md | 资源 URL 的结构是什么？子域名和路径段各自代表什么含义？ |
| 扩展开发 | docs/extending.md | 如何添加新的新闻批次？如何自定义资源过滤或排序规则？ |

## 资源列表

- http://h5.yidianmeii.cn/vnews/0814/3216927.shtml
- http://wap.yidianmeii.cn/vnews/0814/598446.shtml
- http://wap.yidianmeii.cn/vnews/0814/924169.shtml
- http://wap.yidianmeii.cn/vnews/0814/42013.shtml
- http://wap.yidianmeii.cn/vnews/0814/2051.shtml
- http://3g.yidianmeii.cn/vnews/0814/3153344.shtml
- http://h5.yidianmeii.cn/vnews/0814/4421.shtml
- http://h5.yidianmeii.cn/vnews/0814/3759330.shtml
- http://wap.yidianmeii.cn/vnews/0814/7504355.shtml
- http://h5.yidianmeii.cn/vnews/0814/66332.shtml

## 项目结构

```
yidian-news-aggregator/
├── index.md                        # 主资源索引文件，包含完整的 URL 列表和项目文档
├── generate_index.py               # 可选脚本，用于从原始数据生成或更新 index.md
├── requirements.txt                # Python 依赖清单 (仅用于开发工具，生产环境无需安装)
├── config/
│   ├── channels.yaml               # 定义各子域名对应的频道名称和解析规则
│   └── sources.toml                # 上游数据源配置，描述批次日期和对应的资源路径模式
├── scripts/
│   ├── validate_links.py           # 检查所有列出的 URL 是否可访问 (HTTP 状态码验证)
│   ├── fetch_metadata.py           # 从资源页面提取标题、发布时间等元数据 (示例用途)
│   └── export_json.py              # 将资源列表转换为 JSON 格式，便于 API 集成
├── docs/
│   ├── quick-start.md              # 五分钟上手指南，涵盖获取、部署、访问流程
│   ├── deployment.md               # 生产部署建议，包含缓存、负载均衡、安全头配置
│   ├── resource-schema.md          # URL 结构详解，包含子域名、路径段、查询参数说明
│   └── extending.md                # 扩展指南，包含新批次添加、自定义解析器开发
├── tests/
│   ├── test_links.py               # 单元测试，验证链接格式是否符合预期模式
│   └── test_generate.py            # 测试索引生成逻辑，确保输出格式一致
├── .github/
│   └── workflows/
│       ├── validate.yml            # CI 工作流：每次提交时验证所有链接可用性
│       └── update-index.yml        # 定时任务：每日自动检查并更新资源列表 (如有新批次)
└── LICENSE                         # MIT 许可证文件
```

## 贡献指南

**提交新批次数据** – 如果您有来自 YidianMeii 平台的新新闻批次资源，请按照现有日期格式 (MMDD) 和编号模式整理 URL，并通过 Pull Request 提交更新到 `index.md` 中的资源列表章节。提交前请确保所有新增链接可访问且内容类型为 text/html。

**改进验证脚本** – 增强 `scripts/validate_links.py` 的功能，例如添加超时重试机制、并发检查、或更详细的响应统计输出。请确保改动向后兼容，并添加相应的单元测试至 `tests/` 目录。

**完善文档** – 补充或修订 `docs/` 目录下的任何文档，特别是 deployment.md 和 extending.md。修复拼写错误、添加配置示例、或澄清模糊的说明内容均受到欢迎。文档更新应保持技术中立的语气，避免主观评价。

**修复链接格式问题** – 如发现资源列表中存在 URL 格式异常 (例如包含多余空格、错误的转义字符、或协议不一致)，请及时提交修复。所有 URL 必须严格遵守原样输出规则，不得添加或修改协议前缀、域名大小写或尾部斜杠。

**提出功能请求** – 通过 GitHub Issues 提出新的功能建议，例如支持按关键词过滤资源、提供 JSON 和 RSS 输出格式、或增加资源分类标签。建议包含具体的用例描述和预期的实现方案轮廓。

## 常见问题

**问：资源列表中的链接访问时返回 404 或超时，应该如何处理？**

答：YidianMeii 平台的新闻资源按批次发布，部分历史链接可能因内容下架或 CDN 刷新延迟而暂时不可用。建议首先检查网络代理或防火墙设置，确认能够正常解析 `yidianmeii.cn` 域名。若问题持续，可运行 `scripts/validate_links.py` 进行批量验证，并将不可用的链接报告至 GitHub Issues，我们会定期核对并更新索引。对于关键应用，建议在集成时实现降级逻辑，例如跳过不可用链接并记录日志，而非中断整个处理流程。

**问：项目是否提供 JSON 或 RSS 格式的资源输出？**

答：当前主版本仅提供 Markdown 格式的资源列表，这是为了保持最大的可读性和版本控制友好性。但我们提供了 `scripts/export_json.py` 辅助脚本，可以将 `index.md` 中的资源列表解析并转换为 JSON 格式，方便程序化消费。RSS 格式的支持已列入未来版本规划，您可以在 extending.md 中查看自定义输出格式的开发指南，并自行实现适配器。

**问：如何确保我使用的资源列表是最新的？**

答：本项目采用静态索引模式，不会自动刷新。要获取最新批次，您需要定期关注本仓库的更新，或通过 GitHub Watch 功能接收发布通知。如果您有自动化需求，可以基于 `generate_index.py` 编写自己的轮询脚本，从上游数据源拉取最新链接并生成个性化索引。我们同时提供了 GitHub Actions 工作流示例 (`.github/workflows/update-index.yml`)，您可参考其逻辑自行部署定时更新任务。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
