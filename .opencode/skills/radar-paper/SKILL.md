---
name: radar-paper
description: 论文与技术快读（最近24h）。只收真实论文、benchmark、工程 release 或明确技术更新。
---

执行方式：
- 直接执行完整工作流并输出最终 Markdown。
- 禁止输出确认问题、模板、执行过程、占位符。
- 必须先调用 MCP 工具，再输出结论。
- 若没有真实技术发布，明确输出“今日无高价值技术发布”，不要做方向级脑补。

来源规则：
- 使用 `config/source_registry.yaml` 作为来源级别基准。
- `[C/]` / `[D/]` 优先，`[E/]` 可补背景，`[M/]` 仅作线索。
- 没有论文、benchmark、工程 release、开发者公告支撑时，不得把普通行业新闻包装成技术快读。

硬信号标准：
- 论文发布、benchmark 更新、开源 release、模型 / 推理 / 多模态 / 芯片 / agent 的明确技术更新。

工作流：
1) `resolve_date_range`：expression="最近24小时" -> RANGE
2) `get_latest_news`：limit=100, include_url=true
3) `aggregate_news`：date_range=RANGE, limit=40
4) `get_trending_topics`：top_n=12, mode="daily"
5) 只筛 1~3 个真实技术方向；如果不足，则少于 3 个，允许为 0
6) 对每个方向按需调用：
   - `analyze_topic_trend(topic, date_range=RANGE)`
7) 输出快读

输出要求：
- 每个方向必须有：`来源`、`事实`、`可能贡献`、`边界与不确定性`、`可迁移启示`、`待验证清单`。
- `来源` 保留原始源名或域名，并尽量保留 `[C/]` / `[D/]` / `[E/]` / `[M/]` 标签。
- 若信息不足，只能降低结论强度，不能虚构论文细节。

输出格式（Markdown）：
# 论文与技术快读（最近24h）
## 方向1：<技术方向>
- 来源：
- 事实（3~6条）：
- 可能贡献（使用“可能 / 倾向”措辞）：
- 边界与不确定性（至少3条）：
- 可迁移启示：
- 待验证清单：

兜底格式：
# 论文与技术快读（最近24h）
今日无高价值技术发布。缺少论文、benchmark、工程 release 或开发者公告级别的证据，不输出方向级脑补。
