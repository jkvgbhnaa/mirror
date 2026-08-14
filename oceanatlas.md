# Yidian News Aggregator

Yidian News Aggregator is a lightweight, high-performance news resource indexing and external link aggregation system designed for content curation teams, news monitoring analysts, and data journalism practitioners. This project addresses the critical need for centralized, structured access to distributed news content published across multiple subdomains of the yidianmeii.cn platform. By providing a unified entry point and standardized metadata extraction layer, it eliminates the manual effort of navigating disparate mobile and web endpoints while ensuring consistent content retrieval patterns.

The system operates as a read-only aggregation layer that consumes publicly available news articles from the yidianmeii.cn ecosystem. It does not store or republish copyrighted content but instead maintains a searchable index of article metadata, including titles, publication timestamps, section classifications, and permanent URLs. This approach respects the original content providers' rights while offering researchers and analysts a programmable interface for trend detection, topic clustering, and temporal pattern analysis across the news corpus.

## 功能概览

**多端点统一接入** - 自动适配 3g、h5、wap 三个移动端子域名的内容结构差异，屏蔽底层请求细节。

**批量链接规范化处理** - 对原始新闻 URL 进行标准化清洗，自动去除冗余查询参数并补充必要的请求头信息。

**元数据智能提取** - 从每篇新闻页面中抽取标题、发布时间、正文摘要、来源栏目及关联标签，支持自定义字段映射。

**定时轮询与增量更新** - 内置基于 cron 表达式的调度器，可按小时或天粒度自动拉取新增新闻条目，避免全量重复扫描。

**去重与历史归档** - 基于 URL 指纹和内容哈希双重去重机制，已处理的新闻链接自动进入归档库，支持回溯查询。

**结构化数据导出** - 支持将索引结果输出为 JSON Lines、CSV 或 Parquet 格式，便于下游数据分析流水线集成。

## 应用场景

**新闻舆情监控** - 公关公司和品牌部门可配置系统每日定时抓取指定时间段内的新闻报道，自动生成舆情简报，实时追踪媒体提及量和情感倾向变化。

**竞品动态追踪** - 市场分析团队通过订阅特定栏目或关键词，系统自动推送相关新闻更新，帮助快速识别行业竞品的市场活动、产品发布及合作动态。

**历史新闻归档检索** - 新闻机构和档案管理部门利用系统的增量归档能力，对 yidianmeii.cn 平台上的历史报道进行系统性整理，构建可检索的时间序列数据库。

**学术研究与内容分析** - 传播学和社会学研究者可通过系统导出的结构化数据集，进行内容主题建模、报道框架分析和媒体议程设置研究，支持大规模量化内容分析。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/your-organization/yidian-news-aggregator.git

# 进入项目目录
cd yidian-news-aggregator

# 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# 复制示例配置文件并修改数据库连接等参数
cp config.example.yaml config.yaml

# 初始化 SQLite 数据库表结构
python scripts/init_db.py --config config.yaml

# 执行一次全量抓取测试（默认抓取最近 3 天数据）
python runner.py --mode full --days 3

# 启动定时调度服务（后台运行）
nohup python scheduler.py --config config.yaml > logs/scheduler.log 2>&1 &
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行环境，推荐 3.10 或 3.11 以获得最佳性能 |
| SQLite | 3.35.0 及以上 | 默认内嵌数据库，用于元数据索引存储，生产环境可切换到 PostgreSQL |
| requests | 2.28.0 及以上 | HTTP 请求库，处理所有对外部新闻端点的网络请求 |
| beautifulsoup4 | 4.11.0 及以上 | HTML 解析库，用于新闻页面内容的结构化提取 |
| pyyaml | 6.0 及以上 | YAML 配置文件解析，管理所有运行时参数 |
| schedule | 1.2.0 及以上 | 轻量级任务调度库，驱动定时抓取流程 |
| pandas | 2.0.0 及以上 | 数据导出和转换层，支持 DataFrame 操作和多种格式输出 |
| lxml | 4.9.0 及以上 | 高性能 XML/HTML 解析器，作为 beautifulsoup4 的解析后端 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting_started.md | 如何快速配置运行环境并完成第一次数据抓取？ |
| 配置参考 | docs/configuration.md | config.yaml 中每个参数的具体含义和可选值是什么？ |
| API 接口 | docs/api_reference.md | 系统提供了哪些编程接口用于查询和导出索引数据？ |
| 调度与运维 | docs/deployment.md | 如何将系统部署为长期运行的后台服务并进行健康检查？ |
| 数据字典 | docs/data_schema.md | 索引表中每个字段的类型、来源和业务含义是什么？ |
| 性能调优 | docs/performance.md | 当新闻量超过百万级时，如何优化抓取速度和存储效率？ |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/868176.shtml
- http://h5.yidianmeii.cn/jnews/0814/331611.shtml
- http://3g.yidianmeii.cn/jnews/0814/1117.shtml
- http://3g.yidianmeii.cn/jnews/0814/3168.shtml
- http://h5.yidianmeii.cn/jnews/0814/9116693.shtml
- http://wap.yidianmeii.cn/jnews/0814/9918.shtml
- http://h5.yidianmeii.cn/jnews/0814/9603873.shtml
- http://3g.yidianmeii.cn/jnews/0814/331901.shtml
- http://3g.yidianmeii.cn/jnews/0814/36151.shtml
- http://3g.yidianmeii.cn/jnews/0814/1511563.shtml

