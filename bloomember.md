# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, high-performance technical resource navigation and content aggregation system designed for developers, researchers, and technical content curators who need to organize, categorize, and present distributed web resources in a structured manner. The project addresses the common pain point of managing scattered external links, providing a unified access layer with automated metadata extraction, link health monitoring, and category-based filtering capabilities.

Targeting system administrators, DevOps engineers, and technical documentation maintainers, this aggregator serves as a turnkey solution for building internal resource portals or public reference archives. It operates as a static site generator with dynamic health-checking middleware, enabling teams to maintain reliable external resource collections without manual verification overhead.

## 功能概览

**Automated Link Health Monitoring** - Periodically validates each aggregated URL for HTTP status codes and response times, flagging dead or slow resources for administrator review.

**Category-Based Resource Classification** - Assigns semantic tags and hierarchical categories to each external link, enabling faceted navigation and filtered search experiences.

**Metadata Extraction Pipeline** - Scrapes configurable metadata including page titles, description summaries, and last-modified timestamps from target URLs at ingestion time.

**Static Site Generation with Live Refresh** - Produces fully static HTML output for high-performance serving, with incremental regeneration triggered by upstream content changes.

**Customizable Rendering Templates** - Provides Jinja2-based template system allowing complete control over presentation layers without modifying core aggregation logic.

**RESTful Management API** - Exposes authenticated endpoints for programmatic resource addition, removal, and batch update operations.

**Scheduled Crawl Orchestration** - Integrates with cron-based schedulers to execute link verification and metadata refresh tasks during off-peak hours.

**Export and Backup Utilities** - Supports JSON, YAML, and CSV exports of the entire resource catalog for backup or migration purposes.

## 应用场景

**Internal Technical Documentation Portals** - Engineering teams maintaining internal wikis or runbooks can embed this aggregator to centralize references to external API docs, dependency repositories, and operational dashboards, ensuring all team members access the same validated resource set.

**Open Source Project Resource Pages** - Open source maintainers can deploy this aggregator as a companion site for their project, listing official mirrors, community forums, and related tools while automatically notifying when any referenced resource becomes unavailable.

**Academic Research Reference Management** - Research groups compiling literature sources, dataset repositories, and computational tool links can leverage the aggregation system to maintain a living bibliography with automated availability checks, reducing broken reference issues in published papers.

**DevOps Monitoring Dashboards** - Operations teams can embed the aggregator within monitoring stacks to track the availability of external dependency endpoints such as container registries, package repositories, and cloud service status pages, receiving alerts when critical resources fail health checks.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/yidian-dev/resource-aggregator.git
cd resource-aggregator

# Install Python dependencies
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your database connection and scheduler settings

# Initialize SQLite database and run migrations
python manage.py migrate

# Start the development server
python manage.py runserver --port=8080

# In production, use gunicorn with systemd or supervisor
# gunicorn --workers 4 --bind 0.0.0.0:8080 aggregator.wsgi:application
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9+ | 核心运行时，所有后端逻辑和CLI工具均基于Python开发 |
| SQLite | 3.35+ | 默认嵌入式数据库，用于存储资源元数据、分类映射和健康检查历史 |
| Redis | 6.0+ | 可选依赖，用于分布式缓存和任务队列（开发环境可禁用） |
| Node.js | 16.x+ | 前端资源构建工具链，用于编译CSS和JavaScript资产 |
| git | 2.25+ | 版本控制工具，用于克隆仓库和管理补丁更新 |
| cronie | 系统级 | 计划任务守护进程，用于调度定期的链接健康检查（Linux环境） |
| nginx | 1.20+ | 生产环境推荐反向代理，处理静态文件服务和负载均衡 |
| virtualenv | 20.0+ | Python虚拟环境管理器，隔离项目依赖 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | /docs/user-guide/ | 如何使用Web界面浏览、搜索和筛选资源；如何查看健康状态报告；如何配置个人偏好 |
| 管理员手册 | /docs/admin-guide/ | 如何添加或移除资源链接；如何管理分类体系；如何配置自动扫描策略和告警阈值 |
| 开发者文档 | /docs/developer-guide/ | 如何扩展元数据提取器；如何编写自定义渲染插件；如何贡献核心算法改进 |
| API参考 | /docs/api-reference/ | RESTful API的端点定义、请求格式、响应结构和认证机制详情 |
| 部署指南 | /docs/deployment-guide/ | 如何在不同操作系统上完成生产级部署；如何配置HTTPS；如何设置备份策略 |
| 故障排除 | /docs/troubleshooting/ | 常见错误代码含义；日志分析方法；性能调优建议和恢复操作流程 |

## 资源列表

- http://3g.yidianmeii.cn/snews/1076616.shtml
- http://3g.yidianmeii.cn/snews/8361.shtml
- http://3g.yidianmeii.cn/snews/361763.shtml
- http://3g.yidianmeii.cn/snews/00308.shtml
- http://wap.yidianmeii.cn/snews/8510660.shtml
- http://wap.yidianmeii.cn/snews/88035.shtml
- http://h5.yidianmeii.cn/snews/6881.shtml
- http://h5.yidianmeii.cn/snews/6677681.shtml
- http://3g.yidianmeii.cn/snews/1366.shtml
- http://3g.yidianmeii.cn/snews/87810.shtml

## 项目结构

