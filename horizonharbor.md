# Yidian News Aggregator

Yidian News Aggregator is a lightweight, open-source news resource indexing and external link aggregation platform designed for developers, data analysts, and content researchers who need to programmatically access structured news metadata from distributed mobile news endpoints. The project does not host or republish copyrighted content; instead, it provides a normalized indexing layer over publicly accessible news article URLs, enabling batch retrieval, duplicate detection, and temporal correlation analysis across multiple subdomains and publication timestamps.

Target users include open-source intelligence researchers, SEO practitioners, academic data collectors, and journalism students who require reproducible workflows for gathering news article references from mobile-optimized sources. The system implements a minimal dependency crawler harness with configurable rate limiting, user-agent rotation, and response validation, outputting structured JSONL logs suitable for downstream ETL pipelines. All collected metadata remains under the control of the user, and no data is cached or persisted beyond the runtime session unless explicitly configured.

## 功能概览

- **Subdomain-aware endpoint discovery** – Automatically resolves and normalizes article URLs from 3g, wap, and h5 subdomain variants, preserving the original protocol and hostname without alteration.

- **Batch URL ingestion with deduplication** – Accepts plain-text URL lists or line-delimited files, removes exact duplicates, and performs structural deduplication based on path segments and query parameters.

- **Configurable request throttling** – Provides per-domain concurrent request limits, retry backoff strategies, and jittered delays to reduce server load and avoid unintentional traffic spikes.

- **Response metadata extraction** – Parses HTML title tags, meta description fields, publication date hints from structured data, and content-length headers, outputting normalized fields in JSON format.

- **Export adapters** – Supports stdout, JSONL file, and SQLite output modes, allowing seamless integration with data visualization tools and custom analysis notebooks.

- **Integrity validation hooks** – Validates HTTP status codes, content-type headers, and response size thresholds, flagging anomalous entries for manual review.

- **Stateless operation design** – No persistent database or configuration files required; all runtime parameters are passed via command-line arguments or environment variables.

## 应用场景

1. **Journalistic source verification** – Researchers can feed a list of article URLs from mobile news portals into the aggregator to automatically collect titles and publication timestamps, enabling chronological reconstruction of event coverage across multiple outlets.

2. **SEO audit and backlink monitoring** – Digital marketers use the tool to periodically scan news article endpoints to detect broken links, track redirect chains, and monitor changes in meta description length and keyword density.

3. **Academic content analysis** – Social science researchers run batch collections on dated article corpora to perform linguistic analysis, sentiment scoring, or topic modeling without manually copying metadata from each browser tab.

4. **News archive reconstruction** – Archivists and data curators leverage the aggregator to generate manifest files of article references from specific date ranges, which can be cross-referenced with Internet Archive snapshots for preservation auditing.

5. **Quality assurance for content delivery networks** – Platform engineers verify that mobile news subdomains return consistent response headers and proper content-encoding by running nightly validation jobs against predefined URL lists, with alerts triggered on anomalies.

## 快速开始

Below is the standard three-step bootstrap procedure for cloning, installing dependencies, and running a basic ingestion job. Ensure that your system meets the requirements listed in the subsequent section before execution.

```bash
# Step 1: Clone the repository from the public mirror
git clone https://github.com/opensource-archives/yidian-news-aggregator.git
cd yidian-news-aggregator

# Step 2: Install required Python packages inside a virtual environment
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt

# Step 3: Run a sample ingestion with the provided example URL list
python aggregator.py --input examples/sample_urls.txt --output results.jsonl --concurrency 3
```

## 安装要求

The following table enumerates the mandatory runtime dependencies, their minimum versions, and specific configuration notes. All dependencies are available via PyPI and standard system package managers.

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 – 3.11 | Earlier versions lack asyncio features; later versions are not yet fully tested. |
| aiohttp | 3.9.0+ | Used for asynchronous HTTP client sessions with connection pooling and timeout controls. |
| lxml | 4.9.0+ | Required for robust HTML parsing; falls back to html.parser if unavailable but with reduced performance. |
| python-dotenv | 1.0.0+ | Optional but recommended for managing environment-specific variables such as user-agent strings. |
| pytest | 7.0.0+ | Only needed for running the test suite; not required for production ingestion. |
| click | 8.0.0+ | Command-line interface framework for subcommand parsing and argument validation. |
| structlog | 22.0.0+ | Structured logging with JSON output support for downstream log aggregation systems. |

## 文档导航

The table below organizes the available documentation resources by architectural layer, target directory, and the primary questions each document answers.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| User Guide | docs/user-guide/quickstart.md | How do I run my first ingestion with a custom URL list? What output formats are supported? |
| Configuration Reference | docs/configuration/runtime-options.md | Which command-line flags and environment variables control concurrency, timeouts, and retry policies? |
| Architecture Overview | docs/architecture/request-pipeline.md | How does the request scheduler manage rate limiting and backoff across multiple subdomains? |
| Output Schema | docs/architecture/output-schema.md | What fields are included in each JSON record? How are missing metadata fields handled? |
| Testing Guide | docs/developer/testing.md | How can I run the unit test suite and contribute new test cases for custom parsers? |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/179293.shtml
- http://wap.yidianmeii.cn/jnews/0814/606825.shtml
- http://wap.yidianmeii.cn/jnews/0814/8166.shtml
- http://h5.yidianmeii.cn/jnews/0814/445387.shtml
- http://wap.yidianmeii.cn/jnews/0814/5827.shtml
- http://3g.yidianmeii.cn/jnews/0814/861655.shtml
- http://wap.yidianmeii.cn/jnews/0814/7968732.shtml
- http://3g.yidianmeii.cn/jnews/0814/0694.shtml
- http://h5.yidianmeii.cn/jnews/0814/8280.shtml
- http://h5.yidianmeii.cn/jnews/0814/5853.shtml

