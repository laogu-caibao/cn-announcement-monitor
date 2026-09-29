# A股财报与公告盯梢

`laogu-announcements`

A股财报与公告盯梢 skill：维护关注清单，定期抓取新公告并输出中文摘要推送；支持定期报告、业绩预告、分红、定增、减持等类型分级处理。

## 一键安装

```bash
npx skills add laogu-caibao/laogu-announcements
```

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
- 扣子 Coze：扣子编程 → 技能面板 → 创建技能 → 本地上传，上传本仓库打包的 zip（仓库根目录已有 SKILL.md，直接压缩仓库文件夹即可）；如页面要求 `.skill` 后缀，由扣子导入后自动生成，不要只改扩展名。
- Trae：设置 → 技能 → 上传技能，上传同上 zip；或手动放到 `~/.trae/skills/laogu-announcements/`（项目级用 `.trae/skills/laogu-announcements/`）。Trae 也支持 MCP：把 `uvx laogu-mcp` 配进 MCP 设置即可获得 16 个工具（skill 负责流程指导、MCP 负责工具调用）。
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
## English

**laogu-announcements — Filing watcher.** Maintains a watchlist of A-share companies, fetches new announcements (periodic reports, previews, dividends, placements, stake reductions) and pushes Chinese summaries. Install: `npx skills add laogu-caibao/laogu-announcements`.

## FAQ

**Q：laogu-announcements 有什么用？**
适合的场景：盯一批公司的公告，有新披露（定期报告、预告、分红、定增、减持）时自动抓取并出中文摘要。

**Q：数据可靠吗？会荐股吗？**
数字必须来自可核验的公开来源（上市公司公告、交易所公开数据、公开网页），取不到就标「未核验」，绝不编造；只做结构化整理与解读，不构成投资建议。

**Q：怎么安装？支持哪些 AI 平台？**
```bash
npx skills add laogu-caibao/laogu-announcements
```
平台中立 Markdown，Claude Code、Codex、豆包智能体、Workbuddy、扣子 Coze、Trae 等环境均可用；数据能力可用 [laogu-mcp](https://github.com/laogu-caibao/laogu-mcp)（`uvx laogu-mcp`）一次装齐。更多 skill 见[老谷拆财报组织主页](https://github.com/laogu-caibao)。
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

