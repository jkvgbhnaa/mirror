# HrefCollector

HrefCollector 是一个面向技术文档团队与开源项目维护者的外链资产盘点与结构化汇总工具。该项目并非通用爬虫或搜索引擎，而是专注于将分散在多个页面、批次、域名下的原始 URL 清单，转换为可维护、可审计、可版本控制的 Markdown 结构化文档。目标用户包括技术写作人员、开发者关系工程师、合规审计师以及开源社区贡献者，他们需要定期对项目外部引用进行梳理、去重、分类与状态标记。

HrefCollector 的核心工作流基于源数据批次管理：用户将一批原始 URL（如某次内容发布涉及的所有外链）输入系统，工具自动执行协议归一化检查、域名归属识别、状态码探测（可选），并按照预设模板生成包含功能概览、应用场景、安装要求、文档导航、项目结构等完整章节的 README 风格报告。与通用链接管理工具不同，HrefCollector 强调“原样保留”与“可追溯性”，即所有原始 URL 在输出文档中严格保持用户提供的字符串形式，不主动添加协议头、不补全域名、不进行跳转解析，从而确保审计链条的原始性。

## 功能概览

**批次化数据导入**：支持按批次编号（如第 80/90 批）将大量原始 URL 一次性导入系统，工具自动生成批次元数据记录。

**结构化文档生成**：基于内置的 Markdown 模板引擎，将原始 URL 列表自动填充至资源列表章节，并同步生成项目简介、功能概览、应用场景等配套内容。

**原样输出保证**：所有用户给定的 URL 在输出文档中一字不差保留原始字符串形式，不进行任何协议转换、域名补全或路径改写。

**依赖环境检测**：在安装要求章节以表格形式列出所有必需的系统依赖、版本要求与说明，帮助用户快速评估部署环境。

**ASCII 目录树可视化**：自动生成项目根目录的 ASCII 风格目录树，并为每个子目录添加功能注释，便于新贡献者理解代码组织。

**文档导航映射**：构建层面、目录与对应问题之间的三维映射表格，帮助用户根据自身角色快速定位相关文档模块。

## 应用场景

**技术文档外部引用审计**：技术写作团队在发布新版本文档前，使用 HrefCollector 对文档中所有外链进行盘点，生成引用清单，便于合规部门审查链接指向的第三方内容是否仍符合政策要求。

**开源项目 README 维护**：开源项目维护者在定期更新 README 时，使用 HrefCollector 将社区贡献的参考链接、教程资源、相关项目等大量 URL 整理成规范的资源列表章节，避免手动排版错误。

**内容迁移前后链接对比**：当网站域名或路径结构发生变更时，运维人员利用 HrefCollector 导出旧版所有外链清单，迁移后再导入新版环境进行对比，确保无链接遗漏或错误改写。

**多批次外链状态监控**：运营团队每月将当月所有对外发布的资讯、公告、活动页面中的外链汇总为一批次，通过 HrefCollector 生成月度外链报告，并与上一批次进行差异对比，快速发现新增或失效引用。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/your-organization/href-collector.git

# 进入项目目录
cd href-collector

# 安装依赖（使用 pip 针对 Python 版本，或使用 npm 针对 Node.js 版本，根据实际技术栈调整）
pip install -r requirements.txt

# 运行导入脚本，将原始 URL 清单（raw_urls.txt）导入并生成完整 README 文档
python scripts/generate_readme.py --input data/raw_urls.txt --output README.md --batch "80/90"
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心脚本运行环境，用于模板渲染与文件操作 |
| Git | 2.25 及以上 | 用于克隆仓库及版本管理 |
| pip | 20.0 及以上 | Python 包管理工具，用于安装第三方依赖 |
| Markdown 解析库 | markdown-it-py 2.0+ | 用于生成文档时进行 Markdown 语法校验 |
| 网络请求库 | requests 2.25+ | 可选依赖，用于启用 URL 状态探测功能（默认关闭） |
| 操作系统 | Linux/macOS/Windows | 跨平台支持，路径处理已适配主流系统 |
| 终端环境 | 支持 UTF-8 | 确保 ASCII 目录树及特殊字符正常显示 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户入门 | docs/quick-start.md | 如何快速安装并生成第一份外链报告？ |
| 功能配置 | docs/configuration.md | 如何调整模板样式、批次编号格式和输出章节顺序？ |
| 数据规范 | docs/data-format.md | 原始 URL 清单文件应遵循什么格式？如何处理带空格或特殊字符的链接？ |
| 贡献开发 | CONTRIBUTING.md | 如何提交代码改进、新增模板变量或报告 Bug？ |