## 项目结构

The repository follows a modular monolith layout with clear separation between core logic, parsers, export handlers, and supporting utilities. Each directory is annotated with its primary responsibility.

```
yidian-news-aggregator/
├── aggregator/                    # Main package root
│   ├── __init__.py               # Package metadata and version string
│   ├── cli.py                    # Click entrypoint with subcommand definitions
│   ├── runner.py                 # Core orchestration loop; manages the ingestion lifecycle
│   ├── config.py                 # Configuration loader merging env vars and defaults
│   ├── fetcher/                  # HTTP request and response handling layer
│   │   ├── session.py            # aiohttp session factory with timeout and connector settings
│   │   ├── throttler.py          # Sliding-window rate limiter per target hostname
│   │   └── user_agent.py         # Rotating user-agent provider from a curated list
│   ├── parsers/                  # Subdomain-specific HTML and metadata extractors
│   │   ├── base.py               # Abstract parser interface and common utilities
│   │   ├── wap_parser.py         # Parser logic optimized for wap.yidianmeii.cn pages
│   │   ├── h5_parser.py          # Parser for h5 subdomain variants
│   │   └── generic_parser.py     # Fallback parser using generic meta tag detection
│   ├── exporters/                # Output formatters and storage backends
│   │   ├── jsonl.py              # JSONL streaming writer with atomic flush
│   │   ├── sqlite.py             # SQLite exporter with schema migration on first run
│   │   └── stdout.py             # Human-readable colored console output for debugging
│   ├── validators/               # Response integrity and content-type verification
│   │   ├── status.py             # HTTP status code whitelist and retry decision logic
│   │   └── content.py            # Minimum content-length and MIME-type validators
│   └── utils/                    # Miscellaneous helper modules
│       ├── url_normalizer.py     # Subdomain detection and path sanitization (preserves original URL)
│       ├── dedup.py              # In-memory set-based deduplication with optional LRU cap
│       └── logger.py             # Structlog configuration with JSON and pretty-print sinks
├── tests/                        # Unit and integration test suite
│   ├── conftest.py               # Pytest fixtures for mock responses and temporary directories
│   ├── test_fetcher.py           # Tests for session retry logic and timeout handling
│   ├── test_parsers.py           # Parser output validation against sample HTML fixtures
│   └── test_exporters.py         # Exporter correctness for JSONL and SQLite formats
├── docs/                         # End-user and developer documentation
│   ├── user-guide/               # Step-by-step tutorials and usage examples
│   ├── architecture/             # System design decisions and data flow diagrams
│   ├── developer/                # Contribution workflow and coding standards
│   └── reference/                # API reference generated from docstrings
├── examples/                     # Sample input files and demonstration scripts
│   ├── sample_urls.txt           # A small list of seed URLs for quick testing
│   └── full_batch_19.txt         # The complete 10-item batch used in this release
├── requirements.txt              # Production runtime dependencies pinned to stable versions
├── requirements-dev.txt          # Additional dependencies for testing and documentation builds
├── pyproject.toml                # PEP 621 project metadata and build system configuration
├── README.md                     # This document – project overview and entry point
└── LICENSE                       # MIT license text
```

## 贡献指南

Contributions to Yidian News Aggregator are welcome from developers, data scientists, and documentation writers. Please follow the steps below to ensure a smooth integration process.

1. **Fork the repository and create a feature branch** – Navigate to the GitHub repository, fork it to your personal account, and create a new branch with a descriptive name such as `feature/add-h5-meta-parser` or `fix/retry-backoff-calculation`.

2. **Set up the development environment** – Clone your fork locally, run `make dev-setup` (or manually install `requirements-dev.txt`), and verify that all existing tests pass with `pytest tests/`. This ensures that your changes do not introduce regressions.

3. **Implement your change with accompanying tests** – For any new parser, validator, or exporter, add corresponding test fixtures under the `tests/` directory. For bug fixes, provide a minimal test case that reproduces the issue before your patch.

4. **Update documentation and examples** – If your contribution modifies the command-line interface or output schema, update the relevant sections in the `docs/` folder and provide a new example snippet if applicable. Keep the README in sync for user-facing changes.

5. **Submit a pull request with a clear change log** – Open a pull request against the main branch, fill out the provided template, and include a bulleted list of changes. Maintainers will review the submission within 5 business days and may request additional modifications or clarifications.

## 常见问题

**Q: Does this aggregator store or redistribute the full text of news articles?**

A: No. The aggregator only fetches HTTP headers, title tags, meta descriptions, and visible publication date hints. It does not save the main body content of articles, nor does it cache full HTML pages beyond the lifetime of a single request. All data processing is ephemeral and limited to metadata extraction as explicitly configured by the user.

**Q: How can I handle URLs that return 403 Forbidden or 429 Too Many Requests?**

A: The throttler module automatically applies exponential backoff starting from 1 second, up to a maximum of 60 seconds, for responses with 429 status codes. For 403 errors, the system logs a warning and marks the URL as skipped after the first attempt, as these responses typically indicate server-side access controls that cannot be overridden programmatically. You can adjust the retry thresholds via the `--max-retries` and `--backoff-factor` command-line options.

**Q: Can I run this tool behind a corporate proxy or with custom TLS certificates?**

A: Yes. The aiohttp session respects the standard `HTTP_PROXY` and `HTTPS_PROXY` environment variables. For custom certificate authorities, set the `SSL_CERT_FILE` environment variable to point to your PEM bundle. The tool does not bundle any root certificates and relies entirely on the system trust store, which can be overridden via the `--ssl-verify` flag.

## 许可证

MIT

Copyright (c) 2026 Yidian News Aggregator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
