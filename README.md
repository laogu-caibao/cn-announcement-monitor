# cn-announcement-monitor

A股财报与公告盯梢 skill：维护关注清单，定期抓取新公告并输出中文摘要推送；支持定期报告、业绩预告、分红、定增、减持等类型分级处理。

## 文件结构

- `SKILL.md` — skill 主流程（平台中立，可导入豆包智能体 / Workbuddy 等支持定时任务的环境）
- `references/sources.md` — 公告数据源、公告类型分级规则、`watchlist.json` 示例与水位机制

## 使用

1. 按 `references/sources.md` 示例建立 `watchlist.json`（公司名称、代码、关注类型）。
2. 按 `SKILL.md` 的 Workflow 定期运行：查新公告 → 按水位过滤 → 分级摘要 → 中文推送 → 更新水位。

## 说明

- 只盯清单内公司；摘要严格基于公告原文
- 回复一律中文；不做买卖推荐
