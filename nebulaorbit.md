# Yidian Hyperlink Aggregator

Yidian Hyperlink Aggregator 是一个面向技术内容聚合与快速信息检索的开源外链管理工具。项目定位为轻量级技术资源导航站，主要服务于需要频繁查阅分散技术资讯、开发文档与行业动态的开发者、技术运营人员及研究爱好者。该项目通过结构化编排外部优质链接，配合本地化的关键词映射与分类标签体系，显著降低从海量信息中定位有效内容的成本。

本项目不提供爬虫或自动化采集功能，仅作为人工筛选后链接的规范化存储与展示层。核心目标在于帮助个人或小团队建立私有的高质量技术外链库，并以清晰、可维护的方式对外分享或内部使用。项目本身不存储任何第三方内容，所有实际信息均以超链接形式指向原始出处。

## 功能概览

- **链接分类仓储**：支持对收录的每一枚外链赋予多级分类标签，例如“前端工程化”“后端性能”“移动端适配”等，便于按主题快速过滤。
- **元数据提取摘要**：对每一条链接可附加标题、简短描述、收录时间与重要程度评分，形成结构化的链接元信息表。
- **多终端适配预览**：针对移动端、平板与桌面端分别优化链接列表的渲染布局，内置响应式样式模板。
- **全文标题检索**：基于本地存储的链接标题与描述字段，提供毫秒级的关键词模糊匹配检索，不依赖外部搜索引擎。
- **批量导入导出**：支持 CSV 与 JSON 格式的链接批量导入、导出，便于迁移数据或与其他工具集成。
- **访问热度统计**：可选开启本地计数模块，记录每个链接被点击的次数，辅助判断内容热度。
- **只读只写权限分离**：提供基础的身份验证中间件，支持配置只读访客与可编辑管理员两种角色。
- **定时健康检查**：可配置定时任务，对已收录链接进行 HTTP 状态检测，标记失效或重定向的链接。

## 应用场景

- **技术团队内部知识库导航**：研发团队可将日常遇到的优质技术博客、官方文档、调试工具链接统一收录至 Yidian Hyperlink Aggregator，替代杂乱无章的浏览器书签。新人入职时通过该导航站快速了解团队常用技术栈与参考资源。
- **开源项目推荐页**：开源项目维护者可使用本项目构建项目的“生态资源推荐”页面，集中展示相关插件、教程、视频及社区讨论链接，提升项目周边的信息凝聚力。
- **技术资讯周报生成**：运营人员每周将筛选出的技术热点文章链接录入系统，利用导出功能生成 JSON 数据，再配合模板引擎自动生成周报邮件或公众号推文素材。
- **个人学习路径管理**：学习者可按“语言基础”“框架进阶”“架构设计”“性能优化”等阶段分类存放学习资料，配合检索功能快速复习特定知识点下的所有收藏链接。

## 快速开始

以下步骤帮助您在本地环境中快速启动 Yidian Hyperlink Aggregator 服务。

```bash
# 克隆项目仓库
git clone https://github.com/yidian-open/yidian-hyperlink-aggregator.git
cd yidian-hyperlink-aggregator

# 安装依赖（使用 npm）
npm install

# 配置环境变量，复制示例配置文件
cp .env.example .env

# 初始化本地 SQLite 数据库（默认路径为 ./data/links.db）
npm run init-db

# 启动开发服务器，默认监听端口 3000
npm run dev
```

启动成功后，浏览器访问 `http://localhost:3000` 即可看到链接列表首页。管理员后台路径为 `/admin`，默认管理员账号密码请查看 `.env` 文件中的初始值，首次登录后强烈建议修改。

## 安装要求

| 依赖 | 必需 | 说明 |
|---|---|---|
| Node.js >= 18.0.0 | 是 | 运行时环境，需支持 ES2022 语法 |
| npm >= 9.0.0 或 yarn >= 1.22.0 | 是 | 包管理工具，用于安装项目依赖 |
| SQLite 3 | 是 | 嵌入式数据库，用于存储链接元数据及分类信息，无需额外安装服务 |
| 操作系统 | 否 | 跨平台支持 Windows / macOS / Linux，生产环境推荐 Linux 内核 4.0+ |
| 内存 >= 512 MB | 否 | 最低运行内存要求，实际占用随链接数量线性增长，建议 1GB 以上获得更好检索性能 |
| 磁盘剩余空间 >= 200 MB | 否 | 用于存放数据库文件及日志，每条链接元数据占用约 2-4 KB |
| 网络出站访问 | 否 | 健康检查功能需要出站 HTTP/HTTPS 访问权限，若内网部署可关闭该功能 |
| 浏览器 | 否 | 访问管理界面需现代浏览器（Chrome 90+ / Firefox 88+ / Edge 90+） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | /docs/getting-started.md | 如何快速完成安装、初始配置并运行第一个实例；如何添加第一条链接 |
| 功能手册 | /docs/features/link-management.md | 如何批量导入导出、如何设置分类标签、如何启用访问统计与健康检查 |
| 管理指南 | /docs/admin/deployment.md | 生产环境部署建议（Nginx 反向代理、PM2 进程守护、SQLite 备份策略） |
| 接口参考 | /docs/api/rest-api.md | 后端 RESTful API 的端点定义、请求参数与响应示例，供二次开发或集成调用 |
| 自定义开发 | /docs/development/theming.md | 如何修改前端页面样式、添加新的链接元数据字段或扩展分类层级逻辑 |

## 资源列表

