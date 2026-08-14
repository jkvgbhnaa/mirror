# YidianMeta-News-Collector

YidianMeta-News-Collector 是一个面向中文移动端新闻资源聚合与结构化提取的开源工具集。该项目定位于为新闻数据分析师、自然语言处理研究人员以及内容聚合平台开发者提供稳定、可扩展的新闻外链采集与元数据规范化处理能力。项目核心价值在于将分散在多个移动端子域名下的新闻条目通过统一的解析管道进行标准化输出，降低从非结构化 HTML 页面中提取标题、发布时间、正文摘要及来源字段的工程成本。

本项目不提供新闻内容存储服务，亦不涉及任何形式的版权内容分发，仅作为公开页面结构化工具使用。目标用户包括学术机构文本挖掘团队、个人博客作者进行热点素材整理，以及小型内容平台进行合规外链引用管理。

## 功能概览

- **多子域名源统一适配**：内置针对 yidianmeii.cn 域名下 wap、3g、h5 三种移动端前缀的页面结构适配器，自动识别不同终端模板的 DOM 差异。
- **批量链接队列处理**：支持基于文件或标准输入导入待处理 URL 列表，采用生产者-消费者模型并发执行请求，默认并发度为 5。
- **结构化元数据输出**：每条新闻输出为 JSON 格式，包含 title、publish_timestamp、plain_text_preview、source_domain、raw_url 及采集时间戳。
- **可配置请求头伪装**：支持自定义 User-Agent、Referer 及 Cookie，以模拟真实移动设备访问行为，降低被反爬策略拦截的风险。
- **增量去重存储**：基于 URL 哈希的本地 KV 存储记录已处理链接，避免重复抓取，适合定时增量任务。
- **异常重试与超时控制**：每个请求支持最多 3 次重试，单次请求超时阈值可配置（默认 10 秒），失败链接单独写入错误日志。
- **Docker 一键部署**：提供预构建 Docker 镜像，无需安装系统级依赖即可在 Linux 服务器或本地开发环境运行。
- **Prometheus 指标暴露**：集成 /metrics 端点，可接入监控系统观察任务总数、成功数、失败数及平均响应耗时。

## 应用场景

**学术研究中的新闻语料采集**：语言学或传播学研究人员可使用本工具快速抓取指定时间段内特定域名下的新闻页面，构建用于文本倾向性分析或关键词演化跟踪的小型语料库。

**个人博客或公众号的热点摘要生成**：独立内容创作者每日运行一次批量采集任务，将获取到的新闻标题和摘要存入本地数据库，辅助撰写每周热点回顾类文章，同时通过保留原始 URL 满足引用规范。

**企业内部竞品信息监控**：中小型创业公司市场部门可将本工具集成至内部数据看板，定时拉取特定分类下的新闻链接，经过简单规则过滤后推送至企业微信群或钉钉机器人。

**NLP 模型测试集构建**：算法工程师可从采集到的新闻正文中随机抽样，人工标注后作为文本分类或命名实体识别模型的评估数据，本工具提供的纯文本预览字段降低了数据清洗工作量。

## 快速开始

以下命令演示了从克隆代码到执行一次示例采集任务的完整流程。

```bash
# 克隆项目仓库
git clone https://github.com/your-org/yidianmeta-news-collector.git
cd yidianmeta-news-collector

# 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 使用内置示例链接文件运行采集任务
python run_collector.py --input samples/example_urls.txt --output data/result.json --concurrency 3
```

如需使用 Docker 运行，可执行以下命令：

