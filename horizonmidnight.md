# Yidianmeii Content Aggregator

Yidianmeii Content Aggregator 是一个面向中文互联网内容聚合与导航的开源工具集，专注于对 yidianmeii.cn 域名下分散的新闻与资讯页面进行结构化整理、分类索引与快速访问。该项目主要服务于内容研究者、信息收集人员以及对特定领域新闻动态有持续跟踪需求的个人或团队，通过提供统一的资源清单与本地化的阅读辅助能力，降低在多级子域名与大量数字编号页面中反复定位的成本。

该项目本身不存储或转发任何内容，仅提供对外部页面的引用与归类能力，所有原始内容仍由源站点提供服务。项目定位为技术性资源整理工具，强调可复现性、可维护性与扩展性，适合作为数据采集流程中的初始入口层或人工阅读索引的补充方案。

## 功能概览

- 资源清单自动生成：根据预定义的 URL 列表自动生成结构化的资源索引页面，便于人工审阅与批量处理。

- 多子域名归类支持：自动识别 h5、3g、wap 等不同子域名前缀，并按域名分组展示，方便观察内容发布渠道的分布规律。

- 数字标识提取与排序：从每条 URL 中提取末尾的数字编号（如 295888），支持按编号升序或降序排列，辅助追溯内容发布时间或逻辑顺序。

- 简单的本地预览服务：内置基于 Python HTTP 服务器的零配置预览模式，一键启动后可在浏览器中查看生成的索引页面。

- 外部链接校验提示：提供对每条 URL 的可访问性基础校验能力，输出 HTTP 状态码与响应时间，辅助判断链接有效性。

- 导出为多种格式：支持将整理后的资源列表导出为纯文本、Markdown 表格或 CSV 文件，方便导入其他数据处理工具。

- 配置文件热加载：资源列表与归类规则独立存储为 YAML 配置文件，修改后无需重启服务即可生效。

## 应用场景

内容研究团队定期采集新闻页面样本时，可使用本项目的资源清单作为任务队列，按子域名或编号范围分配给不同成员，避免重复访问与遗漏。

个人用户希望在浏览器书签之外建立一份轻量级、可版本控制的阅读清单时，可将本项目作为本地索引工具，每次更新资源列表后通过 Git 记录变更历史。

数据分析师需要对特定域名的内容发布频率或路径结构进行统计时，可利用项目提供的 URL 解析与排序功能，快速生成基础统计报表，减少手动整理时间。

运维或测试人员需要批量验证一批旧链接的可用性时，可借助项目内置的校验模块，一次性输出所有链接的状态结果，并标记异常项。

## 快速开始

以下步骤适用于 Linux、macOS 及 Windows WSL 环境，需提前安装 Git 与 Python 3.8 或更高版本。

```bash
# 克隆项目仓库
git clone https://github.com/yidianmeii/yidianmeii-aggregator.git
cd yidianmeii-aggregator

# 安装依赖（仅使用标准库，无需额外包）
# 若需校验功能，可安装 requests 与 colorama
pip install requests colorama --user

# 运行本地预览服务
python -m http.server 8000 --directory ./output
```

启动后，在浏览器中访问 http://localhost:8000 即可查看当前生成的索引页面。如需重新生成资源清单，请执行：

```bash
python scripts/generate_index.py --config config/resources.yaml --output output/index.html
```

## 安装要求

