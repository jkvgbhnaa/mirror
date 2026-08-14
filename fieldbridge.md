# Yidian Knowledge Base Aggregator

Yidian Knowledge Base Aggregator 是一个轻量级的技术资源外链汇总与导航工具，专为技术社区运营者、文档维护者及个人知识管理者设计。该项目并非传统的内容管理系统，而是一个结构化的链接治理框架，通过集中化管理分散在多个子域名下的高质量技术文章、新闻与参考资源，帮助团队或个人建立可维护、可扩展的外部知识索引体系。

项目定位为“技术资源的入口层”，不存储实际内容，而是通过标准化清单对第三方链接进行归类、标注与版本追踪。目标用户包括开源项目文档组、企业内部技术培训团队、以及需要定期整理技术周报或新闻聚合的开发者。通过本项目提供的目录模板与脚本工具，用户可以快速将零散的 URL 资源转化为结构清晰、可公开访问的知识导航站点。

## 功能概览

- **多源链接聚合管理** 支持同时管理来自不同子域名（如 3g、h5、wap）的技术新闻与文章链接，并提供统一的条目去重与状态标记接口。

- **自动化目录树生成** 基于配置文件自动生成符合项目规范的 ASCII 目录结构，便于维护者快速理解资源存放逻辑与扩展点。

- **资源状态实时检测** 内置轻量级链接可达性检测模块，可定期检查已收录 URL 的响应状态，辅助清理失效资源。

- **分级标签与分类体系** 支持为每条外链标注技术领域、内容类型（教程/新闻/参考）、优先级等元数据，便于按场景筛选。

- **版本化资源清单导出** 提供 Markdown 与 JSON 两种格式的清单导出能力，方便集成到 CI/CD 流程或静态站点生成器中。

- **场景化快速过滤** 内置按日期、域名、内容关键词过滤的查询接口，提升日常维护与内容检索效率。

- **贡献审核工作流** 集成基于分支的贡献审核机制，新增或修改链接需经过审核流程，保证资源质量。

- **文档导航辅助生成** 根据资源清单自动生成文档导航表格，帮助访问者按问题类型或使用层面快速定位所需内容。

## 应用场景

- **技术团队内部知识周报整理** 技术负责人每周收集团队成员推荐的技术文章，通过本项目的资源清单模板统一录入，并自动生成可对外发布的周报导航页面。所有链接按域名与日期归档，便于回溯历史内容。

- **开源项目文档站的外部参考索引** 开源项目维护者需要在文档中引用大量外部规范、教程或案例。使用本项目建立独立的“外部资源”章节，将分散的链接集中管理，并定期检测链接有效性，避免文档中出现死链。

- **技术培训课程的配套阅读列表** 培训讲师为每期课程准备扩展阅读材料，材料来源包括多个子域名的技术新闻站。通过本项目的分类与标签功能，可按课程模块快速筛选对应资源，并生成学员可访问的整洁列表。

- **静态博客的友情链接与资源页** 个人技术博主使用本项目维护博客侧边栏的“推荐阅读”与“工具导航”区域。资源清单的版本化导出功能支持博主在站点重建时快速恢复链接配置。

- **企业合规性资源审计** 企业法务或合规部门需要定期审计外部引用的技术文档来源。本项目提供的链接状态检测与导出表格，可辅助生成审计报告所需的引用清单与访问记录。

## 快速开始

以下步骤帮助您在本地环境中快速启动 Yidian Knowledge Base Aggregator 实例，并导入初始资源列表。

```bash
# 克隆项目仓库至本地
git clone https://github.com/yidian-projects/kb-aggregator.git

# 进入项目根目录
cd kb-aggregator

# 安装项目依赖（需 Node.js 16+ 或 Python 3.9+，依据后端实现）
npm install

# 或使用 Python 虚拟环境
# python -m venv venv
# source venv/bin/activate
# pip install -r requirements.txt

# 运行初始资源导入脚本（将资源列表写入 data/sources.json）
npm run import:initial

# 启动本地开发服务器，默认监听端口 3000
npm start
```