- http://3g.yidianmeii.cn/snews/087179.shtml
- http://wap.yidianmeii.cn/snews/622361.shtml
- http://wap.yidianmeii.cn/snews/29885.shtml
- http://3g.yidianmeii.cn/snews/56992.shtml
- http://h5.yidianmeii.cn/snews/2596365.shtml
- http://wap.yidianmeii.cn/snews/454598.shtml
- http://wap.yidianmeii.cn/snews/02798.shtml
- http://3g.yidianmeii.cn/snews/9812082.shtml
- http://3g.yidianmeii.cn/snews/9856018.shtml
- http://wap.yidianmeii.cn/snews/6555893.shtml

## 项目结构

```
yidian-hyperlink-aggregator/
├── src/                           # 核心源代码目录
│   ├── server/                    # 服务端入口与路由层
│   │   ├── index.js               # Express 应用启动入口，加载中间件与路由
│   │   └── routes/                # REST API 路由定义（links、categories、health）
│   ├── controllers/               # 业务控制器，处理请求参数校验与响应封装
│   │   ├── linkController.js      # 链接增删改查及检索逻辑
│   │   └── categoryController.js  # 分类树管理逻辑
│   ├── models/                    # 数据模型层，封装 SQLite 表操作
│   │   ├── linkModel.js           # 链接表（id, title, url, description, category_id, clicks, created_at）
│   │   └── categoryModel.js       # 分类表（id, name, parent_id, sort_order）
│   ├── services/                  # 外部服务集成与工具函数
│   │   ├── healthCheckService.js  # 定时 HTTP 状态检测任务调度
│   │   └── importExportService.js # CSV/JSON 批量导入导出流处理
│   └── public/                    # 前端静态资源（CSS、客户端 JavaScript）
│       ├── css/                   # 响应式布局样式（mobile-first 设计）
│       └── js/                    # 前端列表渲染与检索交互逻辑
├── data/                          # 数据存储目录（默认）
│   └── links.db                   # SQLite 主数据库文件（自动创建）
├── docs/                          # 完整项目文档（入门、功能、部署、API、开发）
│   ├── getting-started.md
│   ├── features/
│   ├── admin/
│   ├── api/
│   └── development/
├── tests/                         # 单元测试与集成测试用例
│   ├── unit/                      # 模型层与控制器的独立单元测试
│   └── integration/               # API 端到端测试（使用 supertest）
├── scripts/                       # 辅助脚本（数据库初始化、迁移、种子数据填充）
│   ├── init-db.js                 # 建表与默认分类初始化
│   └── seed-demo-links.js         # 插入演示链接数据
├── .env.example                   # 环境变量参考模板（端口、数据库路径、管理员凭证）
├── package.json                   # 项目依赖清单与脚本命令
├── README.md                      # 项目概览文档（即本文件）
└── LICENSE                        # MIT 许可证文件
```

## 贡献指南

欢迎各类贡献，包括但不限于报告问题、提交代码、完善文档或推荐新的外链资源。请遵循以下流程：

1. **查阅现有议题**：在 GitHub Issues 中搜索类似提案或问题，避免重复提交。若无相关议题，请新建一个 issue 清晰描述您希望解决的问题或建议新增的功能。
2. **Fork 仓库并创建特性分支**：从主仓库的 `main` 分支创建您的个人分支，分支命名建议采用 `feature/xxx` 或 `fix/xxx` 格式，并确保分支基于最新主干代码。
3. **编写或修改代码并补充测试**：所有新增功能或修复必须包含对应的单元测试或集成测试，确保测试覆盖核心逻辑。同时更新或新增文档说明，保持文档与代码同步。
4. **提交前运行完整测试套件**：执行 `npm test` 确保所有现有测试通过，执行 `npm run lint` 检查代码风格一致性。提交信息请遵循 Conventional Commits 规范。
5. **发起 Pull Request**：向主仓库的 `main` 分支发起 PR，并在描述中关联相关 issue 编号。PR 合并前至少需要一名项目维护者审核通过。

## 常见问题

**问：SQLite 数据库在高并发访问下是否会出现锁竞争？如何优化？**

答：SQLite 默认使用串行化事务隔离级别，对于读多写少的链接导航场景，并发读取不会产生锁冲突。写入操作（如新增链接或更新点击次数）在极端并发下可能遇到锁等待，但本项目设计为小规模团队使用（建议日活跃用户 < 50），SQLite 完全能够胜任。若需提升并发写入性能，可将数据库文件放置于内存文件系统（临时表）或迁移至 PostgreSQL，项目模型层已预留 ORM 适配接口。

**问：如何迁移或备份已收录的链接数据？**

答：备份最简单的方式是直接复制 `./data/links.db` 文件。若需跨版本迁移，推荐使用内置的导出功能生成 JSON 文件，再在新实例中通过导入功能恢复。导出的 JSON 包含所有链接字段及分类关联关系，不依赖于数据库特定实现。生产环境建议每日通过 cron 任务执行 `npm run backup` 脚本，自动压缩备份至指定目录。

**问：健康检查功能检测到失效链接后会怎样处理？**

答：健康检查服务默认每 24 小时运行一次，对每条链接发送 HEAD 请求以节省带宽。若返回状态码 400 及以上或请求超时，系统会在日志中标记该链接为 `unhealthy`，并在管理后台的“异常链接”视图中高亮显示。项目不会自动删除失效链接，而是等待管理员手动确认后更新或移除，以避免因临时网络波动导致的误判。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
