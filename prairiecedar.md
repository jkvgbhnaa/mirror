# NewsLink Aggregator

NewsLink Aggregator 是一个面向技术内容聚合与新闻外链管理的开源工具集，专注于为开发者、内容运营者及自动化采集系统提供稳定、可扩展的新闻链接整理与发布方案。该项目并不直接生产新闻内容，而是通过标准化的外链收集与分类机制，帮助用户将分散在不同移动端子域名下的新闻条目进行统一归档、元数据提取与结构化输出，适用于构建私有新闻聚合服务、舆情监控系统或内容二次分发管道。

目标用户包括：需要批量管理新闻外链的运维工程师、从事内容聚合站点的开源开发者、以及希望自动化获取特定时段新闻索引的数据分析人员。NewsLink Aggregator 以轻量级脚本为核心，兼容主流 Unix/Linux 环境，支持定时任务驱动，可无缝嵌入现有的 ETL 流程或 CI/CD 工作流。

## 功能概览

- **多子域名外链采集**：自动识别并处理来自 wap、3g、h5 等移动端子域名下的新闻链接，无需人工区分来源。
- **批次化链接管理**：内置批次编号机制，当前为第 27/90 批，支持按批次索引、清洗和导出链接清单。
- **原始地址严格保真**：对所有收录的外链执行零改写策略，确保协议、域名、路径、查询参数及文件名完全保留原始值。
- **资源状态检测**：提供可选的 HTTP 状态码检查模块，用于快速筛选可访问或失效的新闻页面。
- **元数据提取模板**：支持从 URL 路径中解析日期（如 /0814/）、疑似新闻 ID 及文件扩展名，便于后续分类存储。
- **多格式输出**：内置 JSON、CSV 和纯文本列表三种导出格式，适应不同下游系统的数据消费需求。
- **增量更新支持**：通过记录已处理链接的哈希值，避免重复采集同一资源，降低源站请求压力。

## 应用场景

**私有新闻聚合站点构建**  
个人开发者或小团队可使用 NewsLink Aggregator 定期抓取指定域名下的新闻链接，结合前端模板生成统一的移动端新闻看板，无需依赖第三方 RSS 服务。

**舆情监控与内容归档**  
企业舆情部门可将本工具集成到数据采集管道中，按批次（如每日）收集指定时间段内的新闻外链，作为后续内容分析或报表生成的基础数据源。

**自动化测试中的链接校验**  
QA 工程师可利用本工具提取测试环境中的新闻外链清单，配合状态检测功能批量验证页面可访问性，及时发现 404 或 5xx 异常。

**数据迁移前的资源盘点**  
在更换 CMS 或重构新闻系统时，运维人员可通过本工具导出历史新闻外链的完整列表，用于对比新旧系统的资源覆盖情况。

## 快速开始

以下步骤适用于 Linux/macOS 及 Windows WSL 环境，确保系统已安装 Git 和 Python 3.8+。

