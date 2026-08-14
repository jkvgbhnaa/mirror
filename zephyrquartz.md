# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, developer-oriented information indexing and external resource aggregation system. It is designed to collect, categorize, and present distributed web resources from heterogeneous sources, with a particular focus on structured news-style content served from mobile-optimized endpoints. The project targets developers, data analysts, and technical researchers who need to consolidate fragmented online materials into a single browsable and machine-readable interface.

Unlike traditional bookmark managers or CMS platforms, Yidian Resource Aggregator emphasizes deterministic URL mapping, minimal runtime overhead, and straightforward extensibility. It does not host content itself but provides a verified indexing layer over existing third-party resources. The system is built for maintainability, assuming that external URLs may change or become unavailable over time. It includes built-in link health checking and metadata extraction utilities to help operators monitor resource availability.

This project is particularly suited for internal team use, educational demonstrations, or as a foundation for building custom web scrapers and notification bots. It does not require a database, relies on flat-file configuration, and can be deployed on any POSIX-compliant environment with Python 3.8 or later.

## 功能概览

- **集中化资源索引** 提供统一的条目管理接口，将分散的移动端新闻链接聚合为结构化数据集，支持按源域名、路径模式和更新时间过滤。

- **自动链接健康检查** 内置异步 HTTP 探活机制，定期检测每个已收录 URL 的可访问性，并记录状态码与响应时间，便于运维人员及时发现失效链接。

- **元数据智能提取** 从目标页面自动抽取标题、发布时间、正文摘要等核心元数据，无需人工标注，降低资源整理成本。

- **多格式数据导出** 支持将资源列表导出为 JSON、CSV 和 Markdown 表格三种格式，方便导入其他数据分析工具或静态站点生成器。

- **可配置的更新策略** 允许管理员设置全局或单条资源的刷新频率，区分高频变动资源和稳定历史资源，避免无效网络请求。

- **轻量级 Web 预览界面** 提供只读的 HTML 仪表板，展示资源总数、健康率、最近更新等关键指标，并支持快速跳转至原始链接。

- **完整的日志记录** 记录所有资源访问、检查异常和用户操作行为，支持按级别过滤和按日期轮转，便于问题排查。

- **插件化输出钩子** 允许开发者通过简单的 Python 函数注册自定义输出处理器，例如发送通知到企业微信、Slack 或写入远程存储。

## 应用场景

**日常开发参考库** 开发团队可将该项目作为内部技术新闻聚合点，定期收录团队关注的博客、技术公告或安全通告。通过健康检查功能，团队能及时发现失效的外部参考链接，避免文档中的引用断裂。

**数据采集原型验证** 数据工程师可利用本项目的元数据提取模块快速测试目标站点是否适合采集，评估页面结构一致性。索引文件可作为采集任务队列的初始种子列表，减少手工构建起始 URL 的工作量。

**静态网站内容填充** 静态站点维护者可以定期从本系统导出 Markdown 格式的资源列表，直接嵌入到 Jekyll、Hugo 或 VuePress 生成的页面中，作为「友情链接」或「推荐阅读」区块的数据源，无需额外开发后端接口。

**教学案例与演示** 计算机课程教师可将本项目作为 Web 基础、网络请求或数据格式转换的教学示例。学生可以阅读完整代码，理解如何组织异步任务、如何处理 HTTP 异常以及如何设计可扩展的配置体系。

## 快速开始

```bash
# 克隆仓库到本地
git clone https://github.com/your-org/yidian-resource-aggregator.git
cd yidian-resource-aggregator

# 创建并激活 Python 虚拟环境（推荐）
python3 -m venv venv
source venv/bin/activate

# 安装所有运行时依赖
pip install -r requirements.txt

# 执行初始资源索引构建
python build_index.py --input resources.txt --output index.json

# 启动内置 Web 预览服务（默认端口 8080）
python serve.py --port 8080
```

完成上述步骤后，打开浏览器访问 `http://localhost:8080` 即可查看资源仪表板。若要执行完整的健康检查并生成报告，请运行：

