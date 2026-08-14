# LinkVault 开源资源聚合站

LinkVault 是一个面向技术团队、内容创作者和研究人员的高效外链管理与技术资源汇总平台。该项目并非传统的内容管理系统，而是一个轻量级、可自托管的链接资产索引器，专门用于对分散在多个内容源（如移动端新闻简报、技术博客、H5 活动页）中的 URL 进行结构化归档、元数据提取和快速检索。目标用户包括运维工程师、全栈开发者和数据采集工程师，旨在解决手工整理书签效率低下、链接失效无法追踪、以及跨设备访问碎片化资源时缺乏统一视图的痛点。

## 功能概览

- **批量链接导入与归一化清洗**：支持通过文本文件、CSV 或 API 批量提交原始 URL，自动去除跟踪参数（UTM 后缀）并进行域名归一化处理。
- **智能元数据抓取**：后台任务队列自动访问每个提交的链接，抓取页面标题、Meta 描述、主要文本摘要及最后修改时间，生成可搜索的索引。
- **状态监控与死链检测**：周期性回访已入库链接，标记 HTTP 状态码异常（404、502 等），并提供可视化健康仪表盘。
- **多维度标签分类**：允许用户为每个链接打上自定义标签（如 #前端、#安全、#AI），并支持标签组合的布尔检索。
- **公开快照与私有收藏夹**：支持将特定资源列表生成为只读快照页面，便于对外分享；同时保留用户私有的收藏夹功能，与团队空间隔离。
- **RSS 订阅与变更通知**：针对监控的链接源，当页面内容发生显著变化时，可通过 RSS 或邮件触发告警，适合竞品动态跟踪场景。
- **全文检索与高级筛选**：基于 SQLite FTS5 或 Elasticsearch 适配器，提供标题、正文片段、来源域名和入库时间范围的混合检索能力。

## 应用场景

- **技术团队内部知识库沉淀**：团队在日常开发中会产生大量临时性参考链接（如调试方案、依赖库文档）。使用 LinkVault 可将这些分散的链接集中归档，并自动提取摘要，避免优秀资源随聊天记录沉没。例如，将一周内团队群聊中分享的所有 Stack Overflow 链接批量导入，系统自动生成带标签的知识卡片。
- **移动端新闻或简报内容二次分发监控**：针对来自移动端聚合页（如 `3g.yidianmeii.cn` 或 `h5.yidianmeii.cn` 等域名下的动态新闻资源），LinkVault 可定期抓取指定入口页下的链接列表，监控其增删变化，用于内容聚合或竞品内容策略分析。
- **开源项目文档站外链接治理**：开源项目维护者可使用 LinkVault 管理项目 README 或 Wiki 中引用的所有站外参考链接。当外部文档迁移或失效时，系统能提前告警，帮助维护者及时更新文档，提升项目质量。
- **学术或行业研究报告资料整理**：研究人员在收集行业报告、白皮书或新闻事件的时间线样本时，可将大量临时采集的 URL 导入系统，利用标签和全文检索快速组织写作素材，并生成规范化的参考文献列表。

## 快速开始

以下步骤适用于 Linux / macOS 开发环境，建议使用 Python 3.10 及以上版本。

```bash
# 1. 克隆代码仓库
git clone https://github.com/your-org/linkvault.git
cd linkvault

# 2. 创建并激活 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate   # Windows 下使用 venv\Scripts\activate

# 3. 安装项目依赖（包含异步 HTTP 客户端、HTML 解析库、数据库驱动等）
pip install -r requirements.txt

# 4. 初始化 SQLite 数据库结构并创建默认管理员账户
python manage.py initdb
python manage.py createuser --username admin --password your_secure_pass

# 5. 启动开发服务器（默认监听 127.0.0.1:8000）
python manage.py runserver
```

