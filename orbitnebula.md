# WebLink Aggregator Station

WebLink Aggregator Station 是一个面向技术调研、内容审核与数据标注团队的外链资源集中管理平台。该项目将分散于多个移动端入口（H5、WAP、3G）的新闻资讯类外链进行统一抓取、结构化存储与状态监控，解决跨站点资源分散、链接失效难追踪、人工整理效率低下的问题。目标用户包括运维工程师、爬虫开发者、内容运营人员及安全分析团队。

## 功能概览

- **多子域入口聚合**：统一管理 yidianmeii.cn 域名下 h5、wap、3g 三个子域每日发布的新闻资源路径。
- **链接健康状态检测**：定期对收录的 URL 发起 HEAD/GET 请求，记录状态码、响应时间与重定向链。
- **批次化资源管理**：支持按日期（如 0814）和批次（如 24/90）对链接进行分组、标记与导出。
- **元信息自动提取**：从目标页面抽取标题、发布时间、正文摘要及关键字段，生成结构化元数据。
- **变更订阅通知**：当链接内容发生显著变化（如 404、跳转、标题变更）时，通过 Webhook 发送告警。
- **只读镜像归档**：对关键链接生成静态 HTML 快照，防止源站下线导致内容不可追溯。
- **标签与检索系统**：支持为每条链接添加自定义标签（如“科技”“辟谣”“公告”），并基于元数据快速筛选。
- **访问统计看板**：展示各子域来源占比、每日新增趋势、异常链接比例等可视化指标。

## 应用场景

1. 内容风控审核前准备：审核团队在每日开工前，通过本平台拉取当日新增外链清单，按子域分类批量预览，提前识别高风险域名或异常短链，提升审核效率。

2. 爬虫任务编排：爬虫开发者将本平台作为种子 URL 源，通过 REST API 获取指定批次的所有链接，避免手动复制粘贴导致遗漏，同时可结合健康检测结果跳过已失效的种子。

3. 历史链接回溯审计：数据标注人员需要核对三个月前某批链接的原始内容时，可直接访问平台的镜像归档或元数据记录，无需重新漫游互联网查找原网页。

4. 竞品内容监控：运营人员将竞品相关的新闻链接统一收录至平台，设置变更订阅，一旦标题或正文出现关键词变动，第一时间收到通知，便于快速响应。

## 快速开始

```bash
# 克隆仓库
git clone https://github.com/your-org/weblink-aggregator-station.git
cd weblink-aggregator-station

# 安装依赖（Python 3.10+）
pip install -r requirements.txt

# 初始化数据库（SQLite）
python scripts/init_db.py

# 运行开发服务器（默认端口 8000）
python app.py --port 8000

# 或使用 Docker 一键启动
docker-compose up -d
```

访问 http://localhost:8000 进入管理面板。默认管理员账号 admin，密码从环境变量 ADMIN_PASSWORD 读取，若未设置则首次启动时打印在日志中。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 及以上 | 核心运行时，建议使用 3.11+ |
| SQLite | 3.35.0 及以上 | 内置数据库，用于存储链接元数据与任务队列 |
| Redis | 6.0 及以上 | 可选，用于分布式锁和缓存，单机模式可禁用 |
| requests | 2.31.0 | HTTP 请求库，用于健康检测与内容抓取 |
| beautifulsoup4 | 4.12.0 | HTML 解析，用于提取标题、摘要等元信息 |
| lxml | 4.9.0 | 作为 BeautifulSoup 的解析器后端，提升解析速度 |
| pytest | 7.4.0 | 单元测试框架（仅开发环境） |
| docker | 20.10.0 | 容器化部署（生产环境） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user_guide.md | 如何登录、录入链接、查看健康状态、导出报表？ |
| 管理员指南 | /docs/admin_guide.md | 如何配置 Webhook、调整检测频率、管理用户权限？ |
| API 参考 | /docs/api_reference.md | 如何通过 REST API 获取批次列表、提交新链接、查询元数据？ |
| 部署运维 | /docs/deployment.md | 如何用 Docker Compose 部署高可用集群、配置反向代理与 SSL？ |
| 开发贡献 | /docs/contributing.md | 代码风格、提交规范、测试流程、PR 评审标准是什么？ |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/15686.shtml
- http://h5.yidianmeii.cn/jnews/0814/830398.shtml
- http://wap.yidianmeii.cn/jnews/0814/32141.shtml
- http://wap.yidianmeii.cn/jnews/0814/0196.shtml
- http://h5.yidianmeii.cn/jnews/0814/55236.shtml
- http://h5.yidianmeii.cn/jnews/0814/8047.shtml
- http://wap.yidianmeii.cn/jnews/0814/7823419.shtml
- http://wap.yidianmeii.cn/jnews/0814/63239.shtml
- http://h5.yidianmeii.cn/jnews/0814/3399.shtml
- http://h5.yidianmeii.cn/jnews/0814/00340.shtml

