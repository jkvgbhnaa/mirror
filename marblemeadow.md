# ResourceBridge

ResourceBridge 是一个面向技术内容聚合与外链治理的开源元数据汇总平台，专为需要系统化整理、校验、归档大量外部资讯链接的开发者与内容运营团队设计。该项目不直接存储新闻或文章全文，而是通过可复用的结构化索引机制，将分散于多个子域名的异构资源条目统一归集，并提供命令行与 Web 两种校验视图，帮助团队快速识别链接可访问性、响应时间波动及域名归属变化。

ResourceBridge 定位为“资源链接的桥接层”，适用于内部知识库维护、第三方资讯源监控、批量链接迁移前的健康预检等场景。项目核心假设是：链接本身是易变的，但链接的元数据（来源域名、路径模式、状态码、内容哈希）是可治理的。基于这一假设，ResourceBridge 提供了一套轻量级抓取调度与结果对比框架，不对底层内容做语义理解，仅关注链接的物理可达性与结构一致性。

## 功能概览

- 批量链接可达性探测：支持并发 HEAD 与 GET 请求，自动记录状态码、响应时间、重定向链长度，输出结构化 JSON 报告。
- 域名与路径模式归类：自动解析 URL 中的主机名与路径首段，按来源子域名（如 3g、wap、h5）聚合统计，生成域名分布热力图数据。
- 增量变更检测：基于历史运行记录对比同一批次链接在上次检查与本次检查之间的状态差异，标记新增失效、恢复可用、响应时间恶化三类变化。
- 原始链接直出模式：提供 `--raw` 输出选项，将输入列表原样格式化输出，不做任何协议补全或域名规范化，满足第三方系统对接时的严格原文要求。
- 检查结果邮件摘要：集成 SMTP 通知能力，每次批量检查结束后自动发送精简摘要至指定邮箱，包含总链接数、异常数、Top3 慢速链接。
- 配置文件热加载：支持 YAML 格式的检查策略配置（超时阈值、并发数、重试次数），修改配置后无需重启服务即可生效。
- 多格式导出：检查结果可导出为 CSV、JSON、Markdown 表格三种格式，便于导入电子表格或文档系统进一步处理。

## 应用场景

- 资讯聚合平台的内容源监控：运营团队每日需确认数百条外部引用链接是否仍可访问。ResourceBridge 可配置为定时任务，每日凌晨自动巡检，早晨上班前将异常链接列表推送至企业微信群或邮件，减少人工点检时间。
- 网站迁移前的链接资产盘点：在进行域名合并或 CMS 升级前，使用 ResourceBridge 对全站所有外链进行可达性扫描，生成基线报告。迁移后再次扫描，对比两次结果即可快速定位因路径变更导致的断链。
- 内部知识库的链接健康周报：技术文档团队每周将知识库内所有外链导出为文本文件，由 ResourceBridge 进行批量检查，生成的 Markdown 报告直接追加至周报文档中，供管理层评估文档质量。
- 第三方数据源可用性仲裁：当多个数据源提供相同主题的链接但部分失效时，ResourceBridge 可并行检查所有候选链接，根据响应时间和状态码自动推荐最优可用源，供下游数据管道选择。

## 快速开始

