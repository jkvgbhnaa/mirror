# Yidianmeii External Resource Aggregator

Yidianmeii External Resource Aggregator 是一个面向内容采集、数据分析与信息归档场景的轻量化外链资源汇总系统。该项目定位于技术内容运营团队、个人站长与信息分析人员，用于集中管理来自多个子域名与路径结构的散落外部文章链接，并提供统一的访问入口、元数据提取与状态监控能力。

本项目不提供内容存储或数据库写入功能，专注于外链资源的规范化整理、可访问性检查与基础分类输出，可作为更大规模数据处理管道的前端采集环节使用。

## 功能概览

- **多子域名源聚合**：支持对 yidianmeii.cn 域名下 3g、h5、wap 等常见移动端子域名进行统一资源收录与归类。
- **链接结构解析**：自动识别 /snews/ 路径模式，提取资源 ID 与类型标识，便于下游系统进行增量更新。
- **基础可访问性探测**：提供轻量级 HTTP 状态检查，标记异常链接并生成报告。
- **分类标签生成**：依据 URL 中的数字 ID 段与子域名特征，自动生成内容类型推测标签。
- **批量导入与导出**：支持通过文本文件批量导入链接列表，并导出为结构化 JSON 或 CSV 格式。
- **过滤与去重**：内置简单规则过滤器，可排除无效或重复条目，降低下游处理噪声。
- **纯静态部署支持**：项目核心逻辑以 Python 脚本实现，可无状态运行于容器或云函数环境。

## 应用场景

- **内容聚合站点的外链巡检**：运营人员每日使用本工具扫描指定域名下的文章链接列表，快速发现响应异常的页面，以便及时调整内容引用策略。
- **数据分析管道的前置清洗**：数据工程师将本工具集成至 ETL 流程中，作为 URL 规范化与过滤环节，确保后续分析任务输入数据的质量。
- **个人知识库的资源归档**：研究员或博客作者利用本工具整理收藏的深度文章链接，生成带访问状态的资源清单，便于长期维护与查阅。
- **SEO 外链审计辅助**：SEO 分析师定期运行本工具对历史发布内容中的外链进行存活检测，识别失效链接并生成替换建议。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/your-org/yidianmeii-aggregator.git

# 进入项目目录
cd yidianmeii-aggregator

# 安装依赖（建议使用虚拟环境）
pip install -r requirements.txt

# 运行聚合器，对默认资源列表执行扫描
python aggregator.py --input ./data/urls.txt --output ./reports/status.json

# 如需仅解析并分类，不执行网络请求
python aggregator.py --input ./data/urls.txt --mode parse --output ./reports/parsed.json
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心运行环境，建议使用 3.10 长期支持版本 |
| requests | 2.28.0 及以上 | 用于发送 HTTP 请求，检测链接可访问性 |
| urllib3 | 1.26.0 及以上 | requests 依赖，需注意与 OpenSSL 兼容性 |
| python-dotenv | 1.0.0 及以上 | 用于加载环境变量，管理超时与重试配置 |
| pytest | 7.0.0 及以上 | 仅开发测试需要，生产环境可不安装 |
| flake8 | 6.0.0 及以上 | 代码风格检查工具，非运行时必需 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/quickstart.md | 如何最短时间运行聚合器并得到第一份输出报告？ |
| 配置参考 | docs/configuration.md | 支持哪些环境变量与命令行参数？如何调整超时和重试策略？ |
| 输出格式 | docs/output-format.md | 生成的 JSON 与 CSV 报告包含哪些字段，各字段含义是什么？ |
| 扩展开发 | docs/development.md | 如何增加新的子域名解析规则或自定义过滤逻辑？ |
| 常见问题 | docs/faq.md | 遇到 SSL 错误、连接超时或编码问题时应如何处理？ |

## 资源列表

