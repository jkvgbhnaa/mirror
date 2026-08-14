# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, open-source information aggregation and navigation system designed to catalog, organize, and present high-value external technical resources, news articles, and reference materials from distributed sources. The project targets developers, technical researchers, content curators, and DevOps engineers who need a reproducible, version-controlled, and rapidly deployable gateway to manually curated external content without relying on proprietary bookmarking services or closed ecosystems.

Unlike conventional link-in-bio tools or social bookmarking platforms, Yidian Resource Aggregator treats external URLs as first-class data entities. It maintains a static snapshot of resource metadata, allows local filtering and tagging, and provides a clean HTML-rendered interface for browsing entries that are grouped by source domain, content type, and publication date. The project is not a crawler, not a search engine, and not a dynamic CMS. It is a structured reference index that operators can fork, customize, and rebuild on demand, making it ideal for internal team knowledge bases, workshop handouts, or personal reading lists.

## 功能概览

- **Static Site Generation** – Builds a full static HTML catalogue from a curated list of external resource URLs, with zero runtime database or server-side logic.
- **Source Domain Normalization** – Automatically extracts and groups resources by their root domain (e.g., yidianmeii.cn subdomains) to help operators understand content distribution at a glance.
- **Metadata Inference** – Parses URL paths to infer possible content categories, date patterns (e.g., /0814/), and file types, providing structured hints without external API calls.
- **Responsive Grid Layout** – Renders the resource list in a mobile-friendly card-based interface with quick-copy links and native browser navigation.
- **Markdown-Based Configuration** – All resource entries are declared in a single Markdown file, enabling transparent change tracking, pull request workflows, and scripted bulk updates.
- **Custom Tagging Overlay** – Supports optional YAML frontmatter per URL to add custom tags, priority flags, and retention notes without altering the original link.
- **Integrity Checker** – Includes a local validation script that tests each URL for HTTP reachability and redirect sanity, reporting broken links before deployment.

## 应用场景

- **Internal Developer Knowledge Base** – A platform team maintains a curated list of internal service dashboards, runbooks, and incident postmortems. Yidian Resource Aggregator compiles these scattered links into a single, searchable landing page that every engineer can bookmark.
- **Technical Workshop or Bootcamp Material** – Instructors preload a Yidian instance with all reference articles, video transcripts, and supplementary readings for a training session. Participants clone the repository and open the generated HTML locally, avoiding network-dependent bookmark sharing.
- **Personal Research Reading Queue** – A security researcher collects threat intelligence reports, CVE analyses, and vendor advisories from multiple sources. By adding new URLs daily to the configuration, the researcher builds a chronological timeline of references that can be archived and revisited offline.
- **Compliance Artifact Collection** – Compliance officers aggregate external regulatory updates, framework announcements, and audit guidelines. Yidian provides a versioned snapshot of these resources, proving which external references were available during a specific review period.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/your-org/yidian-resource-aggregator.git
cd yidian-resource-aggregator

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Build the static catalogue from the default resource list
python build.py --input resources.md --output dist/