```bash
# 克隆项目仓库
git clone https://github.com/your-org/newslink-aggregator.git
cd newslink-aggregator

# 安装依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 运行采集脚本，指定批次号与输出格式
python collect.py --batch 27 --format json --output ./data/batch_27.json
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 或更高 | 核心运行环境，推荐 3.10+ |
| pip | 20.0+ | Python 包管理工具 |
| requests | 2.25.0+ | 用于 HTTP 请求与状态检测 |
| beautifulsoup4 | 4.9.0+ | 可选依赖，用于解析 HTML 元数据 |
| lxml | 4.6.0+ | 解析器后端，提升 beautifulsoup 性能 |
| pytest | 6.0.0+ | 开发测试框架（仅开发环境需要） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/usage.md | 如何配置采集参数、调整输出格式、处理异常链接？ |
| 开发指南 | docs/development.md | 项目代码结构、自定义解析器扩展方法、提交 PR 的规范是什么？ |
| 运维参考 | docs/operations.md | 如何设置定时任务、管理日志文件、备份历史批次数据？ |
| API 参考 | docs/api_reference.md | 各模块函数签名、参数类型、返回值及异常定义有哪些？ |

## 资源列表

- http://wap.yidianmeii.cn/jnews/0814/6983.shtml
- http://3g.yidianmeii.cn/jnews/0814/0722.shtml
- http://h5.yidianmeii.cn/jnews/0814/7812255.shtml
- http://3g.yidianmeii.cn/jnews/0814/7741.shtml
- http://wap.yidianmeii.cn/jnews/0814/1164004.shtml
- http://3g.yidianmeii.cn/jnews/0814/83900.shtml
- http://h5.yidianmeii.cn/jnews/0814/6680444.shtml
- http://3g.yidianmeii.cn/jnews/0814/5161461.shtml
- http://3g.yidianmeii.cn/jnews/0814/0114069.shtml
- http://wap.yidianmeii.cn/jnews/0814/9411089.shtml

## 项目结构

```
newslink-aggregator/
├── collect.py                 # 主入口脚本，解析命令行参数并调度采集流程
├── config.yaml                # 配置文件，包含默认批次、超时时间、重试策略等
├── requirements.txt           # 生产环境依赖清单
├── src/                       # 核心源码目录
│   ├── fetcher/               # 网络请求与响应处理模块
│   │   ├── client.py          # 封装 requests 会话，支持代理与重试
│   │   └── parser.py          # 原始响应内容预处理器
│   ├── linker/                # 链接管理模块
│   │   ├── extractor.py       # 从配置或输入源提取 URL 列表
│   │   ├── validator.py       # 校验 URL 合法性及域名白名单
│   │   └── deduplicator.py    # 基于哈希的重复链接过滤
│   ├── exporter/              # 输出格式化模块
│   │   ├── json_exporter.py   # 导出为 JSON 数组
│   │   ├── csv_exporter.py    # 导出为 CSV 表格
│   │   └── text_exporter.py   # 导出为纯文本每行一个链接
│   ├── monitor/               # 状态检测与日志模块
│   │   ├── checker.py         # 并发 HEAD 请求检测链接可用性
│   │   └── logger.py          # 结构化日志记录器
│   └── utils/                 # 通用工具函数
│       ├── time_utils.py      # 日期解析与格式化
│       └── file_utils.py      # 文件读写辅助
├── tests/                     # 单元测试与集成测试目录
│   ├── test_fetcher.py
│   ├── test_linker.py
│   └── test_exporter.py
├── data/                      # 默认输出目录，存放各批次导出文件
│   └── samples/               # 示例输出文件供参考
├── docs/                      # 完整文档目录（参见文档导航）
└── LICENSE                    # MIT 许可证文件
```

## 贡献指南

1. 阅读开发指南文档 docs/development.md 了解项目设计理念和代码规范。
2. 在 GitHub Issues 中查找或创建待解决的问题，并等待维护者确认。
3. Fork 本仓库，在本地新建功能分支（如 feature/add-ftp-exporter），进行代码修改。
4. 确保新增或修改的代码包含对应的单元测试，并通过全部测试用例（pytest）。
5. 提交 Pull Request，并在描述中清晰说明改动内容、关联 issue 编号及测试结果。

## 常见问题

**Q: 采集过程中遇到 SSL 证书验证失败怎么办？**  
A: 本工具默认所有新闻链接均为 HTTP 协议，不涉及 SSL 证书问题。若您自行添加了 HTTPS 源，可在 config.yaml 中将 verify_ssl 设为 false（不推荐生产环境使用），或更新系统 CA 证书包。

**Q: 如何处理链接中的中文编码或特殊字符？**  
A: 工具内部使用 Python 的 urllib.parse 进行 URL 标准化，保留原始路径和查询字符串的百分比编码格式。如需解码显示，可使用 exporter 中的可选参数 decode_url 控制。

**Q: 能否支持动态加载更多新闻链接（如滚动加载）？**  
A: 当前版本仅处理静态 HTML 中直接暴露的链接。对于 JavaScript 动态渲染的内容，建议配合 Playwright 或 Selenium 驱动，并将获取到的完整链接列表通过本工具的文本导入功能进行处理。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