## 项目结构

```
weblink-aggregator-station/
├── app.py                          # 主入口，Flask 应用启动与路由注册
├── requirements.txt                # Python 依赖清单
├── docker-compose.yml              # 容器编排配置（app + redis + worker）
├── Dockerfile                      # 生产环境镜像构建脚本
├── config/
│   ├── default.py                  # 默认配置（端口、数据库路径、检测间隔）
│   ├── production.py               # 生产覆盖配置（密钥、日志级别）
│   └── development.py              # 开发覆盖配置（调试模式、热重载）
├── core/
│   ├── fetcher.py                  # 并发抓取器，控制重试、超时、代理
│   ├── parser.py                   # 基于 BeautifulSoup 的元信息提取器
│   ├── health.py                   # 健康状态检测与历史记录对比
│   └── mirror.py                   # 静态镜像生成与存储管理
├── models/
│   ├── link.py                     # Link 模型：url, status, title, batch, subdomain
│   ├── batch.py                    # Batch 模型：批次号、日期、来源子域列表
│   ├── tag.py                      # Tag 模型：标签名、关联链接数
│   └── snapshot.py                 # Snapshot 模型：镜像路径、生成时间、文件大小
├── services/
│   ├── scheduler.py                # 基于 APScheduler 的定时任务（每日检测）
│   ├── notifier.py                 # Webhook 通知服务（企业微信/飞书/钉钉）
│   └── exporter.py                 # 链接导出为 CSV/JSON 格式
├── api/
│   ├── v1/
│   │   ├── links.py                # /api/v1/links 增删改查端点
│   │   ├── batches.py              # /api/v1/batches 批次列表与详情
│   │   └── stats.py                # /api/v1/stats 聚合统计数据
│   └── middleware.py               # CORS、日志、异常捕获中间件
├── web/
│   ├── templates/                  # Jinja2 模板（仪表盘、列表页、详情页）
│   ├── static/                     # 静态资源（CSS、JS、图表库）
│   └── views.py                    # 页面路由与上下文处理器
├── scripts/
│   ├── init_db.py                  # 初始化 SQLite 表结构与索引
│   ├── seed_batch.py               # 从命令行导入指定批次链接（如 24/90）
│   └── cleanup_orphans.py          # 清理未关联批次或失效镜像的垃圾数据
├── tests/
│   ├── unit/                       # 单元测试（fetcher, parser, health）
│   ├── integration/                # 集成测试（API 端点、数据库事务）
│   └── fixtures/                   # 测试用的样本 HTML 与预期输出
└── logs/                           # 运行时日志（按日期轮转，保留 30 天）
```

## 贡献指南

1. 阅读文档导航中的开发贡献手册（/docs/contributing.md），确保理解代码风格（PEP 8 + Black）与提交规范（Conventional Commits）。

2. 在 Issues 区认领未分配的任务或提出新特性讨论，等待维护者标记“help wanted”或“approved”后再开始编码，避免重复或无效工作。

3. 从 develop 分支切出特性分支（命名格式 feature/功能简述 或 fix/问题描述），开发过程中保持单测覆盖率不低于 85%，并确保所有现有测试通过。

4. 提交 Pull Request 前，在本地执行预提交钩子（pre-commit）进行静态检查，并在 PR 描述中关联相关 Issue 编号，说明变更内容与测试结果。

5. 等待至少一位维护者 Code Review，根据反馈进行修改或补测。合并后及时清理已合并的特性分支，保持仓库整洁。

## 常见问题

**Q：如何导入一个已有批次的外链清单，而不是逐条手工添加？**

A：平台支持 CSV 和纯文本（每行一个 URL）批量导入。在管理后台的“批次管理”页面，选择“导入文件”，指定批次号（如 24/90）后上传文件即可。系统会自动去重、校验 URL 格式，并触发首轮健康检测。若需通过 API 导入，可调用 POST /api/v1/batches/{batch_id}/links，请求体携带 URLs 数组。

**Q：健康检测显示某个链接为异常，但实际浏览器中可正常访问，原因是什么？**

A：可能原因包括：1) 目标网站启用了反爬机制（如 User-Agent 检测、Cookie 校验、JavaScript 渲染），而平台的 fetcher 默认使用简单 UA。2) 网络环境差异（例如平台部署在海外服务器，而源站对国内 IP 有特殊内容分发）。3) 响应超时设置过短。请先尝试在配置中调整 fetcher 的 headers（模拟移动端）、延长超时时间或添加代理。如果问题持续，可在页面中标记“忽略”并手动记录备注。

**Q：平台是否支持多用户隔离数据？**

A：社区版默认采用单租户模式，所有用户共享同一链接库。如需多租户隔离，请参考企业版部署方案（另行咨询）。社区用户可以通过创建不同批次（如按团队名称命名批次）配合标签过滤，实现逻辑上的数据划分，并利用导出功能按批次剥离数据。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
