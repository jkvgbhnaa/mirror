# yidianmeii-archive

yidianmeii-archive 是一个轻量级的技术资讯与新闻外链归档系统，专为需要批量采集、管理和分发来自移动端资讯源（如 yidianmeii.cn 子域名）的内容链接而设计。该项目面向内容聚合站点运营者、舆情监控工具开发者以及个人研究用途的数据归档需求，提供标准化的 URL 采集、结构化存储与快速检索能力。

本项目并非一个完整的 CMS 或全文抓取引擎，而是一个专注于外链元数据管理与分类索引的中间层工具。它通过约定式目录结构和纯文本标记，将分散在多个移动端子域名（3g、h5、wap）下的新闻资源链接进行统一规整，并辅以时间戳、来源标签和状态标记，便于后续批量处理、去重校验或导入第三方分析系统。核心设计原则为最小依赖、零配置启动和可移植性，适合在低资源环境（如单核服务器、树莓派或开发机）中长期运行。

## 功能概览

**多子域名源同步**：支持从 3g.yidianmeii.cn、h5.yidianmeii.cn 和 wap.yidianmeii.cn 三个移动端子域名下按日期路径（如 /jnews/0814/）批量拉取新闻链接清单，并自动识别来源子域名。

**链接元数据提取**：从 URL 中解析出日期批次（如 0814）、文件名 ID 和扩展名，生成标准化记录条目，包含原始链接、来源子域名、采集时间戳和校验哈希。

**去重与状态标记**：基于链接 ID 和来源子域名进行双重去重，自动标记新增、重复或失效链接状态，避免重复入库。

**分类标签生成**：根据链接来源子域名和路径特征，自动生成分类标签（如 3g-mobile、h5-mobile、wap-standard），便于按终端类型或访问渠道筛选。

**纯文本索引输出**：将所有归档链接以行记录形式输出至索引文件，支持 .txt 和 .csv 两种格式，每行仅包含一个 URL，符合最小化数据交换规范。

**增量更新支持**：通过记录上次同步时间戳，仅拉取当前日期批次下尚未归档的新链接，降低网络和存储开销。

**可配置忽略规则**：支持通过 .ignore 文件自定义需要跳过的 URL 模式（如特定 ID 段或文件类型），灵活控制归档范围。

**本地缓存与回退**：归档链接列表本地缓存 7 天，当源站不可访问时自动切换至缓存数据，保证服务可用性。

## 应用场景

内容聚合站点的链接源管理：运营者可将 yidianmeii-archive 作为上游数据源，定期拉取指定日期批次下的新闻链接，经过去重和分类后，批量导入自己的内容发布系统或 RSS 生成器，避免手动复制粘贴的低效操作。

舆情监控与趋势分析：研究人员或舆情分析师利用本工具归档特定时间段（如 8 月 14 日）的移动端新闻链接，结合第三方正文提取服务，可快速构建语料库，用于热点话题追踪、传播路径分析或情感倾向研究。

个人知识库的自动化采集：个人开发者或博主可将本项目配置为定时任务，每日自动拉取 yidianmeii.cn 各子域名下的最新链接，并配合本地 Markdown 笔记工具（如 Obsidian 或 Logseq）生成带来源链接的阅读清单，实现个性化资讯聚合。

数据迁移与备份校验：当需要将某批次链接迁移至新的存储系统或 CDN 时，可使用本工具导出的纯链接列表进行批量校验，确保所有 URL 格式合法且来源可追溯，降低迁移过程中的数据丢失风险。

## 快速开始

以下步骤假设您已安装 Git 和 Python 3.8 及以上版本，并位于 Linux/macOS 或 Windows WSL 环境中。

```bash
# 克隆项目仓库
git clone https://github.com/your-org/yidianmeii-archive.git
cd yidianmeii-archive

# 安装依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 运行快速采集任务（默认采集当前日期前一天的链接）
python archive.py --run

# 指定采集特定日期批次，例如 8 月 14 日
python archive.py --date 0814 --output ./output/links_0814.txt
```

执行完毕后，归档链接将输出到 ./output/ 目录下的对应文件中，默认文件名为 links_YYYYMMDD.txt，每行一个 URL。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 或更高 | 核心运行环境，推荐使用 3.10+ 以获得性能改进 |
| requests | 2.28.0+ | 用于发送 HTTP 请求拉取源站链接清单 |
| beautifulsoup4 | 4.11.0+ | 解析源站 HTML 结构，提取链接节点 |
| lxml | 4.9.0+ | 作为 beautifulsoup4 的解析器后端，提供更快的 XML/HTML 解析 |
| python-dotenv | 1.0.0+ | 管理环境变量配置文件，用于存储可选的代理或超时设置 |
| pytest | 7.2.0+ | 仅开发测试时需要，用于运行单元测试套件 |
| flake8 | 6.0.0+ | 仅代码 lint 时需要，用于保持代码风格一致性 |
| setuptools | 65.0.0+ | 用于本地安装和构建分发包 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/usage.md | 如何配置采集参数、如何自定义输出格式、如何处理采集失败的情况？ |
| 开发者指南 | docs/development.md | 项目代码结构如何组织、如何新增一个子域名源、如何扩展元数据字段？ |
| 部署参考 | docs/deployment.md | 如何设置为 systemd 定时任务、如何配置日志轮转、如何迁移缓存目录？ |
| API 设计 | docs/api.md | 内部模块之间的接口契约是什么、采集器类的输入输出规范、如何编写插件？ |
| 常见流程 | docs/workflows.md | 从拉取到去重再到索引输出的完整数据流图示、各阶段耗时参考值。 |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/3157650.shtml
- http://3g.yidianmeii.cn/jnews/0814/3711.shtml
- http://h5.yidianmeii.cn/jnews/0814/6966.shtml
- http://wap.yidianmeii.cn/jnews/0814/69731.shtml
- http://3g.yidianmeii.cn/jnews/0814/16180.shtml
- http://wap.yidianmeii.cn/jnews/0814/639139.shtml
- http://h5.yidianmeii.cn/jnews/0814/66166.shtml
- http://wap.yidianmeii.cn/jnews/0814/660038.shtml
- http://h5.yidianmeii.cn/jnews/0814/8056.shtml
- http://3g.yidianmeii.cn/jnews/0814/911116.shtml