## 资源列表

- http://h5.yidianmeii.cn/jnews/0814/3116917.shtml
- http://wap.yidianmeii.cn/jnews/0814/598666.shtml
- http://wap.yidianmeii.cn/jnews/0814/916169.shtml
- http://wap.yidianmeii.cn/jnews/0814/61013.shtml
- http://wap.yidianmeii.cn/jnews/0814/1051.shtml
- http://3g.yidianmeii.cn/jnews/0814/3153366.shtml
- http://h5.yidianmeii.cn/jnews/0814/6611.shtml
- http://h5.yidianmeii.cn/jnews/0814/3759330.shtml
- http://wap.yidianmeii.cn/jnews/0814/7506355.shtml
- http://h5.yidianmeii.cn/jnews/0814/66331.shtml

## 项目结构

```
href-collector/
├── README.md                     # 项目主文档，由工具自动生成
├── CONTRIBUTING.md               # 贡献指南，含分支策略与提交规范
├── LICENSE                       # MIT 许可证文件
├── requirements.txt              # Python 依赖清单，含版本锁定
├── data/                         # 原始数据与批次归档目录
│   ├── raw/                      # 存放用户提供的原始 URL 清单文件
│   │   └── batch_80_90.txt       # 第 80/90 批次原始数据示例
│   └── archive/                  # 历史批次生成文档归档
│       └── 2026/                 # 按年份分类存档
├── scripts/                      # 核心脚本目录
│   ├── generate_readme.py        # 主生成脚本，读取原始数据并渲染 README
│   ├── template_engine.py        # 模板渲染引擎，包含章节顺序控制
│   └── validator.py              # URL 格式校验与重复检测模块
├── templates/                    # Markdown 模板目录
│   ├── base.md.j2                # 基础模板，含所有章节占位符
│   └── sections/                 # 各章节独立子模板，便于定制
│       ├── features.md.j2
│       ├── scenarios.md.j2
│       └── resources.md.j2
├── tests/                        # 单元测试与集成测试目录
│   ├── test_validator.py         # 针对 URL 校验逻辑的测试用例
│   └── test_generate.py          # 针对完整生成流程的测试
└── docs/                         # 用户文档目录
    ├── quick-start.md
    ├── configuration.md
    └── data-format.md
```

## 贡献指南

1. 在 GitHub 仓库中 Fork 本项目，并在本地克隆您的 Fork 副本，然后创建新的功能分支（命名格式为 feature/您的功能名称 或 fix/问题描述）。

2. 对代码或文档进行修改后，请确保运行 tests 目录下的所有测试用例通过，并新增对应测试覆盖您提交的变更。

3. 提交 commit 时，请使用语义化提交信息格式（如 docs: update resource list template 或 fix: correct URL validator regex）。

4. 向本仓库的 main 分支发起 Pull Request，并在 PR 描述中清晰说明变更目的、涉及章节及测试结果，等待维护者审阅。

5. 若发现 Issue 或存在改进建议，请在 Issues 页面新建问题，并附上复现步骤或具体建议内容。

## 常见问题

**问：为什么生成的资源列表中，URL 没有自动补全 http:// 或 https:// 协议头？**

答：HrefCollector 的设计原则之一是“原样保留”，即完全忠实于用户输入的原始字符串。这一决策旨在满足审计场景下对原始证据链的严格要求，避免工具自动添加或修改任何字符导致与原始记录不一致。若用户需要统一协议，请在导入前自行预处理原始清单文件。

**问：如何新增自定义章节或调整章节顺序？**

答：您可以通过修改 templates/base.md.j2 模板文件来增加或删除章节占位符，并同步更新 scripts/template_engine.py 中的章节顺序列表。调整后请运行测试确保生成流程正常。更详细的定制说明请参考 docs/configuration.md 文档。

**问：URL 状态探测功能默认关闭，如何启用？**

答：在生成脚本中，将 --enable-check 参数设为 true 即可启用状态码探测。启用后工具会尝试对每个 URL 发起 HEAD 请求，并在资源列表中以注释形式标注响应状态（如 200、404、超时等）。请注意该功能会显著增加运行时间，且依赖网络环境稳定，建议在测试环境或小批次数据上先行试用。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
