# Yidian News Aggregator

Yidian News Aggregator is a high-performance, open-source information aggregation and navigation system designed to organize, index, and present news content from distributed sources. This project targets developers, data analysts, and content curators who need a structured way to manage and access news resources across multiple subdomains and endpoints. By providing a unified interface and standardized data processing pipeline, Yidian News Aggregator solves the problem of fragmented content discovery and manual URL management, enabling users to efficiently harvest, filter, and redistribute news metadata for research, archiving, or secondary distribution purposes.

The system is built with modularity and extensibility in mind, supporting custom scrapers, pluggable storage backends, and RESTful APIs for external integration. It is not a content management system but a lightweight orchestration layer that transforms raw HTML sources into clean, queryable datasets. Whether you are building a news monitoring dashboard, conducting media trend analysis, or simply organizing daily reading lists, Yidian News Aggregator provides the foundational tooling to reduce operational overhead and improve data accessibility.

## 功能概览

- **Multi-source URL Harvesting** – Automatically discover and normalize news URLs from configurable entry points, deduplicate entries, and maintain an active resource manifest.

- **Content Metadata Extraction** – Parse HTML pages to extract title, publication timestamp, author information, and summary text using heuristic algorithms and optional XPath rules.

- **Indexing and Full-text Search** – Build inverted indices over extracted content and provide fast keyword-based search with relevance scoring and pagination.

- **Categorization and Tagging** – Apply rule-based or machine-learning-assisted category labels (e.g., technology, finance, health) and user-defined tags for fine-grained organization.

- **Export and Syndication** – Export aggregated results as JSON, CSV, or RSS feeds, enabling seamless integration with downstream analytics tools or news readers.

- **Scheduled Crawling** – Built-in cron-like scheduler to run harvesting jobs at specified intervals, with email or webhook notifications upon completion or failure.

- **Access Control and API Keys** – Secure API endpoints with role-based access control and rate limiting, suitable for multi-user environments or public-facing services.

## 应用场景

- **Personal News Dashboard** – Individuals can deploy the aggregator to collect articles from preferred sources into a single, searchable interface, replacing manual bookmarking and multiple browser tabs.

- **Media Monitoring for Enterprises** – PR and marketing teams can configure the system to track coverage of brand keywords, competitors, or industry trends, exporting reports for weekly executive briefings.

- **Academic Research Data Collection** – Researchers in computational social science can use the aggregator to build longitudinal news corpora, with structured metadata suitable for content analysis, sentiment scoring, or network mapping.

- **Content Curation for Newsletters** – Editors and curators can leverage the tagging and categorization features to filter high-signal stories, generate summaries, and produce daily or weekly email digests with minimal manual effort.

- **DevOps Monitoring and Alerting** – Operations teams can integrate the aggregator with monitoring stacks (e.g., Prometheus, Grafana) to trigger alerts when specific keywords appear in news feeds, enabling rapid incident response for reputation management.

## 快速开始

The following commands will clone the repository, install dependencies, and start the development server on localhost. Ensure you have Git, Python 3.9+, and virtualenv installed before proceeding.

```bash
git clone https://github.com/yidian-news/yidian-aggregator.git
cd yidian-aggregator
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
python manage.py migrate
python manage.py runserver
```

After the server starts, visit http://127.0.0.1:8000/api/v1/status to verify the installation. The default admin credentials are printed in the console during the first startup. Change them immediately in production environments.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 – 3.12 | Core runtime; type hints and async features are utilized. |
| PostgreSQL | 13.0+ | Primary relational database for metadata, user accounts, and job queues. |
| Redis | 6.0+ | Used for caching, rate limiting, and temporary storage of crawled HTML. |
| Node.js | 18.0+ | Required for frontend asset compilation and development server hot-reload. |
| Elasticsearch | 7.17 – 8.x | Optional but recommended for full-text search and advanced analytics. |
| RabbitMQ | 3.10+ | Message broker for distributed crawling tasks when using worker mode. |
| Nginx | 1.20+ | Recommended reverse proxy for production deployments with static file serving. |
| Docker | 20.10+ | Required only if using the provided docker-compose for containerized setup. |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|-----|------|-----------|
| 用户手册 | /docs/user-guide/ | How to configure sources, run crawls, view results, and export data via the web interface. |
| API 参考 | /docs/api-reference/ | Detailed endpoint specifications, request/response schemas, authentication methods, and rate limit headers. |
| 部署指南 | /docs/deployment/ | Production deployment strategies using systemd, Docker Swarm, or Kubernetes; includes SSL configuration and backup procedures. |
| 开发手册 | /docs/development/ | How to extend the system with custom parsers, add new storage backends, or modify the scheduling engine. |
| 运维 FAQ | /docs/operations/ | Troubleshooting common issues such as memory leaks, connection timeouts, database locks, and Redis replication lag. |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/3731.shtml
- http://wap.yidianmeii.cn/jnews/0814/1967.shtml
- http://h5.yidianmeii.cn/jnews/0814/6313.shtml
- http://3g.yidianmeii.cn/jnews/0814/96737.shtml
- http://wap.yidianmeii.cn/jnews/0814/65685.shtml
- http://3g.yidianmeii.cn/jnews/0814/1319316.shtml
- http://3g.yidianmeii.cn/jnews/0814/5131.shtml
- http://h5.yidianmeii.cn/jnews/0814/3566.shtml
- http://h5.yidianmeii.cn/jnews/0814/1876861.shtml
- http://wap.yidianmeii.cn/jnews/0814/06906.shtml

