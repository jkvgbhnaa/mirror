# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, open-source information indexing system designed to organize, categorize, and present distributed web resources through a unified access layer. It targets developers, content curators, and small-scale research teams who need to maintain a manually curated collection of external reference links without building a full-fledged CMS or bookmarking platform from scratch. The project solves the problem of scattered, unmanaged external resource references by providing a structured metadata framework, a simple local development server, and a reproducible deployment workflow for static site generation.

## 功能概览

- **Metadata-driven link cataloging** – Each external URL is stored with optional tags, status codes, and last-verified timestamps to support basic link rot detection.
- **Local development server with live reload** – Built-in Node.js or Python based server that serves the index page and refreshes automatically when the catalog file changes.
- **Static site export mode** – Generates a self-contained HTML directory that can be deployed to any static hosting service without server-side dependencies.
- **Tag-based filtering and full-text search** – Client-side JavaScript provides instant filtering by tags and keyword search across title and description fields.
- **Bulk import from plain text lists** – Accepts newline-separated URLs and automatically fetches page titles via a configurable headless browser or fetch API.
- **Customizable response status monitoring** – Periodically checks each stored URL for HTTP status changes and highlights broken or redirected links in the dashboard.
- **Markdown-based configuration for resource groups** – Allows grouping links into sections (e.g., "News", "Reference", "Archive") using a simple Markdown frontmatter convention.

## 应用场景

- **Personal knowledge base augmentation** – Researchers and writers can maintain a companion index of source articles and reference materials alongside their notes, ensuring that every cited external link is tracked and periodically verified.
- **Team documentation hub** – Small development teams can use the aggregator to centralize API documentation, design specs, and third-party service dashboards, reducing the time spent searching through browser bookmarks or chat histories.
- **Newsletter or content curation pipeline** – Curators collecting industry news can batch-import daily link lists, add editorial notes, and export a static digest page for internal review or public sharing.
- **Legacy system migration assistant** – Organizations replacing outdated content portals can use this tool to inventory existing external dependencies and generate a clean, auditable link manifest before decommissioning old systems.
- **Educational resource repository** – Instructors can organize course-related reading materials, video links, and supplementary references into a browsable, filterable interface for students, with automatic availability checks before each semester.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/your-org/yidian-resource-aggregator.git
cd yidian-resource-aggregator

# Install dependencies (using npm, adjust if using Python/pip)
npm install

# Start the local development server
npm run dev

# For production static build
npm run build
```

The development server will start at `http://localhost:3000` by default. The catalog file is located at `data/catalog.json`. Edit this file to add or remove entries, and the page will reload automatically.

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Node.js | >= 18.0.0 | Runtime environment for the development server and build scripts |
| npm | >= 9.0.0 | Package manager for installing dependencies |
| Git | >= 2.30.0 | Required for cloning and version control operations |
| curl | >= 7.68.0 | Used by the verification script for HTTP status checks |
| SQLite3 | >= 3.35.0 | Optional but recommended for persistent metadata storage in production mode |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门 | `docs/getting-started.md` | How to set up the project, configure the catalog, and run the server for the first time |
| 配置 | `docs/configuration.md` | What configuration options exist, how to customize tags, groups, and verification intervals |
| 部署 | `docs/deployment.md` | How to export static files, deploy to Netlify/Vercel, or serve via Nginx |
| 扩展 | `docs/extending.md` | How to add custom verifiers, integrate with external APIs, or modify the UI template |

## 资源列表

- http://3g.yidianmeii.cn/snews/087179.shtml
- http://wap.yidianmeii.cn/snews/611361.shtml
- http://wap.yidianmeii.cn/snews/19885.shtml
- http://3g.yidianmeii.cn/snews/56991.shtml
- http://h5.yidianmeii.cn/snews/1596365.shtml
- http://wap.yidianmeii.cn/snews/656598.shtml
- http://wap.yidianmeii.cn/snews/01798.shtml
- http://3g.yidianmeii.cn/snews/9811081.shtml
- http://3g.yidianmeii.cn/snews/9856018.shtml
- http://wap.yidianmeii.cn/snews/6555893.shtml

