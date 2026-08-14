# Yidian News Resource Aggregator

Yidian News Resource Aggregator is a lightweight, developer-oriented information aggregation tool designed to systematically collect, normalize, and present news resources from the Yidian platform. This project targets developers, data analysts, and content researchers who require structured access to news articles published on specific dates. It addresses the fundamental challenge of manually tracking scattered news URLs across different subdomains by providing a centralized, version-controlled index that can be integrated into automated workflows, data pipelines, or content monitoring systems. The project does not host or republish news content but serves as a reliable external resource index with strict adherence to original URL preservation.

## 功能概览

- **多子域名覆盖** - 自动识别并整合来自 3g.yidianmeii.cn、wap.yidianmeii.cn 和 h5.yidianmeii.cn 的新闻链接，确保所有移动端资源入口被统一收录。
- **日期批次索引** - 以日期批次（如 0814）为维度组织资源，方便按发布时间进行过滤和检索，满足时效性分析需求。
- **原始 URL 严格保真** - 所有链接保持原始协议（HTTP）、域名格式和路径结构，不进行任何自动补全或规范化改写，确保数据可追溯性。
- **Markdown 原生呈现** - 资源列表以纯 Markdown 格式输出，无需额外解析器即可在代码仓库、文档站点或静态页面中直接渲染。
- **轻量级无依赖** - 项目本身不依赖任何第三方库或运行时环境，仅需标准文本编辑器即可维护和扩展资源列表。
- **可编程接口友好** - 资源列表采用每行单个 URL 的格式，便于使用 grep、awk、sed 等命令行工具进行批处理操作。
- **版本控制集成** - 借助 Git 进行资源变更追踪，每次新增或删除 URL 均有提交记录，支持回溯和历史比对。
- **跨平台兼容** - 无论 Windows、Linux 还是 macOS 系统，均可无障碍使用本项目进行资源查阅和管理。

## 应用场景

- **新闻内容聚合分析** - 研究人员或数据分析师可通过本项目获取指定日期的新闻文章列表，批量提取页面元数据或文本内容，进行话题热度、关键词分布等分析。
- **自动化监控与告警** - 运维或开发人员可将项目集成到定时任务中，每次运行检查资源列表中的 URL 是否可访问，当发现链接失效或返回异常状态码时触发告警。
- **内容归档与备份计划** - 信息管理人员利用项目提供的索引作为归档清单，配合 wget 或 curl 工具批量下载新闻页面，构建离线备份库。
- **SEO 外链质量审核** - SEO 专员可使用资源列表快速核验外链来源域名的多样性和协议一致性，评估外链建设策略的执行情况。
- **数据管道入口构建** - 数据工程师可将本项目作为 ETL 流程的起始节点，从资源列表中读取 URL 并传递给爬虫或 API 请求模块，实现自动化数据采集。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/your-org/yidian-news-aggregator.git

# 进入项目目录
cd yidian-news-aggregator

# 查看资源列表（无需安装，直接预览）
cat resources/index.md

# 如需验证所有 URL 的可访问性（使用 curl 循环检测）
while read url; do
  curl -o /dev/null -s -w "%{http_code} %{url_effective}\n" "$url"
done < resources/index.md

# 统计当前资源总数
grep -c "^http" resources/index.md
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Git | 2.0 或更高 | 用于克隆仓库和提交更新 |
| 文本编辑器 | 任意版本 | 查看或编辑资源列表（如 Vim、VS Code、Notepad++） |
| curl | 7.0 或更高 | 可选，用于 URL 可达性测试 |
| wget | 1.0 或更高 | 可选，用于批量下载新闻页面 |
| grep | 3.0 或更高 | 可选，用于命令行过滤和统计 |
| awk | 任意 POSIX 版本 | 可选，用于文本处理脚本 |
| sed | 任意 POSIX 版本 | 可选，用于流式替换操作 |
| bash | 4.0 或更高 | 可选，用于运行提供的示例脚本 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 资源索引 | /resources/index.md | 当前收录的所有新闻 URL 完整列表在哪查看？ |
| 批次说明 | /batches/2026-08-14/README.md | 该批次资源的来源日期、收录标准和补充说明是什么？ |
| 验证脚本 | /scripts/check-urls.sh | 如何快速验证资源列表中的 URL 是否全部有效？ |
| 变更日志 | /CHANGELOG.md | 每次更新新增或移除了哪些资源？历史记录如何追溯？ |
| 贡献指引 | /CONTRIBUTING.md | 我想提交新的新闻 URL 或修正现有链接，具体流程是什么？ |
| 项目规划 | /ROADMAP.md | 项目未来的扩展方向（如下一批次计划、自动化集成）是什么？ |

