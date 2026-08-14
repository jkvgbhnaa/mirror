# Yidian Resource Aggregator

Yidian Resource Aggregator is a lightweight, open-source information aggregation and navigation system designed for technical content curators, researcher communities, and digital archivists who need to systematically organize, categorize, and surface distributed web resources. The project addresses the common challenge of managing large collections of external references by providing a structured indexing framework, metadata extraction tooling, and a clean presentation layer that transforms raw URL lists into navigable knowledge repositories. Targeting developers, technical writers, and data journalists, this tool simplifies the maintenance of resource-heavy documentation sites while ensuring link integrity and content discoverability.

The aggregator operates as a static site generation pipeline that consumes plain-text URL inventories, enriches each entry with status checking, content fingerprinting, and semantic tagging, then produces a fully searchable HTML dashboard. Unlike general-purpose bookmark managers, Yidian Resource Aggregator focuses on batch processing, automated health checks, and category inference, making it particularly suitable for projects that handle hundreds or thousands of external links across multiple domains. The system is built with modularity in mind, allowing users to plug in custom parsers, output formatters, and notification handlers without modifying the core engine.

## 功能概览

- **批量链接导入与解析** 支持从纯文本文件、CSV 或直接粘贴的 URL 列表中批量导入链接，自动解析协议、域名、路径参数，并提取页面标题和元描述作为初步索引信息。

- **自动可用性检测** 后台调度器定期对已收录链接执行 HTTP 探活，检测响应状态码、重定向链和加载耗时，标记失效或缓慢链接以供人工复核。

- **内容指纹去重** 对每个目标页面计算基于 DOM 结构的内容哈希值，识别近似重复或完全相同的文章，避免资源列表中出现冗余条目。

- **分类标签推荐引擎** 基于 URL 路径关键词、域名归属和页面关键词频率，自动为每条资源生成 3 至 5 个候选分类标签，并支持用户自定义规则覆盖。

- **多格式站点生成** 可将索引数据输出为静态 HTML 目录页、Markdown 表格、JSON API 端点或 CSV 报告，满足文档站点、数据交换和离线归档等不同场景。

- **链接变更追踪** 记录每次检测时目标页面的最后修改时间或内容长度变化，生成变更日志，帮助运营者感知外部资源的更新频率和内容演变。

- **权限分级视图** 支持为公开资源、内部参考和受限访问链接分别设置可见性级别，在生成站点时按用户角色过滤显示条目。

## 应用场景

**技术文档库维护** 开源项目维护者可以使用本工具管理文档中引用的所有外部教程、API 参考和社区讨论链接，定期检查链接有效性，确保用户文档始终保持高质量可用状态。

**行业资讯周报生成** 内容编辑团队每周收集数十篇行业报道和博客文章，通过批量导入生成带分类标签的资源列表，一键导出为 Markdown 格式直接嵌入周报邮件或网站新闻栏目。

**学术参考文献整理** 研究人员在文献调研阶段积累大量在线论文、数据集页面和工具仓库地址，利用自动去重和指纹功能快速清理重复条目，并按主题标签组织成结构化参考文献索引。

**运维监控仪表盘** 运维团队将内部监控面板、日志查询入口和警报管理后台等关键链接纳入系统，开启自动可用性检测后，任何内部服务 URL 的异常响应都能被及时发现并通知责任人。

**离线阅读包构建** 对于网络受限环境下的知识库同步需求，可借助本工具的输出模块生成包含所有链接元数据的清单，配合第三方离线下载工具批量抓取页面内容打包分发。

## 快速开始

以下指令适用于 Linux 及 macOS 环境，Windows 用户建议通过 WSL 或 Git Bash 执行。

```bash
# 克隆项目仓库至本地
git clone https://github.com/yidian-dev/resource-aggregator.git

# 进入项目根目录
cd resource-aggregator

# 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 使用示例数据运行初次构建
cp config/example_urls.txt data/input/urls.txt
python cli.py build --input data/input/urls.txt --output dist/

# 启动本地预览服务器
python -m http.server --directory dist/ 8000
```

执行完成后，在浏览器中访问 `http://127.0.0.1:8000` 即可查看生成的资源导航站点。如需自定义分类规则或调整检测频率，请编辑 `config/settings.yaml` 文件。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行环境，建议使用 3.11 或 3.12 以获得更好性能 |
| pip | 22.0 及以上 | Python 包管理工具，用于安装第三方依赖库 |
| requests | 2.31.0 | HTTP 客户端库，用于链接可用性检测和页面抓取 |
| beautifulsoup4 | 4.12.0 | HTML 解析库，用于提取页面标题、元描述和内容指纹 |
| lxml | 4.9.0 | 高性能 XML/HTML 解析器，作为 beautifulsoup4 的底层解析引擎 |
| pyyaml | 6.0 | YAML 配置文件解析库，用于读取用户自定义设置 |
| jinja2 | 3.1.0 | 模板引擎，用于生成静态 HTML 页面和 Markdown 文档 |
| pytest | 7.4.0 | 单元测试框架，仅在开发环境中需要（非运行时必需） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | 如何快速安装、配置第一个数据源并生成站点？ |
| 配置手册 | docs/configuration.md | settings.yaml 中每个字段的含义是什么？如何自定义分类标签和检测间隔？ |
| 命令行接口 | docs/cli-usage.md | build、check、export、serve 等命令的完整参数说明和示例用法。 |
| 输出模板开发 | docs/template-customization.md | 如何修改 HTML 模板样式、添加自定义布局或新增输出格式？ |
| 插件系统 | docs/plugin-architecture.md | 如何编写自定义解析器、通知处理器或内容过滤器？插件加载顺序和生命周期是怎样的？ |
| API 参考 | docs/api-reference.md | 核心模块（loader、fingerprint、checker、renderer）的类和方法详细说明。 |