## 项目结构

```
yidian-aggregator/
├── api/                         # RESTful API endpoints and serializers
│   ├── v1/                      # Version 1 of the API
│   │   ├── endpoints/           # Resource-specific route handlers
│   │   └── middleware/          # Authentication, logging, and throttling
├── core/                        # Core business logic and domain models
│   ├── crawler/                 # Crawling engine with pluggable fetchers
│   │   ├── base.py              # Abstract fetcher and parser interfaces
│   │   └── parsers/             # Site-specific HTML parsers
│   ├── indexer/                 # Indexing pipeline and search abstraction
│   └── scheduler/               # Job scheduling and task orchestration
├── data/                        # Data access layer and migrations
│   ├── migrations/              # Database schema migration scripts
│   └── repositories/            # Query builders and DAO implementations
├── frontend/                    # React-based admin dashboard and assets
│   ├── src/                     # Source code for UI components
│   └── static/                  # Compiled CSS, JS, and images
├── scripts/                     # Utility scripts for maintenance and testing
│   ├── seed_db.py               # Populate database with sample sources
│   └── benchmark.py             # Performance testing suite
├── tests/                       # Unit and integration tests
│   ├── unit/                    # Isolated component tests
│   └── integration/             # End-to-end test scenarios
├── .env.example                  # Environment variable template
├── docker-compose.yml            # Container orchestration for development
├── Dockerfile                    # Production container build definition
├── manage.py                     # Django management script
├── pyproject.toml                # Project metadata and build configuration
└── README.md                     # This documentation
```

## 贡献指南

We welcome contributions from the community. Please follow the steps below to propose changes or report issues.

1.  **Fork the Repository** – Create a personal fork of the main repository on GitHub and clone it to your local development environment. Ensure your fork is synchronized with the upstream main branch.

2.  **Create a Feature Branch** – Use a descriptive branch name prefixed with `feature/`, `fix/`, or `docs/` (e.g., `feature/add-rss-exporter`). Branch from the latest `develop` branch, not `main`.

3.  **Write Tests and Documentation** – For any new functionality, add corresponding unit tests under `tests/unit/` and update the API documentation or user guide as appropriate. Ensure all existing tests pass by running `pytest` locally.

4.  **Submit a Pull Request** – Push your branch to your fork and open a pull request against the `develop` branch of the upstream repository. Provide a clear description of the changes, reference related issues if any, and check the PR checklist for code style and commit message conventions.

5.  **Code Review and Merge** – At least two maintainers will review your submission. Address any feedback by pushing additional commits to the same branch. Once approved, a maintainer will squash and merge your PR into `develop`. Regular release cycles will merge `develop` into `main` for stable releases.

## 常见问题

**Q: How do I add a new news source that requires JavaScript rendering?**

A: The default fetcher uses `requests` and `BeautifulSoup` for static HTML. For JavaScript-heavy sites, enable the Selenium-based fetcher by setting `FETCHER_ENGINE=selenium` in your `.env` file and ensure a compatible WebDriver is installed. Alternatively, you can implement a custom fetcher subclass and register it in the parser registry.

**Q: The search results are slow when indexing more than 100,000 articles. What tuning options are available?**

A: Slow search is typically due to missing database indexes or insufficient Elasticsearch heap size. Verify that you have created the recommended GIN indexes on `content` and `title` fields for PostgreSQL. For Elasticsearch, increase the `ES_JAVA_OPTS` memory setting and consider using index lifecycle management to move older articles to colder tiers. You can also enable query caching via Redis by setting `CACHE_TTL=300` in your configuration.

**Q: Can I run the crawler in a distributed manner across multiple machines?**

A: Yes. Set `WORKER_MODE=distributed` and configure a shared Redis backend for task queuing and a common PostgreSQL database. Each worker node must have access to the same codebase and secrets. Use the provided Kubernetes manifests or Docker Swarm stack file to orchestrate worker replicas. Be mindful of the target news sites' robots.txt and rate-limit policies to avoid being blocked.

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
