# NewsLink Aggregator

NewsLink Aggregator 是一个面向技术信息采集与新闻外链聚合的开源工具集，定位于为独立开发者、舆情分析人员、垂直领域内容运营者提供轻量级、可扩展的新闻来源管理方案。该项目不直接存储或发布任何新闻内容，而是通过结构化的外链索引机制，帮助用户高效整理来自多个子域名的新闻条目，并支持自定义过滤、去重、分类与导出流程。

目标用户包括：需要批量管理新闻链接的内容编辑、搭建私有舆情看板的运维工程师、以及希望快速构建新闻聚合原型的全栈开发者。NewsLink Aggregator 以纯静态资源 + 脚本方式运作，无需数据库依赖，开箱即用，适合部署在低成本的云主机或 Serverless 环境中。

## 功能概览

- **多子域名来源统一索引**：支持同时从 3g、wap、h5 等移动端子域名拉取新闻条目，自动合并为统一时间线。
- **原始链接严格保真输出**：所有外链按照原始协议、域名、路径和参数原样保留，不进行任何自动跳转、协议升级或 URL 重写，确保新闻来源可追溯。
- **基于日期的批次管理**：内置按日期（如 0814）分组浏览能力，便于对特定批次新闻进行集中审阅和标注。
- **黑名单与白名单过滤**：提供可配置的 keywords 过滤规则，支持正则表达式，帮助用户屏蔽广告页或非目标栏目。
- **结构化元数据提取**：从 URL 路径中解析 newsid 及栏目层级，生成 JSON 格式的元数据快照，便于二次开发。
- **批量导出为 CSV/Markdown**：支持将当前筛选结果一键导出为表格文件，适配 Excel、Numbers 或 Obsidian 等工具。
- **定时更新钩子**：提供 cron 触发脚本示例，可配合系统定时任务自动拉取最新外链列表，保持索引新鲜度。
- **日志与去重记录**：维护本地 sqlite3 轻量日志，记录每次拉取的新增、失效和重复链接数量。

## 应用场景

**个人技术信息流定制**：开发者可定期将 NewsLink Aggregator 部署到个人 VPS，通过定时任务抓取指定域名的新闻索引，再配合邮件或 Telegram Bot 推送摘要，替代对多个新闻站点的反复手动访问。

**垂直领域竞品动态监控**：市场分析人员可将项目部署在内网服务器，利用过滤规则仅保留包含特定产品名或竞品关键词的新闻链接，生成日报并归档至共享目录，供团队晨会使用。

**开源数据集的构建预处理**：数据科学家或 NLP 研究者可将该项目作为数据采集管道的前端，通过导出的结构化链接列表，再并行调用爬虫下载正文内容，构建特定时间段的中文新闻语料库。

**内部知识库的“今日新闻”挂件**：企业内部 Wiki 或 Confluence 管理员可嵌入由 NewsLink Aggregator 生成的静态 HTML 片段，使员工在内部首页即时看到聚合后的行业新闻标题列表（点击跳转原文）。

## 快速开始

以下步骤适用于 Linux / macOS 环境，亦可在 Windows WSL2 中运行。