## 资源列表

- http://wap.yidianmeii.cn/snews/9918.shtml
- http://h5.yidianmeii.cn/snews/9403873.shtml
- http://3g.yidianmeii.cn/snews/332902.shtml
- http://3g.yidianmeii.cn/snews/34251.shtml
- http://3g.yidianmeii.cn/snews/1512563.shtml
- http://3g.yidianmeii.cn/snews/179293.shtml
- http://wap.yidianmeii.cn/snews/606825.shtml
- http://wap.yidianmeii.cn/snews/8166.shtml
- http://h5.yidianmeii.cn/snews/445387.shtml
- http://wap.yidianmeii.cn/snews/5827.shtml

## 项目结构

```
resource-aggregator/
├── cli.py                      # 命令行入口，注册所有子命令
├── config/
│   ├── settings.yaml           # 主配置文件（检测间隔、分类规则、输出选项）
│   └── example_urls.txt        # 示例输入文件，展示 URL 列表格式
├── core/
│   ├── __init__.py
│   ├── loader.py               # 链接导入模块，支持 txt/csv 及直接粘贴
│   ├── checker.py              # HTTP 可用性检测器，含重定向跟踪和超时控制
│   ├── fingerprint.py          # 内容指纹计算器，基于 DOM 树哈希
│   ├── classifier.py           # 分类标签推荐引擎，基于规则和词频
│   └── exporter.py             # 多格式导出器（HTML / Markdown / JSON / CSV）
├── plugins/
│   ├── __init__.py
│   ├── notification_email.py   # 邮件通知插件，发送失效链接报告
│   └── filter_domains.py       # 域名白名单/黑名单过滤插件示例
├── templates/
│   ├── base.html               # HTML 站点主模板，含导航和搜索框
│   ├── card_layout.html        # 卡片式资源列表子模板
│   └── table_layout.html       # 表格式资源列表子模板
├── tests/
│   ├── test_checker.py         # 可用性检测模块单元测试
│   ├── test_fingerprint.py     # 内容指纹去重逻辑测试
│   └── fixtures/               # 测试用静态 HTML 样本文件
├── dist/                       # 默认输出目录，构建产物存放于此
├── requirements.txt            # 生产环境依赖清单
└── README.md                   # 项目说明文档（即本文档）
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于代码提交、文档改进、问题报告和功能建议。请遵循以下流程：

1. 在 GitHub 上 Fork 本仓库至您的个人账号，并克隆到本地开发环境。确保您本地安装了 Python 3.9 及以上版本和必要的开发依赖（参见 `requirements-dev.txt`）。

2. 创建新的特性分支，分支命名建议采用 `feature/简短描述` 或 `fix/问题编号` 格式。在提交代码前，请运行 `pytest tests/` 确保所有现有测试通过，并为新增功能编写对应的单元测试。

3. 提交代码时使用清晰且语义化的 commit 信息，遵循 Conventional Commits 规范（例如 `feat: add retry mechanism for failed checks` 或 `docs: update configuration reference`）。提交前请确保代码风格符合 `flake8` 和 `black` 的默认配置。

4. 向本仓库的主分支发起 Pull Request，在 PR 描述中详细说明变更目的、实现思路和测试覆盖情况。如果 PR 涉及用户可见的功能变化，请同步更新 `docs/` 目录下的相关文档。

5. 等待维护者进行代码审查。审查过程中可能需要您补充测试用例或调整实现细节。合并后您的贡献将出现在下一版本的更新日志中，并可在项目官网的贡献者列表中获得署名。

## 常见问题

**Q: 系统如何处理目标页面需要登录或存在访问限制的情况？**

A: 默认情况下，可用性检测器仅检查 HTTP 状态码是否为 2xx 或 3xx，对于返回 401 或 403 的链接会标记为“受限访问”而非“失效”。您可以在 `settings.yaml` 中配置 `auth_required_status_codes` 列表来自定义哪些状态码被视为需登录，并可设置 `skip_auth_check` 为 true 让检测器完全跳过对这些链接的状态判断，仅记录响应头信息。

**Q: 内容指纹去重是否会因为页面广告或动态脚本变动而产生误判？**

A: 指纹计算器在默认配置下会忽略 `<script>`、`<style>`、`<noscript>` 标签以及所有 `data-*` 属性，同时会对文本内容进行空白符归一化处理。如果您发现某些内容相似但实际不同的页面被错误归为重复，可以调整 `fingerprint_ignore_selectors` 配置项，增加需要忽略的 CSS 选择器列表，或修改 `fingerprint_sensitivity` 参数控制哈希计算的严格程度。

**Q: 我可以将本工具集成到现有的 CI/CD 流水线中吗？**

A: 完全可以。本工具提供纯命令行接口，所有输出均写入文件系统而不依赖外部服务，适合在 GitHub Actions、GitLab CI 或 Jenkins 中作为定时任务运行。推荐的工作流是：在仓库中维护 `data/input/urls.txt` 文件，每次推送更新后由 CI 触发 `cli.py build` 命令，并将生成的 `dist/` 目录部署到静态托管服务（如 GitHub Pages 或 Cloudflare Pages）。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