```bash
docker pull your-registry/yidianmeta-collector:latest
docker run -v $(pwd)/data:/app/data yidianmeta-collector:latest --input /app/data/my_links.txt
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心运行环境，建议使用 3.10 以获得更好的性能 |
| requests | 2.28.0 及以上 | 处理 HTTP 请求及会话管理 |
| beautifulsoup4 | 4.11.0 及以上 | 解析 HTML 文档树，定位新闻正文节点 |
| lxml | 4.9.0 及以上 | 作为 beautifulsoup4 的解析器后端，提供更高的解析效率 |
| redis | 5.0 及以上（可选） | 若启用分布式去重功能，需要 Redis 服务；单机模式下使用内置 sqlite 替代 |
| docker | 20.10 及以上（可选） | 仅当使用容器化部署时所需 |
| prometheus-client | 0.16.0 及以上（可选） | 如需开启监控指标暴露功能则必须安装 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/quick_start.md | 如何快速配置运行环境并执行一次采集任务？采集结果存储在哪里？ |
| 配置手册 | docs/configuration.md | 如何修改请求超时时间、重试次数、并发度以及日志级别？如何自定义请求头？ |
| 适配器开发 | docs/adapter_development.md | 当前支持哪些域名模板？如何为新的新闻网站编写自定义适配器？ |
| 部署运维 | docs/deployment.md | 如何将本工具部署为常驻后台服务？如何接入 Prometheus 监控？ |

## 资源列表

- http://wap.yidianmeii.cn/jnews/0814/3613885.shtml
- http://3g.yidianmeii.cn/jnews/0814/61807.shtml
- http://h5.yidianmeii.cn/jnews/0814/0795669.shtml
- http://h5.yidianmeii.cn/jnews/0814/78376.shtml
- http://3g.yidianmeii.cn/jnews/0814/077175.shtml
- http://3g.yidianmeii.cn/jnews/0814/1076616.shtml
- http://3g.yidianmeii.cn/jnews/0814/8361.shtml
- http://3g.yidianmeii.cn/jnews/0814/361763.shtml
- http://3g.yidianmeii.cn/jnews/0814/00308.shtml
- http://wap.yidianmeii.cn/jnews/0814/8510660.shtml

## 项目结构

```
yidianmeta-news-collector/
├── collector/                           # 主逻辑包
│   ├── __init__.py
│   ├── runner.py                        # 任务调度入口，管理并发与队列
│   ├── fetcher.py                       # 封装 requests 会话，实现重试及超时控制
│   ├── parser.py                        # 调用适配器解析页面，返回统一结构体
│   └── storage.py                       # 增量去重 KV 存储接口，支持 sqlite 和 redis 后端
├── adapters/                            # 针对不同子域名的页面适配器
│   ├── base.py                          # 定义抽象适配器基类，含 parse() 方法契约
│   ├── wap_adapter.py                   # 处理 wap.yidianmeii.cn 下的页面结构
│   ├── h5_adapter.py                    # 处理 h5.yidianmeii.cn 下的页面结构
│   └── mobile3g_adapter.py              # 处理 3g.yidianmeii.cn 下的页面结构
├── cli/                                 # 命令行参数解析与入口脚本
│   ├── __init__.py
│   └── main.py                          # 解析 sys.argv，构建配置对象并启动 runner
├── tests/                               # 单元测试与集成测试
│   ├── test_fetcher.py                  # 模拟 HTTP 响应测试重试逻辑
│   ├── test_adapters.py                 # 使用本地 HTML 样本验证各适配器解析准确率
│   └── fixtures/                        # 存放各个子域名返回的静态 HTML 样本文件
├── docker/                              # 容器化构建相关
│   ├── Dockerfile                       # 多阶段构建，基于 alpine 减小镜像体积
│   └── entrypoint.sh                    # 容器启动时设置环境变量并转发命令
├── docs/                                # 完整文档目录，涵盖配置、部署与开发指南
├── samples/                             # 示例输入输出
│   ├── example_urls.txt                 # 包含 10 条示例链接的纯文本文件
│   └── expected_output.json             # 与示例链接对应的预期 JSON 输出参考
├── requirements.txt                     # 生产环境 Python 依赖锁定
├── requirements-dev.txt                 # 额外包含 pytest、black 等开发工具
├── setup.py                             # 项目安装脚本，定义入口点 console_scripts
├── .env.example                         # 环境变量配置模板，含 LOG_LEVEL 和 REDIS_URL
└── README.md                            # 项目主文档（当前文件）
```

## 贡献指南

我们欢迎社区开发者提交适配器增强、性能优化及文档改进类贡献。请遵循以下流程：

1.  **查阅现有议题**：在提交新功能请求或缺陷修复前，请先浏览 GitHub Issues 列表，确认没有重复议题。若为新需求，请先创建一个议题描述您的改进方案，等待维护者确认方向再动手编码。

2.  **派生仓库并创建特性分支**：将主仓库派生至个人账号下，然后基于 `main` 分支创建以 `feature/` 或 `fix/` 为前缀的新分支，避免直接在 main 分支上修改。

3.  **编写单元测试与适配器回归用例**：所有新增的适配器逻辑必须附带对应的单元测试；修改现有解析规则时，请在 `tests/fixtures` 下更新对应子域名的静态样本，确保测试通过率不低于 95%。

4.  **提交前执行代码规范检查**：使用 `black` 和 `isort` 格式化代码，使用 `pylint` 检查静态错误。提交信息需遵循常规提交规范，首行概括变更内容，正文说明动机与影响范围。

5.  **发起拉取请求**：向主仓库的 `main` 分支发起 PR，描述中关联相关议题编号，并附上本地测试结果截图或日志。维护者将在 48 小时内进行 Review，通过后合并。

## 常见问题

**问：采集过程中出现大量 403 或 521 状态码，如何解决？**

答：首先检查网络环境是否能正常访问目标域名。若可访问，请尝试更新 `config.yaml` 中的 `user_agent` 字段为最新版本的 Chrome 或 Safari 移动端 User-Agent 字符串。部分域名会校验 `Referer` 头，建议将其设置为 `https://www.yidianmeii.cn/` 或域名根地址。若问题依旧，可适当降低并发度并增加单次请求间隔（`delay_seconds` 配置项）。

**问：采集到的正文预览字段包含大量 HTML 标签或 JavaScript 片段，如何获得纯净文本？**

答：默认使用的 `lxml` 解析器会去除 `<script>` 和 `<style>` 标签内容，但部分页面会通过 CSS 伪元素或异步加载插入正文。建议在适配器开发中针对特定域名重写 `_extract_content()` 方法，使用 `re` 正则进一步清洗空白字符，并可通过配置 `keep_paragraph_breaks` 控制是否保留段落换行符。若页面完全由客户端渲染，则需更换为 Selenium 或 Playwright 后端，本项目暂不提供内置支持。

**问：项目是否支持分布式横向扩展？**

答：基础版本仅支持单机多线程模式。若需分布式部署，可将 `storage.py` 中的后端切换为 Redis，并确保所有工作节点连接同一 Redis 实例以共享去重键。任务分发可使用 Celery 或简单 RPC 进行编排，相关方案参见 `docs/deployment.md` 中的高可用附录章节。

## 许可证

MIT License

Copyright (c) 2026 YidianMeta-News-Collector Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
