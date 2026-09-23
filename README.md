# METRIC 官方網站

靜態網站，共四頁，無建置步驟：推送至 GitHub 並啟用 Pages 即完成發佈。

上線網址：<https://mlmicocake-beep.github.io/metric-website/>

| 檔案 | 頁面 |
|---|---|
| `index.html` | 產品 |
| `guide.html` | 部署指南 |
| `download.html` | 下載 |
| `about.html` | 關於 |
| `404.html` | 找不到頁面 |
| `style.css` | 全站樣式（唯一一份） |
| `brand/` | 標誌與 favicon |
| `img/` | 產品畫面 |

## 本機預覽

```bash
python -m http.server 8080 --directory .
```

## GitHub Pages

Settings → Pages → Source 選擇 `main` 分支、根目錄。

專案站位於 `/<repo>/` 路徑下，而非網域根目錄，因此頁面一律使用**相對路徑**。
`404.html` 例外：它須回應任意深度的錯誤網址，因此以 `<base>` 將相對路徑固定於站台根目錄。
遷移至自有網域根目錄時，須修改該行。

## 配色

色票取自產品桌面應用程式的主題定義，並非另行為網站挑選：

| | 淺色 | 深色 |
|---|---|---|
| 背景 | `#ffffff` | `#0d1117` |
| 文字 | `#1f2328` | `#e6edf3` |
| 次要文字 | `#656d76` | `#7d8590` |
| 細線 | `#d0d7de` | `#30363d` |
| 主色 | `#0053fd` | `#4a84fe` |

淺色底沒有官方的次要文字色。需偏離上表時使用 `#5C6572`，細線使用 `#DBDFE4`。

## 排版原則

1. **Flat, not boxed**：不在卡片內再套卡片，同一區塊內不加分隔框；分組僅以留白與單一細線區隔。
2. **文字按鈕不使用圓角**，尺寸由 padding × line-height 決定，不設固定高度；僅圖示按鈕使用 4px。
3. **Tokens, not literals**：顏色一律使用 CSS 變數。

## 標誌規範

以下四條出自標誌規範，並非風格偏好：

1. **紅色僅用於標誌本身。** 連結、按鈕與強調一律不使用紅色，強調色使用 Metric 藍。
2. **不得加入漸層、陰影或發光效果。** 整份 CSS 不含任何 `box-shadow` 或 `gradient`。
3. **波形不得修改，亦不得拉直或對稱化。** 一律使用 `brand/` 內的原始 SVG，不得重繪。
4. **符號周圍留白至少等於波形高度的 1/4。**

標誌分為深色底與淺色底兩個檔案，以 `<picture>` 依 `prefers-color-scheme` 切換。
若只放其中一個，另一種色彩模式下標誌將無法顯示。

## 替換圖片前

`img/` 為產品實機畫面。**替換前須確認畫面不含網路位址、序號與帳號名稱**，
不得直接上傳原始擷圖。

資訊密集的 UI 畫面須滿版呈現（`.wide` ＋ 獨立一行），不得置入兩欄版塊：實測寬度僅 458px，
表格文字無法辨識。

三張圖皆設定 `width`／`height` 屬性，讓瀏覽器預留版位，避免載入時版面位移；
首圖不使用 lazy 載入，並設定 `fetchpriority="high"`。

## 下載頁運作方式

`download.html` 於瀏覽器端即時讀取 GitHub Releases API，**發佈新版時無須修改此頁**。
讀取失敗時改為顯示「前往發行頁面」，不會呈現空白。

發行 repo：`mlmicocake-beep/metric-releases`

頁面不顯示 SHA-256：`github.com` 的 release 下載端點不回傳
`Access-Control-Allow-Origin`，瀏覽器以 fetch 讀取附件必定遭 CORS 阻擋
（`api.github.com` 會回傳 `*`，因此可列出版本，但無法讀取附件內容）。
需要核對時，點選「簽章清單」直接開啟該檔。