```
resource-aggregator/
├── aggregator/
│   ├── __init__.py                     # 包初始化，版本声明
│   ├── settings.py                     # 全局配置（数据库、缓存、调度参数）
│   ├── urls.py                         # URL路由定义，映射API端点和视图
│   ├── wsgi.py                         # WSGI入口，用于生产部署
│   └── asgi.py                         # ASGI入口，支持异步协议（WebSocket）
├── apps/
│   ├── core/                           # 核心数据模型和通用工具函数
│   │   ├── models.py                   # Resource, Category, HealthCheck 数据表定义
│   │   ├── managers.py                 # 自定义数据库管理器，封装复杂查询
│   │   └── validators.py               # URL格式验证、域名白名单逻辑
│   ├── crawler/                        # 爬虫与元数据提取模块
│   │   ├── fetcher.py                  # 异步HTTP客户端，支持重试和超时控制
│   │   ├── parser.py                   # HTML解析器，提取标题、描述和关键词
│   │   └── scheduler.py                # 任务调度器，控制扫描频率和并发数
│   ├── api/                            # RESTful API实现
│   │   ├── views.py                    # 类视图，处理资源CRUD和批量操作
│   │   ├── serializers.py              # DRF序列化器，数据验证与转换
│   │   └── permissions.py              # 基于角色的访问控制策略
│   ├── dashboard/                      # Web管理仪表盘
│   │   ├── views.py                    # 管理界面视图，渲染模板上下文
│   │   ├── forms.py                    # 表单类，用于资源添加和编辑
│   │   └── templates/                  # Jinja2模板文件目录
│   └── health/                         # 健康检查独立模块
│       ├── checkers.py                 # 各协议检查器（HTTP/HTTPS/TCP）
│       ├── reporters.py                # 报告生成器（JSON/HTML/Email格式）
│       └── alerts.py                   # 告警触发逻辑，对接Webhook和邮件
├── static/                             # 静态资产（CSS、JavaScript、图片）
│   ├── css/                            # 基础样式和响应式布局文件
│   ├── js/                             # 前端交互逻辑（搜索、筛选、排序）
│   └── images/                         # 图标和logo资源
├── templates/                          # 全局模板（基模板和错误页面）
│   ├── base.html                       # 基础骨架模板，包含导航栏和页脚
│   ├── index.html                      # 首页资源列表渲染模板
│   └── error.html                      # 统一错误页面处理
├── scripts/                            # 维护和部署脚本
│   ├── seed.py                         # 初始化数据库，植入默认分类和示例资源
│   ├── backup.py                       # 数据库备份和归档工具
│   └── migrate_links.py                # 批量导入外部链接列表的迁移脚本
├── tests/                              # 单元测试和集成测试套件
│   ├── test_models.py                  # 数据模型层测试用例
│   ├── test_api.py                     # API端点功能性测试
│   └── test_crawler.py                 # 爬虫和解析逻辑的模拟测试
├── docs/                               # 完整文档源文件（reStructuredText格式）
│   ├── user-guide/                     # 面向终端用户的详细操作说明
│   ├── admin-guide/                    # 面向管理员的配置和运维手册
│   └── developer-guide/                # 面向贡献者的架构设计和扩展指南
├── requirements.txt                    # Python生产依赖列表（固定版本）
├── requirements-dev.txt                # 开发额外依赖（测试、lint、文档构建）
├── Dockerfile                          # 容器构建定义，多阶段优化镜像大小
├── docker-compose.yml                  # 本地开发环境编排（含Redis和PostgreSQL可选）
├── manage.py                           # Django管理入口，执行迁移和Shell命令
├── .env.example                        # 环境变量配置模板，包含敏感参数占位
├── .gitignore                          # 版本控制忽略文件规则
└── README.md                           # 当前项目说明文档
```

## 贡献指南

1. Fork the repository and create a feature branch from the main branch with a descriptive name corresponding to the issue or feature being addressed. Ensure your local development environment matches the version constraints specified in the installation requirements table.

2. Implement your changes while maintaining test coverage for new functionality. Run the full test suite locally using `pytest --cov=aggregator` to verify that no existing behavior is broken. Adhere to the PEP8 style guide and use the provided pre-commit hooks for automatic formatting.

3. Update the relevant documentation sections within the /docs directory to reflect any user-facing changes, API modifications, or configuration additions. Include code examples where applicable and ensure that the documentation builds without warnings.

4. Submit a pull request targeting the main branch with a clear description of the problem solved, approach taken, and any manual testing performed. Reference any related issues by number and include screenshots for UI changes.

5. After initial review, address feedback promptly and engage in discussion regarding design decisions. Maintainers will merge the pull request once all checks pass and at least one maintainer approves the changes.

## 常见问题

**Q: How does the aggregator handle resources that require authentication or session cookies to access?**

A: The crawler module supports custom request headers and cookie injection via the settings configuration. For authenticated resources, you can define per-resource credential mappings in the database using a JSON field, and the fetcher will apply these before each health check. We recommend using service account credentials rather than personal accounts, and store sensitive tokens using the environment variable substitution mechanism rather than hardcoding.

**Q: What is the recommended deployment strategy for high-availability production environments?**

A: We recommend a three-tier deployment: a stateless application layer running behind a load balancer (e.g., nginx or HAProxy), a separate Redis instance for caching and task queue, and a primary-replica database setup for SQLite or PostgreSQL. Static assets should be served via CDN or offloaded to nginx directly. The scheduler should run on a single designated node to avoid duplicate health-check tasks. For containerized environments, the included Dockerfile supports Kubernetes deployments with liveness and readiness probes configured.

**Q: Can I extend the aggregator to support protocols other than HTTP/HTTPS, such as FTP or SSH?**

A: Yes, the health checker module is designed with a pluggable architecture. You can implement new checker classes by subclassing `BaseChecker` and overriding the `check()` method. Register your custom checker in the `settings.CHECKER_REGISTRY` dictionary. The dashboard will automatically detect and display protocol-appropriate status indicators. Pull requests for community-contributed checkers are welcome, provided they include corresponding test cases and documentation.

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