## 资源列表

- http://3g.yidianmeii.cn/jnews/0814/179193.shtml
- http://wap.yidianmeii.cn/jnews/0814/606815.shtml
- http://wap.yidianmeii.cn/jnews/0814/8166.shtml
- http://h5.yidianmeii.cn/jnews/0814/665387.shtml
- http://wap.yidianmeii.cn/jnews/0814/5817.shtml
- http://3g.yidianmeii.cn/jnews/0814/861655.shtml
- http://wap.yidianmeii.cn/jnews/0814/7968731.shtml
- http://3g.yidianmeii.cn/jnews/0814/0696.shtml
- http://h5.yidianmeii.cn/jnews/0814/8180.shtml
- http://h5.yidianmeii.cn/jnews/0814/5853.shtml

## 项目结构

```
yidian-news-aggregator/
├── README.md                     # 项目总览与入门指南
├── CHANGELOG.md                  # 版本更新记录
├── CONTRIBUTING.md               # 贡献者操作手册
├── ROADMAP.md                    # 未来开发路线图
├── LICENSE                       # MIT 许可证文件
├── resources/                    # 所有资源索引文件存放目录
│   └── index.md                  # 主资源列表（含全部 URL）
├── batches/                      # 按日期分组的批次说明目录
│   └── 2026-08-14/               # 具体批次文件夹（以收录日期命名）
│       ├── README.md             # 该批次的元信息说明
│       └── sources.txt           # 该批次的原始链接备份
├── scripts/                      # 辅助工具脚本目录
│   ├── check-urls.sh             # URL 可达性检测脚本
│   ├── count-stats.sh            # 统计资源数量和域名分布
│   └── export-csv.sh             # 将资源列表导出为 CSV 格式
└── tests/                        # 测试用例目录
    ├── format-validator.test.sh  # 检查 URL 格式是否符合规范
    └── duplicate-finder.test.sh  # 检测是否存在重复链接
```

## 贡献指南

1. 复刻（Fork）本项目仓库到个人账号下，并在本地克隆复刻后的副本。请确保本地 Git 配置了正确的用户名和邮箱，以便提交记录可追溯。

2. 在 resources/index.md 文件末尾追加新的新闻 URL，或修改现有条目。每次仅提交一个明确的变更目的，例如仅新增、仅删除或仅修正某个 URL，避免混合操作。

3. 运行测试脚本验证资源格式合规性。执行 `bash tests/format-validator.test.sh` 检查所有 URL 是否以 `http://` 开头且不包含多余空白字符，同时运行 `bash tests/duplicate-finder.test.sh` 确保无重复条目。

4. 提交变更并编写清晰的提交信息（Commit Message），格式建议为 `[类型] 简短描述`，例如 `[add] 追加 0815 批次资源` 或 `[fix] 修正 179193 号链接路径错误`。提交后推送到远程复刻仓库。

5. 通过 Web 界面发起拉取请求（Pull Request），在描述中说明变更原因、涉及 URL 数量以及是否经过自测。项目维护者将在 2 个工作日内审核，审核通过后合并入主分支。

## 常见问题

**Q: 为什么资源列表中有些 URL 使用 HTTP 协议而不是 HTTPS？我能否自行修改为 HTTPS？**

A: 项目明确规定所有 URL 必须保持用户提供的原始协议和域名形式。这是为了保证数据来源的完全可追溯性，避免因协议升级或域名变更导致的访问差异。如果您发现某个 HTTP 链接已支持 HTTPS，请在贡献时同时保留原始 HTTP 链接，并在备注中说明 HTTPS 可用情况，但切勿直接替换原 URL。

**Q: 如果某个资源链接失效（返回 404 或其他错误），应该如何处理？**

A: 首先使用 curl -I 确认该 URL 的当前响应状态。如果确认永久失效，请在贡献流程中提交删除请求，并在提交信息中注明失效原因。若链接仅临时不可达但内容仍存在，建议等待 24 小时后重试，不要立即删除，以免误判。

**Q: 项目是否支持自动从 Yidian 平台抓取最新新闻列表？**

A: 当前版本为手动维护模式，旨在保证资源索引的精确性和可控性。自动抓取功能已列入 ROADMAP.md 的远期规划，届时会提供可选的自动化脚本，但默认仍以人工审核为核心策略，确保每条链接均经过验证。

## 许可证

MIT

> 外链数量: 10 | 生成时间: 2026-08-14 21:24:15
