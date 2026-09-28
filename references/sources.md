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
- 原文链接：巨潮资讯 `http://www.cninfo.com.cn/new/disclosure/detail?stockCode={代码}&announcementId={art_code}`（栏目不同时用标题在巨潮搜索定位）。

## 备选数据源（主接口失败时）

1. 深交所公告接口：`POST http://www.szse.cn/api/disc/announcement/annList`
   body: `{"stock":[["002466"]],"seDate":["{开始}~{结束}"],"channelCode":["fixed_disc"],"pageSize":20,"pageNum":1}`
2. 巨潮资讯历史公告查询：`POST http://www.cninfo.com.cn/new/hisAnnouncement/query`
   form: `stock=002466&tabName=fulltext&pageSize=20&pageNum=1&column=szse&category=&plate=&seDate=&searchkey=`
   （`column` 取 `szse`/`sse`/`bj` 按交易所）
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

`watermark` 记录已处理的最新的 `art_code`（或 `display_time`）；每次运行先读文件，按水位过滤后再写回。
