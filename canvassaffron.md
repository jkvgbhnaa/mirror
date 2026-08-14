# Yidian Meta Hub

Yidian Meta Hub 是一个面向中文互联网内容聚合与结构化存储的开源外链管理工具，定位于将分散在移动端 H5、WAP 等不同入口的新闻资讯、专题页面、数据快照进行统一收录、索引与归档。项目目标用户为内容运营人员、爬虫开发者、舆情分析工程师以及需要批量管理短效外链的技术团队。通过标准化 URL 输入与可扩展的元数据提取逻辑，Yidian Meta Hub 解决多源异构链接难以归类、生存周期不可控、重复采集成本高等痛点，提供一套轻量级、可自部署的链接仓库方案。

## 功能概览

批量外链导入与去重校验 支持一次性录入大批量 URL，自动识别重复条目并生成冲突报告。

多终端入口适配标记 自动识别 h5、wap、3g 等不同移动端前缀，并保留原始子域名信息用于下游路由分发。

链接元数据快照生成 对每个收录链接生成标题推测、时间戳提取、来源域名归类等基础元数据字段。

黑名单与失效链接过滤 内置可配置的 HTTP 状态码检测机制，自动标记 4xx/5xx 类不可用链接。

JSON 与 CSV 结构化导出 支持将链接库导出为标准数据交换格式，便于接入 Elasticsearch、数据湖或自定义看板。

定时巡检与更新通知 集成 cron 式检查任务，对已收录链接进行定期可用性重检，并输出变更日志。

标签化分类管理 允许用户为每条链接打上自定义标签，如“时事”、“科技”、“快讯”等，构建多维度检索视图。

权限分级与只读快照 提供基础的多用户只读/可写权限分割，适合团队协作场景下的链接资产保护。

## 应用场景

舆情监测团队每日采集移动端新闻快照 团队每日从多个移动入口抓取热点新闻链接，利用 Yidian Meta Hub 统一入库并自动去重，避免重复爬取，同时通过标签快速筛选特定类别内容用于后续情感分析。

内容聚合站点批量导入历史归档链接 内容站点运营者需要将历史发布的数千条外链集中归档，通过批量导入接口一次性提交原始 URL 列表，系统自动补全来源标记与抓取时间，生成可检索的归档目录。

个人开发者自建“稍后阅读”链接仓库 开发者将日常浏览中分散在不同终端的有价值文章链接统一保存至本地实例，利用定时巡检功能监控链接是否失效，避免收藏夹大面积损坏。

数据分析工程师准备外链特征训练集 工程师导出 CSV 格式的链接列表及其元数据，结合外部 API 扩充页面标题、正文摘要等字段，构建用于链接质量评估或分类模型的训练数据集。

企业内部知识库外链合规审计 企业知识库管理员定期导入所有外部引用链接，通过黑名单过滤机制自动屏蔽违规或失效域名，确保内部文档引用的外链符合安全合规要求。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/yidian-dev/yidian-meta-hub.git

# 进入项目目录
cd yidian-meta-hub

# 安装依赖（基于 Python 3.10+）
pip install -r requirements.txt

# 初始化本地 SQLite 数据库
python scripts/init_db.py

# 启动开发服务（默认端口 8080）
python app.py run --host 0.0.0.0 --port 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 或更高 | 核心运行环境，建议使用 pyenv 管理 |
| SQLite | 3.35 或更高 | 默认内置数据库，用于元数据存储 |
| requests | 2.28.0 或更高 | 用于 HTTP 状态检测与元数据抓取 |
| PyYAML | 6.0 或更高 | 配置文件解析，支持 YAML 格式的自定义规则 |
| pytest | 7.2.0 或更高 | 单元测试框架，仅开发模式需要 |
| schedule | 1.2.0 或更高 | 定时巡检任务调度库 |
| flask | 2.2.0 或更高 | Web API 服务框架，可选依赖 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 入门指南 | docs/quickstart.md | 如何五分钟内完成部署并导入第一批链接？ |
| 配置手册 | docs/configuration.md | 如何调整黑名单规则、巡检频率和导出格式？ |
| API 参考 | docs/api_reference.md | 批量导入、标签管理、状态查询等接口如何调用？ |
| 运维实践 | docs/operation_guide.md | 如何备份数据库、迁移实例以及处理大规模链接？ |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/15686.shtml
- http://h5.yidianmeii.cn/jnews/0814/830398.shtml
- http://wap.yidianmeii.cn/jnews/0814/31161.shtml
- http://wap.yidianmeii.cn/jnews/0814/0196.shtml
- http://h5.yidianmeii.cn/jnews/0814/55136.shtml
- http://h5.yidianmeii.cn/jnews/0814/8067.shtml
- http://wap.yidianmeii.cn/jnews/0814/7813619.shtml
- http://wap.yidianmeii.cn/jnews/0814/63139.shtml
- http://h5.yidianmeii.cn/jnews/0814/3399.shtml
- http://h5.yidianmeii.cn/jnews/0814/00360.shtml