- http://3g.yidianmeii.cn/snews/3153344.shtml
- http://h5.yidianmeii.cn/snews/4421.shtml
- http://h5.yidianmeii.cn/snews/3759330.shtml
- http://wap.yidianmeii.cn/snews/7504355.shtml
- http://h5.yidianmeii.cn/snews/66332.shtml
- http://3g.yidianmeii.cn/snews/3257650.shtml
- http://3g.yidianmeii.cn/snews/3711.shtml
- http://h5.yidianmeii.cn/snews/4944.shtml
- http://wap.yidianmeii.cn/snews/69731.shtml
- http://3g.yidianmeii.cn/snews/26280.shtml

## 项目结构

```
yidianmeii-aggregator/
├── aggregator.py            # 主入口脚本，解析参数并调度各模块
├── requirements.txt         # 生产环境依赖列表
├── .env.example             # 环境变量模板（超时、重试、日志级别）
├── core/
│   ├── __init__.py          # 核心包初始化
│   ├── fetcher.py           # HTTP 请求封装，含重试与超时控制
│   ├── parser.py            # URL 解析与子域名、ID 提取逻辑
│   └── reporter.py          # 结果输出：JSON / CSV / 控制台表格
├── filters/
│   ├── __init__.py          # 过滤器包初始化
│   ├── dedup.py             # 基于 URL 和 ID 的去重器
│   └── allowlist.py         # 可配置的域名与路径白名单
├── tests/
│   ├── test_fetcher.py      # 网络请求模块的单元测试
│   ├── test_parser.py       # 解析逻辑的边界用例测试
│   └── fixtures/            # 测试用的静态 URL 样本文件
├── data/
│   ├── urls.txt             # 默认输入文件，可由用户替换
│   └── sample_output.json   # 示例输出，用于验证格式
└── docs/
    ├── quickstart.md        # 快速入门文档
    ├── configuration.md     # 完整配置说明
    └── development.md       # 开发与贡献指南
```

## 贡献指南

1. 查阅 `docs/development.md` 文档了解项目架构与代码风格约定，确保新增代码符合 flake8 规范并通过现有单元测试。
2. 在 `tests/` 目录下为新增功能或修复内容添加对应的测试用例，保证测试覆盖率达到 80% 以上。
3. 提交拉取请求前，请在本地运行 `pytest tests/` 确保全部测试通过，并执行 `python aggregator.py --input data/urls.txt --output /tmp/report.json` 验证基础功能无回归。
4. 提交信息请遵循语义化格式（如 `feat: 增加对 wap 子域名的重定向跟踪` 或 `fix: 修复解析器对空 ID 的处理异常`），并附上简要说明。
5. 拉取请求需至少一名项目维护者审核，审核通过后由维护者合并至主分支。

## 常见问题

**Q: 运行扫描时遇到 SSL 证书验证错误，该如何处理？**

A: 部分环境下 Python 的 urllib3 可能无法验证某些证书链。您可以通过设置环境变量 `YIDIANMEII_SSL_VERIFY=false` 临时关闭证书验证（不推荐在生产环境使用），或更新系统 CA 证书包。更安全的做法是使用 `--timeout` 参数增加等待时间，并检查网络代理设置。

**Q: 输入文件中的链接数量很大，如何避免被目标服务器限流？**

A: 您可以通过环境变量 `YIDIANMEII_REQUEST_DELAY` 设置请求间隔（单位秒），默认值为 0.5 秒。此外，`YIDIANMEII_MAX_RETRIES` 变量控制重试次数，建议在批量扫描时适当增大延迟并减少并发数，以降低对源站的压力。

**Q: 输出报告中的 status 字段为 -1 表示什么？**

A: status 字段为 -1 表示该链接在最大重试次数内未获得任何 HTTP 响应，通常原因包括 DNS 解析失败、连接超时或目标主机不可达。请检查网络连通性，或调整 `YIDIANMEII_TIMEOUT` 环境变量（默认 10 秒）增加等待时长。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