完成上述步骤后，访问 `http://localhost:3000` 即可查看资源导航界面。导入脚本默认读取 `data/raw_links.txt` 文件中的 URL 列表，您可按格式替换该文件内容。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 16.x 或更高 | 运行核心服务与构建脚本，推荐使用 LTS 版本 |
| npm | 8.x 或更高 | 包管理器，用于安装项目依赖及运行任务 |
| Python | 3.9 或更高（可选） | 仅当使用 Python 后端版本时需要，用于链接检测模块 |
| Git | 2.25 或更高 | 用于克隆仓库及版本控制操作 |
| 磁盘空间 | 至少 50 MB | 用于存储配置文件、日志及临时缓存，不存储实际资源内容 |
| 内存 | 最低 512 MB，推荐 1 GB | 保证链接检测与清单导出任务流畅运行 |
| 网络 | 出站 HTTPS/HTTP 连通 | 用于检测外部链接可达性，需允许访问目标域名 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 跨平台支持，但生产环境推荐 Linux |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/getting-started.md` | 如何快速配置初始资源列表？本地开发环境如何启动？第一次导入数据需要哪些步骤？ |
| 配置手册 | `docs/configuration.md` | 如何修改资源分类标签？链接检测间隔如何调整？导出格式支持哪些类型？ |
| 维护操作 | `docs/maintenance.md` | 如何批量更新链接状态？如何清理重复条目？如何手动触发完整检测流程？ |
| 贡献规范 | `docs/contributing.md` | 提交新链接需要遵循什么格式？审核流程包含哪些步骤？如何回滚错误的链接变更？ |
| API 参考 | `docs/api-reference.md` | 提供了哪些内部接口用于查询资源？如何集成到外部监控系统？状态码含义是什么？ |
| 常见问题 | `docs/faq.md` | 链接检测超时如何处理？子域名被封禁有何替代方案？能否迁移至其他数据库后端？ |

## 资源列表

- http://3g.yidianmeii.cn/snews/83900.shtml
- http://h5.yidianmeii.cn/snews/6680666.shtml
- http://3g.yidianmeii.cn/snews/5161661.shtml
- http://3g.yidianmeii.cn/snews/0116069.shtml
- http://wap.yidianmeii.cn/snews/9611089.shtml
- http://3g.yidianmeii.cn/snews/383937.shtml
- http://3g.yidianmeii.cn/snews/980190.shtml
- http://wap.yidianmeii.cn/snews/0836.shtml
- http://h5.yidianmeii.cn/snews/5381.shtml
- http://h5.yidianmeii.cn/snews/711060.shtml

## 项目结构

```
kb-aggregator/
├── bin/                                 # 可执行脚本目录
│   ├── check-links.js                   # 链接状态检测命令行工具
│   └── export-json.js                   # 资源清单 JSON 导出工具
├── config/                              # 项目全局配置文件
│   ├── default.yaml                     # 默认配置（分类、检测间隔、端口）
│   └── schema.json                      # 配置文件 JSON Schema 校验定义
├── data/                                # 数据存储目录（不包含实际资源内容）
│   ├── sources.json                     # 主资源清单，包含所有 URL 及元数据
│   ├── raw_links.txt                    # 初始导入的原始链接列表（纯文本）
│   └── audit.log                        # 链接检测与变更操作审计日志
├── docs/                                # 文档目录，包含各层面说明
│   ├── getting-started.md               # 入门指南
│   ├── configuration.md                 # 配置手册
│   ├── maintenance.md                   # 维护操作说明
│   ├── contributing.md                  # 贡献规范
│   ├── api-reference.md                 # API 参考
│   └── faq.md                           # 常见问题汇总
├── scripts/                             # 辅助维护脚本
│   ├── deduplicate.js                   # 资源列表去重脚本
│   ├── generate-tree.sh                 # 自动生成 ASCII 目录树脚本
│   └── import-initial.js                # 初始数据导入脚本（读取 raw_links.txt）
├── src/                                 # 核心源代码目录
│   ├── core/                            # 核心逻辑模块
│   │   ├── aggregator.js                # 资源聚合与分类引擎
│   │   └── validator.js                 # 链接格式与可达性校验器
│   ├── http/                            # HTTP 服务层
│   │   ├── server.js                    # 开发服务器入口
│   │   └── routes.js                    # 路由定义（状态页、导出接口）
│   └── utils/                           # 通用工具函数
│       ├── logger.js                    # 日志记录封装
│       └── formatter.js                 # Markdown / JSON 格式化工具
├── test/                                # 单元测试与集成测试目录
│   ├── unit/                            # 核心模块单元测试
│   └── integration/                     # 端到端检测流程测试
├── .gitignore                           # Git 忽略文件配置
├── LICENSE                              # MIT 许可证文件
├── package.json                         # Node.js 项目依赖与脚本定义
└── README.md                            # 项目主说明文档（本文件）
```

## 贡献指南

1. **克隆仓库并创建特性分支** 从主仓库 fork 项目，然后在本地克隆 fork 后的副本。新建一个描述性的特性分支，例如 `feat/add-resource-tags` 或 `fix/link-checker-timeout`，确保分支名称清晰反映变更意图。

2. **遵循资源清单格式规范** 若新增或修改资源链接，必须遵守 `data/sources.json` 中定义的 JSON Schema 结构。每条记录需包含 `url`、`domain`、`status`、`tags` 及 `lastChecked` 字段。新增链接时，`status` 初始值应设为 `pending`，后续由检测脚本自动更新。

3. **运行本地测试与检查** 提交变更前，需在本地执行 `npm run test` 运行全部单元测试和集成测试，确保现有功能未受影响。同时执行 `npm run lint` 进行代码风格检查，并手动验证开发服务器可正常启动且资源列表显示正确。

4. **编写或更新相关文档** 若变更涉及用户可见的行为（如新增配置项、修改 API 接口），必须在 `docs/` 目录下对应的文档文件中补充说明。对于新增资源类别，需在 `docs/configuration.md` 中更新分类示例。

5. **提交 Pull Request 并等待审核** 将特性分支推送至您的远程仓库，然后向主仓库的 `main` 分支发起 Pull Request。PR 描述中需明确列出变更内容、测试结果以及是否影响现有资源清单。项目维护者将在 2-3 个工作日内审核，必要时会提出修改意见。

## 常见问题

**Q: 链接检测脚本频繁超时或返回错误状态，应如何调整？**

A: 检测超时通常由目标服务器响应慢或网络限制引起。您可以在 `config/default.yaml` 中调整 `checker.timeout` 参数（单位毫秒），建议值从 5000 逐步增加至 15000。同时，检查 `checker.userAgent` 配置，部分站点会拦截非浏览器请求，可将其修改为常见浏览器 UA 字符串。若仍频繁失败，可使用 `bin/check-links.js --retry 3` 启用重试机制。

**Q: 我能否使用其他编程语言或数据库来替代 Node.js 部分？**

A: 本项目核心逻辑与数据存储解耦，资源清单仅依赖 JSON 文件。您完全可以编写 Python、Go 或 Rust 版本的解析与检测工具，只要保持 `data/sources.json` 的 Schema 不变即可。社区已有用户贡献了 Python 版的链接检测脚本，位于 `contrib/python-checker/` 目录下（需自行下载）。官方版本仍以 Node.js 为主，但欢迎提交替代实现的 PR。

**Q: 如何迁移已有的大量历史链接至本项目？**

A: 首先将历史链接整理为纯文本文件，每行一个 URL，保存为 `data/raw_links.txt`。然后运行 `npm run import:initial`，脚本会自动读取该文件并生成符合 Schema 的初始 `sources.json`。如果历史链接包含分类或日期信息，建议在导入前编写简单的预处理脚本，将元数据转换为 `tags` 字段的数组格式。对于超过 1000 条链接的批量导入，推荐分批次执行并间隔 1 分钟，避免检测线程过载。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