| 依赖项 | 必需 | 说明 |
|--------|------|------|
| Python 3.8+ | 是 | 运行核心脚本与预览服务的基础解释器 |
| Git 2.20+ | 是 | 克隆仓库与管理版本变更 |
| requests 2.25+ | 否 | 用于链接校验模块，若未安装则跳过校验 |
| colorama 0.4+ | 否 | 用于终端彩色输出，若未安装则使用普通文本 |
| yaml 5.3+ | 否 | 用于解析配置文件，若未安装则使用 JSON 后备方案 |
| 浏览器 | 否 | 仅用于查看生成的 HTML 索引页面，非强制 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门 | docs/quickstart.md | 如何快速生成第一份资源索引并启动预览？ |
| 配置 | docs/configuration.md | 资源列表的 YAML 格式如何编写？归类规则如何定义？ |
| 开发 | docs/development.md | 如何扩展新的导出格式或自定义排序逻辑？ |
| 运维 | docs/operations.md | 如何定时更新资源列表？如何集成到 CI 流水线？ |

## 资源列表

- http://h5.yidianmeii.cn/snews/295888.shtml
- http://3g.yidianmeii.cn/snews/1892.shtml
- http://3g.yidianmeii.cn/snews/2362.shtml
- http://3g.yidianmeii.cn/snews/9167292.shtml
- http://h5.yidianmeii.cn/snews/366858.shtml
- http://wap.yidianmeii.cn/snews/9111636.shtml
- http://3g.yidianmeii.cn/snews/8068.shtml
- http://wap.yidianmeii.cn/snews/115539.shtml
- http://wap.yidianmeii.cn/snews/8321733.shtml
- http://3g.yidianmeii.cn/snews/4640.shtml

## 项目结构

```
yidianmeii-aggregator/
├── config/                                 # 配置文件目录
│   └── resources.yaml                      # 主资源列表，含 URL 与分类标签
├── scripts/                                # 可执行脚本目录
│   ├── generate_index.py                   # 从配置生成 HTML 索引页
│   ├── validate_links.py                   # 校验链接可用性并输出报告
│   └── export_csv.py                       # 将资源列表导出为 CSV 格式
├── output/                                 # 生成输出目录（默认）
│   ├── index.html                          # 生成的索引页面
│   └── links_report.csv                    # 导出的 CSV 报告（如有）
├── tests/                                  # 单元测试目录
│   ├── test_parser.py                      # 测试 URL 解析与编号提取
│   └── test_generator.py                   # 测试索引生成逻辑
├── docs/                                   # 文档目录
│   ├── quickstart.md                       # 快速入门指南
│   ├── configuration.md                    # 配置文件详细说明
│   ├── development.md                      # 开发与扩展指南
│   └── operations.md                       # 运维与自动化说明
├── .gitignore                              # Git 忽略规则
├── LICENSE                                 # MIT 许可证
└── README.md                               # 本文件
```

## 贡献指南

1. 在 GitHub 上 Fork 本仓库，并克隆到本地开发环境。请确保使用 main 分支作为基线。

2. 创建新的功能分支，分支命名格式为 feature/简短描述 或 fix/问题编号，例如 feature/sort-by-time。

3. 提交代码前请运行现有单元测试，确保无回归问题。新增功能需附带相应的测试用例。

4. 更新资源列表时，请编辑 config/resources.yaml 文件，并按照已有格式添加或删除条目。提交信息中需注明变更原因。

5. 提交 Pull Request 到主仓库的 main 分支，并在描述中说明变更内容、测试结果以及任何可能的影响范围。

## 常见问题

问：项目是否存储或缓存原始页面内容？

答：否。本项目仅维护 URL 列表和生成索引页面，不存储任何原始页面内容、文本、图片或其它媒体文件。所有内容访问均直接跳转至源站点。

问：资源列表中的链接失效了怎么办？

答：项目本身不保证外部链接的长期可用性。用户可自行运行 validate_links.py 脚本检查链接状态，并根据输出结果手动更新 resources.yaml 文件。

问：是否支持添加自定义分类或标签？

答：支持。在 config/resources.yaml 中，每个 URL 条目下方可增加 tags 字段，用于自定义标签，生成索引时会自动按标签分组展示。

问：能否将项目用于商业用途？

答：可以。本项目采用 MIT 许可证，允许自由使用、修改、分发，包括商业用途，但需保留原始版权声明。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