# Serve the generated site locally
python -m http.server 8000 --directory dist/
```

The build script reads `resources.md`, parses every URL entry, generates an index page with grouped cards, and writes all assets into the `dist/` directory. You can customize the output template by modifying `templates/index.html`.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | >= 3.9 | Core runtime for the build script and validation tools |
| pip | >= 21.0 | Package installer for required Python libraries |
| requests | >= 2.28.0 | Used by the integrity checker for HTTP reachability tests |
| PyYAML | >= 6.0 | Optional YAML frontmatter parser for custom metadata |
| markdown | >= 3.4.0 | Renders the resource list Markdown into HTML fragments |
| git | >= 2.30 | Required only for cloning and version control operations |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| Operator Guide | docs/operator-guide.md | How to add, remove, or batch-update resources; how to rebuild and redeploy the site |
| Contributor Handbook | docs/contributor-handbook.md | Coding standards, pull request process, and testing requirements for build script changes |
| Template Customization | docs/template-customization.md | How to modify the HTML/CSS layout, change card styles, and add custom JavaScript |
| Integrity Checker | docs/integrity-checker.md | Usage of the `check.py` script, timeout tuning, and handling of redirect chains |
| Release Process | docs/release-process.md | Version tagging, changelog generation, and automated deployment via GitHub Actions |

## 资源列表

- http://wap.yidianmeii.cn/vnews/0814/454598.shtml
- http://wap.yidianmeii.cn/vnews/0814/02798.shtml
- http://3g.yidianmeii.cn/vnews/0814/9812082.shtml
- http://3g.yidianmeii.cn/vnews/0814/9856018.shtml
- http://wap.yidianmeii.cn/vnews/0814/6555893.shtml
- http://3g.yidianmeii.cn/vnews/0814/9775.shtml
- http://3g.yidianmeii.cn/vnews/0814/644265.shtml
- http://wap.yidianmeii.cn/vnews/0814/985750.shtml
- http://wap.yidianmeii.cn/vnews/0814/424526.shtml
- http://h5.yidianmeii.cn/vnews/0814/275438.shtml

## 项目结构

```
yidian-resource-aggregator/
├── build.py                     # Main build script; orchestrates parsing, rendering, and output
├── check.py                     # Integrity checker; tests each URL for reachability
├── resources.md                 # Primary resource list in Markdown format (edit this)
├── requirements.txt             # Python dependencies pinned with versions
├── config.yaml                  # Site-wide settings: title, description, pagination, sort order
├── templates/                   # Jinja2-style HTML templates
│   ├── base.html                # Base layout with navigation and footer
│   ├── index.html               # Resource card grid template
│   └── detail.html              # Optional detail view for each resource entry
├── static/                      # Static assets copied directly to dist/
│   ├── css/
│   │   └── style.css            # Custom stylesheet (dark/light theme support)
│   └── js/
│       └── clipboard.js         # One-click copy utility for URLs
├── utils/                       # Internal helper modules
│   ├── parser.py                # Parses resources.md into structured dict objects
│   ├── grouper.py               # Groups resources by domain, date, and inferred category
│   └── validator.py             # Validates URL format and schema compliance
├── tests/                       # Unit tests for build logic and parser edge cases
│   ├── test_parser.py
│   ├── test_grouper.py
│   └── fixtures/
│       └── sample_resources.md  # Test data for continuous integration
├── docs/                        # Extended documentation (see Documentation Navigation)
│   ├── operator-guide.md
│   ├── contributor-handbook.md
│   ├── template-customization.md
│   ├── integrity-checker.md
│   └── release-process.md
└── .github/
    └── workflows/
        └── deploy.yml           # GitHub Actions workflow for automated builds and pages deployment
```

## 贡献指南

1. **Fork the repository and create a feature branch** – Fork the main repository to your personal GitHub account, then create a local branch with a descriptive name such as `feature/add-resource-tagging` or `fix/parser-encoding-issue`.

2. **Update resources.md or modify the build script** – For resource additions, append the new URL to `resources.md` following the established format (one URL per line, with optional YAML frontmatter). For code changes, include relevant unit tests under `tests/` and ensure all existing tests pass locally.

3. **Run the integrity checker before committing** – Execute `python check.py --input resources.md --timeout 5` to validate that all listed URLs are reachable. If any URL is permanently redirected, update the entry accordingly. Report unreachable URLs in the pull request description.

4. **Submit a pull request with a clear description** – Open a pull request against the `main` branch. Include the motivation for the change, a summary of modifications, and the output of the integrity checker. Pull requests that add more than 20 new URLs should include a brief rationale for bulk additions.

5. **Participate in the review process** – Maintainers may request adjustments to formatting, tag consistency, or code style. Address all review comments within 5 business days. Once approved, the pull request will be squashed and merged, and the automated workflow will rebuild the public site.

## 常见问题

**Q: Can I use this project behind a corporate firewall that blocks external HTTP requests?**

A: Yes. The build process does not require live network access to generate the site. Only the optional integrity checker (`check.py`) performs outbound requests. You can run the build script with the `--offline` flag to skip all network validation. Alternatively, you can pre-fetch the resources and store them locally, then modify the build script to point to local file paths instead of remote URLs.

**Q: How do I handle resources that return 301 or 302 redirects?**

A: The integrity checker follows redirects by default and reports the final resolved URL in its output. If a resource permanently moved, update the entry in `resources.md` to the new location. For temporary redirects, the checker will still mark the original URL as reachable, but we recommend updating the entry if the redirect persists for more than 30 days to reduce latency.

**Q: What happens if one resource fails to load during the build?**

A: By default, the build script logs a warning and continues processing the remaining resources. The generated HTML will include the failed entry with a visual indicator (e.g., a dashed border) to notify viewers. You can change this behavior by setting `strict_mode: true` in `config.yaml`, which will abort the build on the first failure and exit with a non-zero code, suitable for CI pipelines that require 100% resource validity.

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
