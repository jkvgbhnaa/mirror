# Yidianmeii News Bridge

Yidianmeii News Bridge 是一个轻量级的技术资讯聚合与导航系统，定位于对中文互联网碎片化信息进行结构化整理与分发。项目不对源内容做二次编辑，而是通过稳定链接映射机制，为开发者、数据分析师与信息研究员提供可预测、可追溯的外部资源引用入口。

本项目适用于需要批量引用特定新闻快照、构建外部链接监控系统、或者对站点内容进行长期归档扫描的自动化场景。通过统一的链接前缀与资源编号规则，所有外链均保持原站原始协议与域名形态，确保引用链路零篡改。

## 功能概览

- 统一资源索引表：维护全部外链的原始地址与内部编号映射，支持快速查找。
- 多子域名适配：兼容 3g、wap、h5 三种移动端前缀，自动匹配请求来源。
- 静态导航页生成：根据资源列表构建只读型 HTML 目录，适合内嵌至监控面板。
- 链接可达性检测：周期性 HEAD 请求验证资源存活状态，输出 JSON 格式报告。
- 原始协议保留策略：所有输出链接强制保持输入时的 http/https 与域名大小写。
- 批量导入导出：支持通过文本文件批量追加资源，自动去重并生成新编号。
- 轻量级日志记录：记录每次资源访问的时间戳、来源 IP 与 User-Agent，用于流量分析。

## 应用场景

- 数据采集管道中的 URL 白名单维护：在爬虫系统中，将本项目作为外链来源的集中管理仓库，通过内部编号引用外部新闻链接，避免散落在代码各处的裸地址导致维护困难。
- 内容审核前后对比引用：审核人员需要快速打开原始新闻链接进行内容核验，本项目提供清晰的列表视图，减少人工复制粘贴错误。
- 自动化监控告警配套：运维团队可定时拉取本项目的资源列表，对每个链接做状态码检查，当大量链接不可达时触发告警，及时发现源站异常。
- 学术研究中的引用存档：研究互联网信息传播的学者可将本项目生成的资源快照编号作为论文附录中的引用索引，保证读者可复现原始材料。

## 快速开始

以下命令演示了从克隆仓库到启动本地导航服务的完整流程。

```bash
# 克隆项目仓库
git clone https://github.com/your-org/yidianmeii-bridge.git
cd yidianmeii-bridge

# 安装依赖（使用 pip 管理 Python 后端）
pip install -r requirements.txt

# 初始化资源数据库（导入内置资源列表）
python manage.py initdb
python manage.py import-urls --file resources/default.txt

# 启动开发服务器（默认监听 127.0.0.1:8080）
python server.py
```

启动后，访问 http://127.0.0.1:8080/nav 可查看所有资源的导航页面，访问 http://127.0.0.1:8080/api/urls 可获取 JSON 格式的完整资源清单。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心运行环境，用于后端服务与脚本工具 |
| pip | 20.0 及以上 | Python 包管理工具，用于安装依赖库 |
| Flask | 2.0.0 及以上 | Web 框架，提供导航页与 API 接口 |
| requests | 2.25.0 及以上 | 用于发送 HEAD 请求检测链接可达性 |
| pytest | 6.0.0 及以上 | 单元测试框架，仅在开发环境中使用 |
| git | 2.25.0 及以上 | 版本控制工具，用于克隆与提交变更 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/usage.md | 如何添加新资源、如何生成导航页、如何导出列表 |
| 运维指南 | /docs/operations.md | 如何配置定时检测、如何调整日志级别、如何备份数据库 |
| 开发参考 | /docs/development.md | 项目目录结构说明、核心类设计、API 端点定义 |
| 设计决策 | /docs/design.md | 为何保留原始协议、为何不使用重定向、为何不缓存内容 |

## 资源列表

- http://3g.yidianmeii.cn/snews/9775.shtml
- http://3g.yidianmeii.cn/snews/666165.shtml
- http://wap.yidianmeii.cn/snews/985750.shtml
- http://wap.yidianmeii.cn/snews/616516.shtml
- http://h5.yidianmeii.cn/snews/175638.shtml
- http://wap.yidianmeii.cn/snews/6983.shtml
- http://3g.yidianmeii.cn/snews/0711.shtml
- http://h5.yidianmeii.cn/snews/7811155.shtml
- http://3g.yidianmeii.cn/snews/7761.shtml
- http://wap.yidianmeii.cn/snews/1166006.shtml

## 项目结构

```
yidianmeii-bridge/
├── server.py                 # Flask 应用入口，注册路由与启动服务
├── requirements.txt          # Python 依赖列表，固定版本号
├── manage.py                 # 命令行管理工具，包含 initdb/import/check 子命令
├── config/
│   ├── settings.py           # 全局配置项：端口、日志路径、检测超时
│   └── urls.txt              # 初始资源列表模板，每行一个 URL
├── core/
│   ├── __init__.py
│   ├── repository.py         # 资源存取类，基于 SQLite 实现增删改查
│   ├── inspector.py          # 链接检测类，封装 requests 的 HEAD 方法
│   └── formatter.py          # 导航页与 JSON 输出格式化工具
├── templates/
│   └── nav.html              # Jinja2 模板，渲染资源列表页面
├── static/
│   ├── style.css             # 导航页基础样式，移动端适配
│   └── script.js             # 前端点击复制与过滤功能
├── tests/
│   ├── test_repository.py    # 数据库操作单元测试
│   └── test_inspector.py     # 检测逻辑模拟测试
├── logs/                     # 运行时日志目录，按天滚动
│   └── access.log            # 访问日志示例
└── README.md                 # 本文档
```

## 贡献指南

1. 复刻本仓库至个人账户，并在本地创建功能分支，分支命名格式为 `feature/简述修改内容` 或 `fix/问题编号`。
2. 对资源列表进行增删改时，请同步更新 `config/urls.txt` 文件，并运行 `python manage.py dedup` 命令去除重复条目。
3. 所有代码修改需附带对应的单元测试，测试文件存放于 `tests/` 目录，确保 `pytest` 执行全部通过。
4. 提交前执行 `python manage.py lint` 检查代码风格是否符合 PEP 8 规范，并修复所有警告。
5. 发起 Pull Request 至主仓库的 `main` 分支，在请求描述中说明变更目的、影响范围以及测试结果摘要。

## 常见问题

**问：为什么资源列表中的链接不能点击跳转？**

答：本项目定位为引用索引而非代理网关，导航页默认以纯文本形式展示完整 URL，目的是避免嵌入 iframe 或重定向导致原始来源被屏蔽。用户如需访问，可自行复制链接到浏览器地址栏打开，或使用浏览器插件进行批量打开。

**问：如何批量添加新的外部链接？**

答：编辑 `config/urls.txt` 文件，每行写入一个完整的 URL（必须包含协议），然后执行 `python manage.py import-urls --file config/urls.txt --append`。该命令会自动去重，并为新链接生成内部编号。导入完成后，导航页将在下一次刷新时展示新增条目。

**问：项目会缓存外部页面的内容吗？**

答：不会。本系统只存储和展示链接地址本身，不发起 GET 请求获取页面正文，也不保存任何副本。仅通过 HEAD 请求检测链接的 HTTP 状态码，用于监控可达性。所有内容版权归属于原始站点，本项目不涉及内容分发。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
