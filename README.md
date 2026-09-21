# METRIC 官方網站

靜態站，四頁，沒有建置步驟 —— 推上 GitHub、開 Pages 就是網站。

上線網址：<https://mlmicocake-beep.github.io/metric-website/>

| 檔案 | 頁 |
|---|---|
| `index.html` | 介紹 |
| `guide.html` | 如何使用 |
| `download.html` | 下載 |
| `about.html` | 關於我 |
| `404.html` | 找不到頁面 |
| `style.css` | 全站樣式（唯一一份） |
| `brand/` | 標誌與 favicon |
| `img/` | 產品畫面 |

## 本機預覽

```bash
python -m http.server 8080 --directory .
```

## GitHub Pages

Settings → Pages → Source 選 `main` 分支、根目錄。

專案站掛在 `/<repo>/` 底下而不是網域根目錄，所以頁面一律用**相對路徑**。
`404.html` 是例外 —— 它會被拿來回應任何深度的錯誤網址，所以用 `<base>` 把
相對路徑釘在站根。搬到自有網域的根目錄時要改那一行。

## 配色

色票取自產品桌面應用程式的主題定義，不是為網站另外挑的：

| | 淺色 | 深色 |
|---|---|---|
| 背景 | `#ffffff` | `#0d1117` |
| 文字 | `#1f2328` | `#e6edf3` |
| 次要文字 | `#656d76` | `#7d8590` |
| 細線 | `#d0d7de` | `#30363d` |
| 主色 | `#0053fd` | `#4a84fe` |

淺色底沒有官方的次要文字色。要脫離上表時用 `#5C6572`，細線用 `#DBDFE4`。

## 排版三條

1. **Flat, not boxed** —— 不要卡片套卡片、不要在同一塊裡加分隔框；分組只用留白與單一細線。
2. **文字按鈕不做圓角**，尺寸由 padding × line-height 決定、不給固定高度。只有圖示按鈕用 4px。
3. **Tokens, not literals** —— 顏色一律走 CSS 變數。

## 標誌規範

四條來自標誌規範，不是風格偏好：

1. **紅色只用於標誌本身。** 連結、按鈕、強調一律不用紅 —— 強調色用 Metric 藍。
2. **不得加漸層、陰影、發光。** 整份 CSS 沒有一個 `box-shadow` 或 `gradient`。
3. **波形不得修改、不得拉直對稱化。** 一律用 `brand/` 裡的原始 SVG，不要重畫。
4. **符號周圍留白至少等於波形高度的 1/4。**

標誌有深色底與淺色底兩個檔，用 `<picture>` 依 `prefers-color-scheme` 切換。
只放其中一份的話，在另一種模式下整個標誌會看不見。

## 換圖之前

`img/` 是產品實機畫面。**換圖前先確認畫面上沒有網路位址、序號、帳號名稱**，
原始擷圖不要直接放上來。

密集的 UI 畫面要滿版（`.wide` ＋ 獨立一行），不要塞進兩欄版塊 —— 實測只有 458px 寬，
表格文字完全看不清楚，那種圖等於沒放。

三張都帶 `width`／`height` 屬性讓瀏覽器先留位，載入時版面不會跳；首圖不 lazy
並帶 `fetchpriority="high"`。

## 下載頁怎麼運作

`download.html` 在瀏覽器端即時讀 GitHub Releases API，**發新版時這一頁完全不用改**。
讀不到時退回「前往發行頁面」，不會變成一片空白。

發行 repo：`mlmicocake-beep/metric-releases`

SHA-256 不在頁面上顯示 —— release 附件的網址會轉到 `objects.githubusercontent.com`，
該處不送 `Access-Control-Allow-Origin`，瀏覽器 fetch 一定被 CORS 擋掉。要核對的人
按「簽章清單」直接開那個檔。