访问 `http://127.0.0.1:8000` 即可进入控制台，开始导入链接或配置抓取任务。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Python | 3.10 - 3.12 | 核心运行时环境，推荐使用 3.11 以获得最佳性能 |
| SQLite | 3.35+ | 内置数据库，用于存储链接索引、元数据和用户配置，支持 JSON 字段 |
| aiohttp | 3.9.0+ | 异步 HTTP 客户端库，用于高并发抓取页面内容 |
| beautifulsoup4 | 4.12.0+ | HTML/XML 解析器，用于提取页面标题和正文摘要 |
| elasticsearch (可选) | 8.0+ | 如需启用高级全文检索，需额外部署 Elasticsearch 服务 |
| Redis (可选) | 7.0+ | 用于分布式任务队列和缓存状态，单机模式可跳过 |
| Docker | 20.10+ | 生产环境推荐容器化部署，提供一键编排脚本 |
| Nginx | 1.24+ | 生产环境反向代理与静态资源服务（可选） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
| :--- | :--- | :--- |
| 用户手册 | `/docs/user-guide/` | 如何批量导入链接？标签系统如何工作？如何生成公共快照页面？ |
| 管理员指南 | `/docs/admin-guide/` | 如何配置抓取频率？如何调整死链检测策略？用户权限如何管理？ |
| API 参考 | `/docs/api-reference/` | 提供哪些 RESTful 接口？如何通过 API 完成链接的增删改查和检索？ |
| 部署运维 | `/docs/deployment/` | 支持哪些部署方式（Docker、Systemd）？环境变量有哪些？如何备份数据？ |
| 开发者指南 | `/docs/developer-guide/` | 项目整体架构是怎样的？如何扩展新的内容解析器？如何编写自定义过滤器？ |

## 资源列表

- http://3g.yidianmeii.cn/vnews/0814/9677241.shtml
- http://wap.yidianmeii.cn/vnews/0814/122826.shtml
- http://3g.yidianmeii.cn/vnews/0814/1344.shtml
- http://h5.yidianmeii.cn/vnews/0814/9309703.shtml
- http://h5.yidianmeii.cn/vnews/0814/2990188.shtml
- http://3g.yidianmeii.cn/vnews/0814/087179.shtml
- http://wap.yidianmeii.cn/vnews/0814/622361.shtml
- http://wap.yidianmeii.cn/vnews/0814/29885.shtml
- http://3g.yidianmeii.cn/vnews/0814/56992.shtml
- http://h5.yidianmeii.cn/vnews/0814/2596365.shtml

## 项目结构

```
linkvault/
├── manage.py                 # 项目统一命令行入口（初始化、运行、迁移、用户管理）
├── requirements.txt          # Python 依赖列表（含生产与开发环境分组）
├── .env.example              # 环境变量配置模板（数据库路径、抓取并发数、密钥等）
├── src/                      # 核心源代码目录
│   ├── core/                 # 核心模块：应用配置、数据库连接、全局异常处理
│   │   ├── config.py         # 基于 Pydantic 的设置管理，支持 .env 覆盖
│   │   ├── database.py       # SQLite 连接池与 FTS5 索引管理
│   │   └── exceptions.py     # 自定义异常层次（抓取超时、解析失败、重复提交等）
│   ├── crawler/              # 抓取与解析引擎
│   │   ├── fetcher.py        # 异步 HTTP 请求器，支持重试、限流和 User-Agent 轮换
│   │   ├── parser.py         # HTML 解析器（基于 BeautifulSoup），提取标题、摘要、正文
│   │   └── scheduler.py      # 定时任务调度器，管理周期性回访与死链检测
│   ├── models/               # 数据模型层（ORM 映射）
│   │   ├── link.py           # Link 实体：URL、状态码、标题、摘要、最后访问时间
│   │   ├── tag.py            # 标签实体：名称、颜色、创建时间
│   │   └── user.py           # 用户与权限模型（管理员/普通成员/只读访客）
│   ├── web/                  # Web 控制台与服务端渲染视图
│   │   ├── routes/           # 路由蓝图（dashboard, links, tags, settings, api）
│   │   ├── templates/        # Jinja2 模板文件（列表页、详情页、管理面板）
│   │   └── static/           # 编译后的 CSS/JS 资源（基于 Tailwind 与 Alpine.js）
│   ├── services/             # 业务逻辑服务层
│   │   ├── link_service.py   # 链接增删改查、导入导出、状态更新逻辑
│   │   ├── search_service.py # 全文检索适配器（SQLite FTS5 或 Elasticsearch）
│   │   └── tag_service.py    # 标签关联与聚合统计服务
│   └── utils/                # 通用工具函数
│       ├── url_utils.py      # URL 归一化、域名提取、参数过滤
│       ├── time_utils.py     # 时间格式化与时区转换辅助
│       └── hash_utils.py     # 链接去重指纹计算（基于 URL 和标题相似度）
├── tests/                    # 单元测试与集成测试目录
│   ├── test_crawler/         # 抓取模块的 Mock 测试（模拟响应与超时）
│   └── test_models/          # 数据模型约束与数据库事务测试
├── scripts/                  # 运维辅助脚本（备份、数据迁移、批量导入示例）
│   ├── backup_db.sh          # 自动备份 SQLite 文件到指定目录
│   └── import_csv.py         # 从 CSV 批量导入链接的命令行工具
└── docker-compose.yml        # 生产级编排文件（包含应用、Redis、Elasticsearch 服务）
```

