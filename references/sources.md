# 公告数据源与水位机制

## 主数据源：东方财富公告接口（实测可用）

```
Referer: https://data.eastmoney.com/
GET https://np-anotice-stock.eastmoney.com/api/security/ann?sr=-1&page_size=20&page_index=1&ann_type=A&client_source=web&stock_list={纯数字代码}
```

- `stock_list` 必须用纯数字代码（`002466`），不带 `.SZ`/`.SH` 后缀；多家用逗号分隔。
- 返回 `data.list[]` 字段：
  - `art_code`：公告唯一 ID（如 `AN202609021828931752`），用作去重/水位键
  - `title`：公告标题
  - `display_time`：发布时间（`2026-09-02 18:40:16:375`）
  - `columns[].column_name`：栏目（"定期报告" / "其他" 等）
- 原文链接（人工阅读）：东财公告正文页 `https://data.eastmoney.com/notices/detail/{纯数字代码}/{art_code}.html`。
  例：`https://data.eastmoney.com/notices/detail/002466/AN202609021828931752.html`
- 正文抓取接口（机器读取，2026-09-29 实测可用）：**不要直接抓上面页面的 HTML**（那是 JS 空壳，纯服务端抓不到正文），用内容接口：
```
Referer: https://data.eastmoney.com/
GET https://np-cnotice-stock.eastmoney.com/api/content/ann?art_code={art_code}&client_source=web&page_index=1
```
  - 返回 JSON：`data.notice_content`（HTML 富文本，需 strip 标签后再提炼数字）、`data.page_size`（>1 时循环 `page_index` 逐页抓全，定期报告类正文分页，不翻页会漏掉财务数据章节）、`data.attach_list[].attach_url`（PDF 备选，正文抓不到时下载 PDF）
  - 抓取成功判定：`success=1` 且 `notice_content` 非空；否则视为失败，执行降级。
- 备选：巨潮资讯 `http://www.cninfo.com.cn/new/disclosure/detail`（其 announcementId 是巨潮自有纯数字 ID，与东财 `art_code` 不是同一体系，不可直接拼接；仅作"用标题在巨潮搜索定位"的备选说明）。

## 备选数据源（主接口失败时）

1. 深交所公告接口：`POST http://www.szse.cn/api/disc/announcement/annList`
   body: `{"stock":[["002466"]],"seDate":["{开始}~{结束}"],"channelCode":["fixed_disc"],"pageSize":20,"pageNum":1}`
   注意：2026-09-29 实测纯服务端 POST 被 WAF 拦截（返回 50x 错误页），需浏览器环境或完整请求头。
2. 巨潮资讯历史公告查询：`POST http://www.cninfo.com.cn/new/hisAnnouncement/query`
   form: `stock=002466&tabName=fulltext&pageSize=20&pageNum=1&column=szse&category=&plate=&seDate=&searchkey=`
   （`column` 取 `szse`/`sse`/`bj` 按交易所）
   注意：2026-09-29 实测按此参数返回 0 条（`totalRecordNum=0`），疑似参数格式已变更，**暂标注"待验证"，恢复前勿依赖**。
3. 上交所：`http://www.sse.com.cn/disclosure/listedinfo/regular/`（页面读取）

## 公告类型分级（按标题关键词）

- 定期报告：标题含"年度报告"/"半年度报告"/"季度报告"
- 业绩预告/快报：标题含"业绩预告"/"业绩快报"
- 分红：标题含"权益分派"/"分红派息实施"
- 定增/发债：标题含"非公开发行"/"向特定对象发行"/"可转换公司债券"
- 减持：标题含"减持计划"/"减持股份"
- 回购：标题含"回购"
- 重组/收购：标题含"重大资产重组"/"收购"
- 其他：一句话归纳

## watchlist.json 示例

```json
{
  "updated_at": "2026-09-29T01:00:00+08:00",
  "companies": [
    {"name": "天齐锂业", "code": "002466", "watermark": "AN202609021828931752", "added": "2026-09-29"}
  ]
}
```

`watermark` 记录已处理的最新的 `art_code`（AN+日期前缀，字典序即时间序；全 skill 统一用 `art_code` 作水位键，不用 `display_time`）；每次运行先读文件，按水位过滤后再写回。
