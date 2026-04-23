# 為野生動物而走聯盟 官方網站

Taiwan Wildlife Parade — 為野生動物而走聯盟官方網站原始碼。

本站為靜態網站，透過 GitHub Pages 部署於 <https://walkforwildlife.org.tw>。

## 頁面

- `index.html` — 首頁
- `news.html` — 訊息專區（從 `news.json` 讀取最新消息）
- `member.html` — 組織架構
- `about.html` — 組織章程
- `join.html` — 加入我們
- `donate.html` — 捐款支持
- `documents.html` — 相關文獻

## 其他檔案

- `banner.jpg` — 活動主視覺
- `CNAME` — GitHub Pages 自訂網域設定
- `news.json` — 訊息專區資料來源
- `robots.txt` / `sitemap.xml` — 搜尋引擎索引設定
- `source/` — 原始文件存檔（章程修訂檔等）

## 本機預覽

在專案根目錄執行任一靜態檔案伺服器即可：

```sh
python3 -m http.server 8000
```

然後開啟 <http://localhost:8000>。

## 聯絡

- 📞 0918-832539
- ✉️ <taiwanwalkforwildlife@gmail.com>
- 📍 臺北市文山區汀州路4段88號理學院大樓C202室

## License

[MIT](LICENSE)