## 贡献指南

1. **提交 Issue 进行需求或缺陷讨论**：在提交代码之前，请先在 GitHub Issues 中创建新的话题，描述您想要解决的问题或新增的功能。对于缺陷报告，请务必提供详细的复现步骤、日志信息以及宿主环境（操作系统、Python 版本）。
2. **Fork 项目并创建特性分支**：将主仓库 Fork 到您的个人账户下，然后基于 `main` 分支创建您的开发分支，命名规范为 `feature/功能简述` 或 `fix/缺陷编号`。
3. **编写符合规范的代码与测试用例**：请遵循项目根目录下的 `.flake8` 和 `mypy.ini` 配置进行代码风格与类型检查。所有新增功能或修复必须包含对应的单元测试，确保测试覆盖率不低于 85%。
4. **编写或更新相关文档**：如果您的更改涉及用户可见的功能（如新增环境变量、修改 API 接口），请同步更新 `/docs` 目录下的对应文档，并在 Pull Request 中说明文档变更点。
5. **发起 Pull Request 并等待 Code Review**：提交 PR 时请清晰填写变更摘要，关联相关的 Issue 编号。项目维护者将在 3 个工作日内进行评审，可能会要求您调整代码或补充测试。

## 常见问题

**问：提交大量链接时，系统如何处理重复记录？**  
答：LinkVault 在入库前会对 URL 进行归一化处理（移除协议之外的末尾斜杠、排序查询参数、去除常见跟踪参数如 `utm_source`），然后基于归一化后的字符串生成 SHA-256 指纹。若指纹已存在，系统默认执行“覆盖更新”策略（更新标题、摘要和最后访问时间），而不会产生重复条目。您也可以在导入配置中选择“跳过”或“严格报错”模式。

**问：抓取任务是否会影响目标服务器的负载？**  
答：系统内置了自适应限流器。您可以在环境变量中设置 `CRAWLER_CONCURRENCY`（默认 5）和 `CRAWLER_DELAY`（请求间隔，默认 1 秒）。此外，系统遵守 `robots.txt` 协议，并支持配置 `CRAWLER_USER_AGENT` 以模拟不同客户端。对于内部或受信任的站点，您可以在后台将其加入“白名单”以解除限流约束。

**问：如何从旧版 SQLite 迁移数据到新版或切换到 PostgreSQL？**  
答：项目源码中提供了 `scripts/migrate_db.py` 脚本，支持从旧版 SQLite 导出 JSON 格式的数据备份，并重新导入到新版本的 SQLite 或 PostgreSQL 中。对于 PostgreSQL 的支持，您只需在 `.env` 中修改 `DATABASE_URL` 为 `postgresql://...` 格式，然后运行 `python manage.py db upgrade` 即可完成表结构迁移。请注意，在执行生产环境迁移前，务必先在测试环境进行完整演练。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