```bash
# 克隆仓库
git clone https://github.com/resource-bridge/resource-bridge.git
cd resource-bridge

# 安装依赖（使用 Python 3.10+ 虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 运行一次快速检查（使用项目自带的示例链接列表）
python bridge.py check --input samples/example_urls.txt --output reports/ --format json
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.10 及以上 | 核心运行环境，低于 3.10 将无法使用 match-case 语法及部分 typing 特性 |
| requests | 2.28.0 及以上 | 用于发送 HTTP 请求，支持连接池复用与超时精细控制 |
| pyyaml | 6.0 及以上 | 解析检查策略配置文件，支持多文档流 |
| jinja2 | 3.1.0 及以上 | 用于渲染 Markdown 格式的检查报告模板 |
| pytest | 7.0.0 及以上 | 仅开发测试时需要，用于运行单元测试与集成测试套件 |
| docker | 20.10.0 及以上 | 如需使用容器化部署方式，需要 Docker Engine 或兼容容器运行时 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | docs/getting_started.md | 如何安装、配置第一个检查任务、理解输出报告的基本字段含义 |
| 配置参考 | docs/configuration.md | YAML 配置文件中每个字段的作用、默认值、可取值范围及典型配置案例 |
| 输出格式规范 | docs/output_formats.md | JSON/CSV/Markdown 三种输出格式的字段定义、数据类型及示例解析 |
| 故障排查 | docs/troubleshooting.md | 常见错误码含义、网络超时处理策略、SSL 证书验证问题解决方案 |

## 资源列表

- http://3g.yidianmeii.cn/snews/7867.shtml
- http://wap.yidianmeii.cn/snews/971594.shtml
- http://wap.yidianmeii.cn/snews/03207.shtml
- http://3g.yidianmeii.cn/snews/0975698.shtml
- http://wap.yidianmeii.cn/snews/9254.shtml
- http://wap.yidianmeii.cn/snews/3423885.shtml
- http://3g.yidianmeii.cn/snews/42807.shtml
- http://h5.yidianmeii.cn/snews/0795469.shtml
- http://h5.yidianmeii.cn/snews/78374.shtml
- http://3g.yidianmeii.cn/snews/077275.shtml

## 项目结构

```
resource-bridge/
├── bridge.py                 # CLI 入口，解析子命令并调度检查、报告、对比等流程
├── config/
│   ├── default.yaml          # 默认检查策略（超时 10s，并发 50，重试 2 次）
│   └── production.yaml       # 生产环境推荐配置（超时 5s，并发 100，重试 3 次）
├── core/
│   ├── checker.py            # 核心检查引擎，实现并发请求、重试逻辑、重定向追踪
│   ├── parser.py             # URL 解析与规范化工具（但保留原始输出模式）
│   └── differ.py             # 两次检查结果的增量对比算法
├── output/
│   ├── json_exporter.py      # 输出 JSON 格式结果，保留完整元数据
│   ├── csv_exporter.py       # 输出扁平 CSV，适合 Excel 打开
│   └── markdown_exporter.py  # 输出人类可读的 Markdown 表格报告
├── notifiers/
│   ├── smtp_notifier.py      # 通过 SMTP 发送摘要邮件
│   └── webhook_notifier.py   # 通过自定义 Webhook 推送结果（预留接口）
├── tests/
│   ├── unit/                 # 单元测试（解析、对比、导出等独立模块）
│   └── integration/          # 集成测试（端到端检查真实 URL，使用 mock 网络）
├── samples/
│   └── example_urls.txt      # 示例链接列表，供快速体验使用
└── requirements.txt          # Python 依赖列表（含版本锁定）
```

## 贡献指南

1. 阅读项目贡献规范文件 CONTRIBUTING.md（位于仓库根目录），了解提交信息格式、分支命名规则及测试覆盖率要求。
2. 在 GitHub Issues 中查找标记为 `good-first-issue` 或 `help-wanted` 的任务，评论认领后等待维护者分配。
3. 本地创建特性分支，命名格式为 `feature/简短描述` 或 `fix/问题编号`，开发完成后确保所有单元测试与集成测试通过。
4. 提交 Pull Request 时附带检查报告截图或日志片段，说明变更带来的性能影响或功能改进点。
5. 代码审查通过后，维护者将合并至主分支，并在 CHANGELOG.md 中记录变更条目。

## 常见问题

**问：检查大量链接时出现 `Too Many Open Files` 错误怎么办？**

答：这是由于系统文件描述符限制过低导致的。请调整配置文件中 `concurrency` 参数的值（建议降至 20 以下），或通过 `ulimit -n 4096` 提高系统限制。若使用 Docker 运行，请在启动时添加 `--ulimit nofile=4096:4096` 参数。

**问：部分链接返回 403 但浏览器中能正常打开，如何解决？**

答：某些源站会校验 `User-Agent` 请求头。请在配置文件的 `headers` 字段中自定义 `User-Agent` 为常见浏览器标识（如 Chrome 或 Firefox 的最新版本），并开启 `cookies` 支持。若仍被拒绝，可尝试降低并发数，避免触发源站的反爬阈值。

**问：如何只检查链接的可达性而不下载任何响应体？**

答：在检查策略配置中设置 `method: HEAD` 即可。HEAD 请求仅获取响应头，不传输响应体，可大幅减少带宽消耗和检查时间。但请注意，少数源站不支持 HEAD 方法，此时会自动降级为 GET 并设置 `stream: true` 仅读取前 1024 字节后断开。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