## 项目结构

```
yidianmeii-archive/
├── archive.py                # 主入口脚本，解析命令行参数并调度采集流程
├── requirements.txt          # 生产环境依赖列表，供 pip 安装使用
├── .env.example              # 环境变量模板，包含代理、超时和缓存路径示例
├── .gitignore                # 忽略缓存、日志和虚拟环境目录
├── src/                      # 核心源代码目录
│   ├── __init__.py           # 包初始化文件，导出主要采集器类
│   ├── fetcher.py            # 子域名链接拉取模块，含请求重试和超时控制
│   ├── parser.py             # HTML 解析与链接提取模块，基于 beautifulsoup4
│   ├── dedupe.py             # 去重引擎，支持内存集合和可选 Redis 后端
│   ├── indexer.py            # 索引输出模块，生成 .txt 和 .csv 格式文件
│   └── utils/                # 通用工具函数集
│       ├── __init__.py
│       ├── date_helpers.py   # 日期批次转换与校验函数
│       └── url_normalizer.py # URL 标准化、子域名识别与路径规整
├── tests/                    # 单元测试目录
│   ├── test_fetcher.py       # 模拟源站响应，测试拉取逻辑
│   ├── test_parser.py        # 测试不同 HTML 结构下的链接解析准确率
│   └── test_dedupe.py        # 去重算法的边界条件测试
├── output/                   # 默认输出目录，所有索引文件存放于此
│   └── .keep                 # 占位文件，确保目录被 git 追踪
├── cache/                    # 本地缓存目录，存储最近 7 天的原始响应
│   └── .keep
├── logs/                     # 运行日志目录，按天轮转
│   └── .keep
├── docs/                     # 完整文档目录
│   ├── usage.md
│   ├── development.md
│   ├── deployment.md
│   ├── api.md
│   └── workflows.md
└── LICENSE                   # MIT 许可证文件
```

## 贡献指南

1.  Fork 本仓库至您的个人账户，并克隆到本地开发环境。确保您已安装所有开发依赖（见 requirements.txt 及 dev-requirements.txt）。

2.  新建一个功能分支，分支名称应反映本次修改的目的，例如 `feature/add-h5-parser` 或 `fix/dedupe-memory-leak`。请勿在主分支上直接修改。

3.  编写代码或修改现有模块时，请遵循 PEP 8 编码规范，并使用 flake8 进行静态检查。所有新增功能必须附带对应的单元测试，测试覆盖率不低于 80%。

4.  提交 commit 时，请使用明确的英文日志信息，格式为 `<type>: <short description>`，其中 type 可选 feat、fix、docs、style、refactor、test、chore。若涉及重大变更，请在 commit body 中详细说明。

5.  推送分支至您的远程仓库，然后通过 GitHub 界面发起 Pull Request 到本仓库的 main 分支。PR 描述中请清晰列出变更内容、测试结果和相关问题编号（如有）。等待核心维护者审核，并根据反馈进行修订。

## 常见问题

Q: 采集过程中出现 HTTP 404 或 503 错误，如何应对？

A: 本工具内置了指数退避重试机制（默认最多重试 3 次），若仍然失败，程序会自动切换至本地缓存数据（如果有）。您也可以手动指定 `--retry 5` 增加重试次数，或通过 `--timeout 30` 调整请求超时时间。如果特定子域名持续不可用，可使用 `.ignore` 文件临时屏蔽该域名的采集。

Q: 如何扩展以支持新的子域名或路径格式？

A: 您无需修改核心代码，只需在 `src/fetcher.py` 中的 `DOMAIN_MAP` 字典里添加新的子域名条目，并指定对应的路径模板即可。若新源的结构与现有解析逻辑差异较大，建议继承 `BaseParser` 类并重写 `parse_links` 方法，然后在 `parser.py` 的工厂函数中注册您的解析器类。

Q: 采集到的链接数量与源站实际数量不一致，可能是什么原因？

A: 通常原因有三个：一是源站采用分页或动态加载机制，而本工具默认只抓取首屏；二是部分链接被 `.ignore` 规则匹配跳过；三是源站响应内容中包含了非标准路径（如相对路径或缺少协议）。您可以通过启用 `--debug` 模式查看详细日志，定位被过滤或解析失败的链接。若需处理分页，请参考 `docs/development.md` 中的分页扩展章节。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
