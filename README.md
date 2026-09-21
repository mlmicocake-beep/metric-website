# METRIC 官方網站

靜態站，四頁，沒有建置步驟 —— 推上 GitHub、開 Pages 就是網站。

| 檔案 | 頁 |
|---|---|
| `index.html` | 介紹 |
| `guide.html` | 如何使用 |
| `download.html` | 下載 |
| `about.html` | 關於我 |
| `404.html` | 找不到頁面 |
| `style.css` | 全站樣式（唯一一份） |
| `brand/` | 標誌與 favicon |

## 上線前待辦

1. ✅ **`about.html` 的聯絡方式** —— 2026-09-21 改成指向發行 repo 的 GitHub Issues。
   確定正式對外窗口（信箱或表單）之後換掉那一段。
2. ✅ **`download.html` 的 `REPO`** —— `mlmicocake-beep/metric-releases`，與發行站一致。
3. ⬜ **Portal 的 `_GITHUB_REPO_DEFAULT`** —— 目前**刻意留空**（空字串），因為指向不存在
   的 repo 會讓 `_gh_ready` 誤放行再撞 404。發行 repo 公開、確認 API 讀得到之後，
   才把它填成 `mlmicocake-beep/metric-releases`。

## 開 GitHub Pages

Settings → Pages → Source 選 `main` 分支、根目錄。網址會是
`https://<帳號>.github.io/<repo>/`。

## 風格來源：Agent 桌面 GUI

**色票與字體不是自己挑的**，直接取自產品的主題定義
`apps/desktop/src/themes/presets.ts` 的 `nousTheme`
（label「Metric」、描述「GitHub chrome, Metric blue accent」）：

| | 淺色 | 深色 |
|---|---|---|
| 背景 | `#ffffff` | `#0d1117` |
| 文字 | `#1f2328` | `#e6edf3` |
| 次要文字 | `#656d76` | `#7d8590` |
| 細線 | `#d0d7de` | `#30363d` |
| 主色 | `#0053fd` | `#4a84fe` |

排版規則取自 `apps/desktop/DESIGN.md`，三條直接決定這份 CSS 長什麼樣：

1. **Flat, not boxed** —— 不要卡片套卡片、不要在同一塊裡加分隔框；分組只用留白
   與單一細線。所以這一站沒有「一堆有框圓角卡片」。
2. **文字按鈕不做圓角**，尺寸由 padding × line-height 決定、不給固定高度。
   只有圖示按鈕用共用的 4px 圓角。
3. **Tokens, not literals** —— 顏色一律走 CSS 變數。

改版前先看那兩份檔案；產品改了 token，這裡要跟著改。

## 設計約束（改版前先看）

四條來自標誌規範，不是風格偏好：

1. 🔴 **紅色只用於標誌本身。** 品牌規範寫的是「紅色僅用於需要人處理的事項」，
   所以站上的連結、按鈕、強調**一律不用紅** —— 強調色用產品自己的 Metric 藍。
   把紅拿去當強調色是這條最常被違反的地方。
2. **不得加漸層、陰影、發光。** 整份 CSS 沒有一個 `box-shadow` 或 `gradient`。
3. **波形不得修改、不得拉直對稱化。** 標誌一律用 `brand/` 裡的原始 SVG，不要重畫。
4. **符號周圍留白至少等於波形高度的 1/4。**

另外兩個踩過的點：

- 標誌有**深色底與淺色底兩個檔**，用 `<picture>` 依 `prefers-color-scheme` 切換。
  只放深色底那份的話，在淺色模式下整個標誌會看不見。
- 淺色底**沒有官方的次要文字色**。品牌文件指定用 `#5C6572`，細線用 `#DBDFE4` ——
  深色底的 `#8A94A3` 放在淺底對比不足。

## 圖片素材

`img/` 的三張是**產品實機截圖**，來自 `Desktop/METRIC-簡報/screens/`
（那批進簡報前已經把 IP、閘道、序號馬賽克過）。

| 檔 | 用途 |
|---|---|
| `app-home.png` | 首圖：乾淨的產品主畫面 |
| `app-work.png` | 「寫程式、執行、自己修」：實際寫 Python、跑起來、依結果修正 |
| `app-report.png` | 「交出來的東西可以直接看」：讀網路設備後的結構化報告 |

🔴 **換圖前先確認遮蔽。** 原圖不可以直接放上來 —— 位址、序號、登入帳號、
「啟用 Windows」水印都要處理掉。

⚠️ **密集的 UI 截圖要滿版**（`.wide` ＋ 獨立一行），不要塞進兩欄版塊。
實測放在 `.split` 裡只有 458px 寬，表格文字完全看不清楚，那種圖等於沒放。

三張都帶 `width`／`height` 屬性讓瀏覽器先留位，載入時版面不會跳；首圖不 lazy
並帶 `fetchpriority="high"`。

## 數字口徑

技能寫「50+」、工具寫「23」。理由：目錄裡的技能總數（195）與畫面顯示的數字對不上，
而 `/api/skills` 會因設定檔而異 —— 對外用下限講法才不會在客戶面前被問倒。

## 本機預覽

`.claude/launch.json` 有 `metric-site`（埠 8933）。或直接：

```bash
python -m http.server 8933 --directory metric-website
```