```bash
# 克隆仓库
git clone https://github.com/your-org/newslink-aggregator.git
cd newslink-aggregator

# 安装 Python 依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 运行默认批次拉取（示例批次 0814）
python run.py --batch 0814 --output ./output

# 查看生成的链接列表
cat ./output/links_0814.txt
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心运行环境，用于执行拉取、过滤和导出脚本 |
| pip | 20.0 及以上 | 管理 Python 依赖包 |
| sqlite3 | 3.31 及以上 | 用于本地日志记录和去重缓存（Python 内置模块） |
| requests | 2.25.1 及以上 | 发送 HTTP 请求获取外链页面内容 |
| beautifulsoup4 | 4.9.3 及以上 | 解析 HTML 结构以提取链接和文本锚点 |
| lxml | 4.6.3 及以上 | 作为 beautifulsoup4 的解析器，提高解析效率 |
| pytest | 6.2.0 及以上 | 仅开发测试时需要，生产环境可不安装 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user-guide.md | 如何配置过滤规则、如何调整输出格式、如何定时运行？ |
| 开发者指南 | docs/developer-guide.md | 如何扩展新的子域名来源、如何修改元数据提取逻辑？ |
| 部署参考 | docs/deployment.md | 支持哪些部署方式（Docker、systemd、cron）？如何保证外链稳定访问？ |
| 常见工作流 | docs/workflows.md | 如何结合 RSSHub、Huginn 或 n8n 实现自动化链路？ |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/3731.shtml
- http://wap.yidianmeii.cn/jnews/0814/1967.shtml
- http://h5.yidianmeii.cn/jnews/0814/6323.shtml
- http://3g.yidianmeii.cn/jnews/0814/96737.shtml
- http://wap.yidianmeii.cn/jnews/0814/45685.shtml
- http://3g.yidianmeii.cn/jnews/0814/1329324.shtml
- http://3g.yidianmeii.cn/jnews/0814/5132.shtml
- http://h5.yidianmeii.cn/jnews/0814/3544.shtml
- http://h5.yidianmeii.cn/jnews/0814/2874842.shtml
- http://wap.yidianmeii.cn/jnews/0814/06904.shtml

## 项目结构

```
newslink-aggregator/
├── run.py                      # 程序入口，解析命令行参数并调度主流程
├── requirements.txt            # Python 依赖清单
├── config/
│   ├── settings.yaml           # 全局配置文件（超时、重试、用户代理等）
│   └── filters.yaml            # 关键词黑名单与白名单规则
├── core/
│   ├── fetcher.py              # 负责发送 HTTP 请求并获取响应内容
│   ├── parser.py               # 从 HTML 中提取结构化链接与锚点文本
│   ├── dedup.py                # 基于 sqlite3 的本地去重缓存管理
│   └── exporter.py             # 导出为 CSV、Markdown 或纯文本格式
├── utils/
│   ├── logger.py               # 日志记录模块，支持文件与控制台双输出
│   └── validator.py            # URL 合法性校验与协议归一化检查（但保留原样）
├── tests/
│   ├── test_fetcher.py         # 网络请求模块的单元测试
│   ├── test_parser.py          # 解析逻辑的测试用例
│   └── test_dedup.py           # 去重缓存模块的测试
├── output/                     # 默认输出目录（生成的外链列表存放于此）
│   └── .gitkeep
├── logs/                       # 运行日志存储目录
│   └── .gitkeep
└── docs/                       # 详细文档
    ├── user-guide.md
    ├── developer-guide.md
    └── deployment.md
```

## 贡献指南

我们欢迎并鼓励社区贡献，无论是提交问题报告、改进文档，还是增加新的解析器适配。请按照以下流程参与：

1. 在 GitHub Issues 中查找或新建一个与您改动相关的讨论主题，简要说明您要修复的问题或新增的功能，避免重复工作。
2. Fork 本仓库到您的个人账户，并基于 main 分支创建一个新的特性分支，分支命名建议采用 `feature/简要描述` 或 `fix/问题编号` 格式。
3. 在您的分支上完成代码或文档修改，并确保所有现有单元测试通过。若添加了新功能，请同步补充对应的测试用例和文档说明。
4. 提交 Pull Request 到本仓库的 main 分支，PR 描述中需关联对应的 Issue 编号，并列举主要变更点。提交后 CI 会自动运行测试与代码风格检查。
5. 项目维护者将在 3 个工作日内审阅您的 PR，可能会提出修改意见。合并后，您的贡献将被列入项目贡献者列表（CONTRIBUTORS.md）。

## 常见问题

**问：项目会自动跳转或补全 URL 吗？为什么输出必须保持原样？**

答：不会。NewsLink Aggregator 严格遵循“不修改原始链接”的设计原则。因为新闻来源站点的访问权限、内容变更或 A/B 测试往往依赖于特定的子域名、协议或路径格式，任何自动补全（如添加 www 或强制 HTTPS）都可能导致链接访问失败或重定向到错误页面。因此我们强制保留用户提供的原始链接形态，确保可复现性和可追溯性。

**问：如何更新已拉取的链接批次？会覆盖之前的记录吗？**

答：默认情况下，每次运行会基于 `--batch` 参数指定的日期生成独立输出文件（如 `links_0814.txt`），不会覆盖其他批次。同时去重模块会记录已处理链接的哈希值，若同一链接在后续拉取中再次出现，会被标记为“重复”并记录在日志中，但不会再次写入输出主文件，避免列表膨胀。如需强制重新拉取全部链接，可使用 `--force` 选项。

**问：该项目能否直接抓取新闻正文内容？**

答：不能。NewsLink Aggregator 定位为“外链索引工具”，而非完整爬虫。项目仅提取页面中的链接地址及其周围的标题锚点，不下载或解析新闻正文，以免增加带宽负载和法律合规风险。若您需要正文内容，可基于本项目导出的链接列表，另行使用专门的抓取工具或服务进行处理。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
