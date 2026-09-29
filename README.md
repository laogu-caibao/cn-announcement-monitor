# A股财报与公告盯梢

`laogu-announcements`

A股财报与公告盯梢 skill：维护关注清单，定期抓取新公告并输出中文摘要推送；支持定期报告、业绩预告、分红、定增、减持等类型分级处理。

## 一键安装

仓库地址（点击复制）：

`https://github.com/laogu-caibao/laogu-announcements`

**方式一：克隆**

```bash
git clone https://github.com/laogu-caibao/laogu-announcements.git
```

**方式二：下载 ZIP**

https://github.com/laogu-caibao/laogu-announcements/archive/refs/heads/main.zip

**导入使用**

- Claude Code / Muse：把仓库中的 `SKILL.md` 放到 `~/.claude/skills/laogu-announcements/` 下即可调用。
- 豆包智能体 / Workbuddy 等：按各平台的 skill 上传流程导入 `SKILL.md`。
- 一次装好全部 16 个：用 [laogu-mcp](https://github.com/laogu-caibao/laogu-mcp)，`uvx laogu-mcp` 一键安装。
## 文件结构

- `SKILL.md` — skill 主流程（平台中立，可导入豆包智能体 / Workbuddy 等支持定时任务的环境）
- `references/sources.md` — 公告数据源、公告类型分级规则、`watchlist.json` 示例与水位机制

## 使用

1. 按 `references/sources.md` 示例建立 `watchlist.json`（公司名称、代码、关注类型）。
2. 按 `SKILL.md` 的 Workflow 定期运行：查新公告 → 按水位过滤 → 分级摘要 → 中文推送 → 更新水位。

## 说明

- 只盯清单内公司；摘要严格基于公告原文
- 回复一律中文；不做买卖推荐

---
## 出品

**老谷拆财报** —— 以数据为刃，剖市场真相

- 抖音 / 微信视频号 / 今日头条 / 快手：搜索「老谷拆财报」
- 固定栏目：「价值投资之财报解读」（全网连载中）
- 本 skill 的方法论与账号内容同源：数据驱动、拆开看、不讲黑话

### 扫码关注

| 微信视频号 | 抖音 |
|---|---|
| ![视频号二维码](docs/qrcode-shipinhao.jpg) | ![抖音二维码](docs/qrcode-douyin.png) |
| 扫一扫，关注视频号 | 抖音号：gubaobao22 |

> 作者声明：个人观点，仅供参考，不构成投资建议。