```bash
python check_links.py --source index.json --report report.md
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 及以上 | 核心运行环境，所有脚本均基于 Python 编写 |
| aiohttp | 3.8.0 及以上 | 异步 HTTP 客户端，用于并发健康检查和元数据提取 |
| lxml | 4.9.0 及以上 | HTML 解析引擎，支持 XPath 和 CSS 选择器提取元数据 |
| click | 8.1.0 及以上 | 命令行参数解析库，用于构建友好的 CLI 交互 |
| Jinja2 | 3.1.0 及以上 | 模板渲染引擎，用于生成 HTML 预览仪表板 |
| pytest | 7.0.0 及以上 | 单元测试框架，仅开发环境需要，用于运行测试套件 |
| black | 22.0.0 及以上 | 代码格式化工具，仅开发环境需要，保持代码风格一致 |
| mypy | 0.990 及以上 | 静态类型检查器，仅开发环境需要，用于类型注解验证 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `docs/getting-started.md` | 如何配置首个资源索引文件？如何调整日志级别？如何通过 systemd 实现开机自启？ |
| 配置参考 | `docs/configuration.md` | 每个配置项的含义是什么？如何自定义元数据提取规则？如何设置不同的健康检查超时阈值？ |
| 插件开发 | `docs/plugin-dev.md` | 如何编写一个自定义输出钩子？如何注册新的元数据解析器？插件加载顺序如何控制？ |
| 运维手册 | `docs/operations.md` | 如何迁移索引数据？如何清理过期资源记录？如何通过 Prometheus 导出监控指标？ |
| API 接口 | `docs/api.md` | 预览服务提供了哪些 REST 端点？请求参数和响应结构是什么？如何通过 API 添加或删除资源？ |
| 故障排查 | `docs/troubleshooting.md` | 遇到 SSL 证书错误怎么办？某些页面永远返回 403 如何绕过？日志中出现大量超时警告如何处理？ |

## 资源列表

- http://wap.yidianmeii.cn/snews/639139.shtml
- http://h5.yidianmeii.cn/snews/66166.shtml
- http://wap.yidianmeii.cn/snews/660038.shtml
- http://h5.yidianmeii.cn/snews/8056.shtml
- http://3g.yidianmeii.cn/snews/911116.shtml
- http://h5.yidianmeii.cn/snews/63606.shtml
- http://wap.yidianmeii.cn/snews/37386.shtml
- http://wap.yidianmeii.cn/snews/0763.shtml
- http://wap.yidianmeii.cn/snews/70310.shtml
- http://3g.yidianmeii.cn/snews/86606.shtml

## 项目结构

```
yidian-resource-aggregator/
├── build_index.py            # 主构建脚本，读取资源列表生成索引文件
├── check_links.py            # 链接健康检查工具，输出 Markdown 报告
├── serve.py                  # 轻量级 Web 预览服务器启动入口
├── requirements.txt          # 运行时依赖清单（生产环境）
├── requirements-dev.txt      # 开发环境额外依赖（测试、格式化、类型检查）
├── src/                      # 核心源代码目录
│   ├── __init__.py           # 包初始化，导出主要 API
│   ├── fetcher.py            # 异步 HTTP 请求封装，包含重试与代理逻辑
│   ├── parser.py             # 元数据提取器，支持多种解析后端（lxml/html5lib）
│   ├── indexer.py            # 索引构建与序列化核心类
│   ├── checker.py            # 健康检查调度器，支持并发控制和结果聚合
│   └── exporter.py           # 输出格式转换：JSON、CSV、Markdown
├── plugins/                  # 官方插件目录
│   ├── __init__.py           # 插件自动发现机制
│   ├── wework_hook.py        # 企业微信机器人输出钩子示例
│   └── slack_hook.py         # Slack Webhook 输出钩子示例
├── templates/                # Jinja2 HTML 模板文件
│   ├── dashboard.html        # 主仪表板页面模板
│   └── partials/             # 可复用的页面片段（表格、状态标签等）
├── tests/                    # 单元测试与集成测试
│   ├── test_fetcher.py       # 覆盖 fetcher 模块的所有公共方法
│   ├── test_parser.py        # 针对不同 HTML 结构的解析测试用例
│   └── fixtures/             # 测试用的静态 HTML 样本文件
├── config/                   # 配置文件目录
│   ├── default.yaml          # 默认全局配置（超时、并发数、日志级别）
│   └── custom.yaml.example   # 用户自定义配置模板，可覆盖默认值
├── logs/                     # 日志文件存储目录（运行时动态生成）
│   ├── app.log               # 综合日志，记录所有级别信息
│   └── errors.log            # 仅记录 WARNING 及以上级别的错误事件
├── docs/                     # 完整文档，参见文档导航章节
└── LICENSE                   # MIT 许可证文本
```

## 贡献指南

1. 阅读 `CONTRIBUTING.md` 文档了解行为准则和开发流程，确保您的贡献符合项目的设计哲学和编码规范。建议先浏览 `docs/plugin-dev.md` 和 `docs/configuration.md` 以熟悉核心抽象概念。

2. 在 GitHub Issues 中查找标记为 `good-first-issue` 或 `help-wanted` 的任务，或在 Discussions 板块提出新功能建议，与维护者和其他贡献者讨论实现方案，避免重复劳动或设计偏差。

3. Fork 本仓库并创建功能分支，分支命名遵循 `feature/短描述` 或 `fix/问题编号` 格式。开发过程中请运行 `black .` 和 `mypy src/` 保持代码格式与类型一致性，并确保 `pytest` 测试套件全部通过。

4. 提交代码时编写清晰的 commit message，遵循 Conventional Commits 规范（如 `feat: 添加重试退避策略` 或 `fix: 修复解析空页面时的 None 异常`）。提交前请自行 rebase 到最新主干分支，减少合并冲突。

5. 发起 Pull Request 并填写完整模板，描述变更动机、实现方法和测试结果。PR 至少需要一位核心维护者批准，所有 CI 检查通过后方可合并。合并后您的名字将自动添加到 `CONTRIBUTORS.md` 列表中。

## 常见问题

**问：健康检查出现大量 SSL 证书错误，如何解决？**

答：部分老旧或非标准配置的源站可能使用自签名证书或过期证书。您可以在 `config/custom.yaml` 中将 `ssl_verify` 设置为 `false` 以全局禁用证书验证。但出于安全考虑，我们建议仅在测试环境使用此选项。生产环境下，请更新系统 CA 证书包或将特定域名的证书指纹加入信任列表。您也可以使用 `--ssl-disable` 命令行参数临时覆盖配置。

**问：如何添加新的资源条目而不重新构建整个索引？**

答：项目支持增量更新。您可以将新条目追加到 `resources.txt` 文件末尾，然后运行 `build_index.py --update` 模式。该模式会对比现有索引和新资源列表，仅处理新增或发生变化的条目，保留已有条目的元数据缓存，大幅减少处理时间。若需强制刷新所有条目，请使用 `--force` 参数。

**问：预览仪表板上的资源状态颜色代表什么含义？**

答：绿色表示资源在最近 24 小时内检查为可访问（HTTP 状态码 2xx）；黄色表示资源可访问但响应时间超过 5 秒，或状态码为 3xx 重定向；红色表示资源不可访问（4xx、5xx 或网络超时）；灰色表示尚未执行过检查或检查结果未知。鼠标悬停在状态指示器上可查看详细检查时间和具体 HTTP 状态码。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