## 项目结构

```
yidian-meta-hub/
├── app.py                  # 主入口文件，初始化 Flask 应用与路由
├── requirements.txt        # 生产环境依赖清单
├── config/
│   ├── default.yaml        # 默认配置（端口、巡检间隔、黑名单域名）
│   └── custom.yaml.example # 用户自定义配置模板
├── core/
│   ├── importer.py         # 批量导入引擎，含去重与格式校验
│   ├── checker.py          # 链接可用性检测模块，支持异步并发
│   ├── metadata.py         # 元数据提取（标题、时间、来源域名）
│   └── exporter.py         # 导出为 JSON / CSV 格式的转换器
├── storage/
│   ├── db.py               # SQLite 数据库连接与 CRUD 操作
│   ├── models.py           # 数据表映射对象（Link, Tag, CheckLog）
│   └── migrations/         # 数据库版本升级脚本
├── scheduler/
│   ├── cron.py             # 定时任务调度器，封装 schedule 库
│   └── tasks.py            # 具体巡检任务定义（检查、通知、清理）
├── api/
│   ├── v1/                 # RESTful API 版本 v1 端点
│   │   ├── links.py        # 链接增删改查接口
│   │   ├── tags.py         # 标签管理接口
│   │   └── health.py       # 健康检查与状态接口
│   └── middleware.py       # 权限校验与请求日志中间件
├── tests/
│   ├── unit/               # 单元测试用例
│   └── integration/        # 集成测试（真实数据库与网络请求）
└── scripts/
    ├── init_db.py          # 首次运行建表与默认数据插入
    └── seed_example.py     # 生成示例链接数据用于测试
```

## 贡献指南

提交 Issue 描述具体需求或缺陷 在 GitHub Issues 页面新建问题，清晰说明使用场景、预期行为和当前现象，并附上运行环境与日志片段。

Fork 主仓库并创建功能分支 从主分支 checkout 出新分支，命名遵循 `feature/` 或 `fix/` 前缀，例如 `feature/add-tag-export`。

编写或更新单元测试覆盖变更 所有新增函数或修改逻辑必须附带对应的 pytest 用例，确保测试通过率保持 100%。

发起 Pull Request 并关联相关 Issue PR 描述中注明解决的问题编号，简要概述实现方案与测试结果，等待维护者审阅。

遵循代码风格与提交规范 使用 Black 格式化 Python 代码，提交信息采用 `type(scope): subject` 格式，如 `feat(importer): add csv batch import`。

## 常见问题

Q: 导入大量链接时出现内存溢出如何处理？
A: 建议将批量导入切分为每批 500 条以内的多个子任务，并启用 `--stream` 参数启用流式处理模式。同时可调整配置中的 `batch_size` 和 `worker_pool` 参数控制并发度。

Q: 定时巡检任务未按预期执行，如何排查？
A: 请检查 `scheduler/cron.py` 中的时区配置是否与系统时区一致，并确认进程是否以守护模式常驻运行。可查看 `logs/checker.log` 中的详细错误堆栈，常见原因包括网络代理设置或 DNS 解析超时。

Q: 如何将现有 SQLite 数据迁移至 MySQL 或 PostgreSQL？
A: 项目未内置直接迁移工具，但可使用 `exporter.py` 导出为 JSON 或 CSV 格式，再通过目标数据库的导入工具完成迁移。建议在迁移前关闭所有写入操作，以保证数据一致性。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