## 项目结构

```
yidian-resource-aggregator/
├── src/                         # Source code directory
│   ├── server/                  # Development server implementation
│   │   ├── index.js             # Entry point for the HTTP server
│   │   └── watcher.js           # File change detection and live reload logic
│   ├── builder/                 # Static site generation modules
│   │   ├── export.js            # HTML and asset export pipeline
│   │   └── transformer.js       # Catalog data to HTML template transformation
│   ├── verifier/                # Link status checking subsystem
│   │   ├── checker.js           # HTTP status polling with configurable timeout
│   │   └── reporter.js          # Formats verification results for display
│   └── cli/                     # Command-line interface utilities
│       ├── commands.js          # Defines available CLI subcommands
│       └── parser.js            # Argument parsing and help text generation
├── public/                      # Static assets served directly
│   ├── css/
│   │   └── style.css            # Base styles and responsive layout definitions
│   ├── js/
│   │   ├── filter.js            # Client-side tag filtering logic
│   │   └── search.js            # Full-text search implementation
│   └── index.html               # Main application shell (overwritten during build)
├── data/                        # User-modifiable data directory
│   ├── catalog.json             # Primary link catalog in JSON format
│   └── groups.yaml              # Defines group names, icons, and display order
├── docs/                        # Project documentation (see Document Navigation)
│   ├── getting-started.md
│   ├── configuration.md
│   ├── deployment.md
│   └── extending.md
├── tests/                       # Unit and integration test suite
│   ├── unit/
│   │   ├── verifier.test.js     # Tests for HTTP status checking functions
│   │   └── transformer.test.js  # Tests for data transformation utilities
│   └── integration/
│       └── export.test.js       # End-to-end static export validation
├── scripts/                     # Utility scripts for maintenance
│   ├── import-from-text.js      # Bulk import helper for plain text URL lists
│   └── verify-all.js            # Manual verification runner for all catalog entries
├── .gitignore                   # Git ignore rules for node_modules, logs, etc.
├── package.json                 # npm manifest with dependencies and scripts
├── README.md                    # This file
└── LICENSE                      # MIT license text
```

## 贡献指南

1. **Fork the repository and create a feature branch** – Use `git checkout -b feature/your-descriptive-name` to isolate your changes. Ensure your branch is based on the latest `main` commit.

2. **Run the development environment and validate your changes** – Execute `npm run dev` to start the server. Verify that your modifications do not break the existing UI or build pipeline. Add or update tests in the `tests/` directory if your change introduces new functionality.

3. **Update documentation accordingly** – If you add a new configuration option or change the behavior of an existing feature, reflect the change in the relevant `docs/` file. For user-facing changes, also update the "功能概览" section of this README.

4. **Submit a pull request with a clear description** – Include the motivation for the change, a summary of the implementation, and any manual testing steps performed. Reference any related issues using `#issue-number`.

5. **Wait for code review and address feedback** – Maintainers will review your pull request within five business days. Address all comments and keep the branch rebased to avoid merge conflicts.

## 常见问题

**Q: How do I add a new URL to the catalog without restarting the server?**

A: The development server watches `data/catalog.json` for changes. Simply edit the file in your preferred text editor and save it. The server will detect the change and trigger a live reload of the browser page. For production builds, run `npm run build` after modifying the catalog to regenerate the static files.

**Q: The verifier marks many URLs as broken, but they work in my browser. Why?**

A: The verifier uses a headless HTTP client with a strict timeout (default 5 seconds) and does not follow complex JavaScript redirects. Some sites may block headless requests or require cookie consent. You can adjust the timeout and user-agent string in `src/verifier/checker.js`. For manual verification, use the `scripts/verify-all.js` utility with the `--timeout` flag to increase the waiting period.

**Q: Can I use this project with Python instead of Node.js?**

A: The current implementation is Node.js-based, but the architecture is deliberately simple. The catalog is plain JSON, and the static export generates standard HTML/CSS/JS. A Python port would only need to replicate the server, watcher, and export functions. We welcome community contributions for a Python alternative under a separate branch. For now, Node.js 18+ is the officially supported runtime.

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