## 项目结构

```
yidian-news-aggregator/
├── config/                          # 配置目录
│   ├── config.yaml                 # 主配置文件，含抓取参数、调度规则、数据库连接
│   └── logging.yaml                # 日志级别和输出格式配置
├── core/                           # 核心业务逻辑
│   ├── fetcher.py                  # 多端点请求分发器，处理 3g/h5/wap 的差异化请求
│   ├── parser.py                   # 新闻页解析引擎，基于 BeautifulSoup 提取元数据
│   ├── deduplicator.py             # 布隆过滤器 + 哈希双重去重实现
│   └── indexer.py                  # 索引写入与查询接口，封装数据库操作
├── adapters/                       # 子域名适配层
│   ├── base.py                     # 适配器抽象基类
│   ├── mobile_3g.py                # 3g.yidianmeii.cn 端点适配器
│   ├── mobile_h5.py                # h5.yidianmeii.cn 端点适配器
│   └── wap.py                      # wap.yidianmeii.cn 端点适配器
├── scheduler/                      # 调度与任务管理
│   ├── cron_parser.py              # cron 表达式解析与下次执行时间计算
│   ├── job_manager.py              # 任务队列管理与异常重试机制
│   └── runner.py                   # 单次抓取任务执行入口
├── scripts/                        # 运维与工具脚本
│   ├── init_db.py                  # 数据库表结构初始化
│   ├── export.py                   # 数据导出工具，支持 JSON/CSV/Parquet
│   └── cleanup.py                  # 过期日志与临时文件清理
├── tests/                          # 单元测试与集成测试
│   ├── test_fetcher.py             # 请求分发模块测试
│   ├── test_parser.py              # 解析引擎测试，含各类 HTML 结构样本
│   └── fixtures/                   # 测试用静态 HTML 样本文件
├── docs/                           # 详细文档（见文档导航章节）
├── logs/                           # 运行时日志输出目录
├── data/                           # 本地数据缓存与归档存储
├── requirements.txt                # Python 依赖清单
├── setup.py                        # 项目安装脚本
└── README.md                       # 本文件
```

## 贡献指南

1. 在 GitHub 上 fork 本仓库到个人账号，并 clone 到本地开发环境。确保使用 Python 3.9+ 并创建独立的虚拟环境以隔离依赖。

2. 在本地新建功能分支，分支命名遵循 `feature/功能简述` 或 `fix/问题简述` 格式。提交代码前请运行 `pytest tests/` 确保所有现有测试用例通过，并针对新增功能补充对应的单元测试。

3. 若涉及适配新子域名或修改解析逻辑，请提供至少三个真实新闻页面的 URL 样本作为测试依据，并更新 `adapters/` 下的对应适配器类。所有对外请求必须设置合理的超时和重试策略。

4. 更新 `docs/` 中相关文档，特别是配置参数说明和数据字典。若修改了配置文件结构，请同步更新 `config.example.yaml` 并添加注释说明新参数的作用域和默认值。

5. 提交 Pull Request 到主仓库的 `develop` 分支，PR 描述中需清晰说明修改目的、影响范围及测试覆盖情况。核心贡献者将在 48 小时内进行 Code Review 并合并。

## 常见问题

**Q: 系统是否支持抓取 yidianmeii.cn 之外的新闻站点？**

A: 当前版本仅针对 yidianmeii.cn 的三个子域名进行了适配（3g、h5、wap）。如需扩展至其他新闻源，需要继承 `adapters.base.BaseAdapter` 并实现 `fetch()` 和 `parse()` 方法，随后在 `core/fetcher.py` 中注册新的适配器。社区计划在 v2.0 版本中提供通用的 RSS 和 Atom 协议支持。

**Q: 抓取频率过高是否会被目标服务器封禁？**

A: 系统默认采用礼貌爬取策略，请求间隔不低于 3 秒，并自动读取目标站点的 robots.txt 规则。调度器默认配置为每小时最多执行一次全量扫描。用户可通过 `config.yaml` 中的 `rate_limit` 参数调整请求间隔，建议生产环境保持默认值以避免触发服务器限流。如果遇到 429 状态码，系统会触发指数退避重试，最长等待 5 分钟后再次尝试。

**Q: 历史数据如何迁移或备份？**

A: 系统使用 SQLite 作为默认存储，数据库文件位于 `data/index.db`。用户可直接复制该文件进行冷备份。如需热备份或迁移至 PostgreSQL，可在 `config.yaml` 中切换 `database` 连接串为 PostgreSQL 格式，系统启动时会自动创建对应表结构。数据导出可使用 `python scripts/export.py --format csv --output backup.csv` 命令将所有索引记录导出为通用格式。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
