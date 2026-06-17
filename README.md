# 2026 APEC · AI-Driven Design Innovation — 一頁式形象官網

> 2026 APEC 透過 AI 技術驅動設計創新國際研討會 · 一頁式活動官網
> 主辦：財團法人台灣設計研究院（TDRI）｜時間：2026.09.16–17｜地點：台灣設計研究院 創意劇場

純前端、零依賴、零建置流程（no build step）的單檔網站。直接用瀏覽器打開 `index.html` 即可，也可直接丟到任何靜態主機部署。

---

## 1. 檔案結構

```
.
├── index.html      # 完整網站（HTML + CSS + JS 全部內嵌，單檔即可運作）
└── README.md       # 本文件
```

> 目前刻意採「單檔」設計，方便攜帶與部署。若要拆成 `index.html` / `css/style.css` / `js/main.js` 三檔以利維護，跟我說一聲即可拆分。

---

## 2. 本地預覽 / 部署

**本地預覽**

```bash
# 方法 A：直接用瀏覽器打開 index.html

# 方法 B：起一個本地伺服器（建議，地圖 iframe 在 http 下行為較正常）
python3 -m http.server 8080
# 開啟 http://localhost:8080
```

**部署**：無需編譯，丟上任一靜態主機即可

- Vercel / Netlify：拖曳資料夾或連 Git repo，framework 選 `Other`，build command 留空，output 設為根目錄
- GitHub Pages：push 後於 Settings → Pages 指定分支根目錄
- Cloudflare Pages / 自架 Nginx：同理，指向含 `index.html` 的資料夾

> 預計網址：`2026apec-design.com`（依簡報需求，網址不含 TW）。

---

## 3. 設計系統 Design Tokens

所有顏色集中在 `index.html` 內 `<style>` 最上方的 `:root`，改色只要動這裡。

| Token | 值 | 用途 |
| --- | --- | --- |
| `--ink` | `#05080F` | 主背景（深空藍黑） |
| `--ink-2` | `#0A1120` | 面板 / hover 底色 |
| `--ink-3` | `#0E1A2D` | 次層底色 |
| `--blue` | `#4F7BFF` | 強調色・電藍 |
| `--cyan` | `#37E2D8` | 主色・青 |
| `--green` | `#54F0A8` | 強調色・春綠 |
| `--glow` | `linear-gradient(100deg,#4F7BFF,#37E2D8,#54F0A8)` | 標題漸層、重點文字 |
| `--fg` | `#E3EDFB` | 主文字（冷白） |
| `--muted` | `#8696B4` | 次要文字 |
| `--line` / `--line-2` | `rgba(125,160,205,.14 / .30)` | hairline 線條、面板邊框 |

**字體**（由 Google Fonts 載入）

- `Space Grotesk` — 標題 / 數字（科技感 display）
- `JetBrains Mono` — 標籤、議程時間、座標等（呼應「數據」語彙）
- `Inter` + `Noto Sans TC` — 中英內文

---

## 4. 視覺主軸（對應簡報三概念）

整頁的靈魂是**固定在背景的一層 canvas 流場**，內容用半透明玻璃面板浮在上面，所以滾動時背景連續、不是一塊塊堆疊。

1. **流動與串聯 Flow & Connection** — 藍→青→綠的光點沿 flow field 漂移，鄰近自動連線並帶拖尾，象徵 APEC 經濟體間的數據交換。
2. **賦能與轉型 Empowerment & Transformation** — 由簡入繁的碎形 / 幾何生長。
3. **數位與實體融合 Digital × Physical** — 機械網格疊上有機流線；背景三顆**解構線框水晶**（對應簡報「三礦物解構」）隨滑鼠/捲動視差浮動。

> 三個概念在「01 Concept」區各有一個獨立小 canvas 即時動畫示意。

效能與無障礙：`devicePixelRatio` 上限 2、依視窗大小自動調整粒子數、`prefers-reduced-motion` 下改為靜態單幀、RWD 至手機、保留鍵盤 focus。

---

## 5. 各區塊內容對照

| 區塊 | id | 內容 |
| --- | --- | --- |
| Hero | `#hero` | 主標 AI-Driven Design Innovation + 中文副標 + 日期/地點/形式/主辦四格 |
| 01 Concept | `#concept` | 三大設計主軸（含三個示意動畫） |
| 02 About | `#about` | 簡報 ABOUT 原文 + 三項數據 |
| 03 Agenda | `#agenda` | Day1 / Day2 切換式時間軸 |
| 04 Speakers | `#speakers` | 講者待公布佔位卡 ×4 |
| 05 Venue | `#venue` | 設研院資訊 + Google 地圖嵌入 |

---

## 6. 客製化指南（最常改的地方）

**改配色** → `:root` 內對應變數（見第 3 節）。

**改議程** → 議程是寫死的 HTML，在 `index.html` 找 `<div class="agenda show" id="d1">`（第一天）與 `id="d2"`（第二天）。每筆是一個 `.arow`：

```html
<div class="arow">
  <div class="time">10:20—11:10</div>
  <div>
    <div class="sess">場次標題</div>
    <div class="desc">場次說明（可省略）</div>
  </div>
</div>
<!-- 午休等用 <div class="arow brk"> 會以斜體灰字呈現 -->
```

**改講者** → 找 `<div class="spk-grid">`，每張卡是一個 `.spk`，把 `To be announced` 換成姓名、`role` 換成頭銜即可（水晶背景動畫會自動生成）。

**改地圖地點** → 找 `<iframe ... src="https://www.google.com/maps?q=台灣設計研究院&z=15&output=embed">`，把 `q=` 後改成正確地址或座標。地圖已套深色濾鏡（`.map-wrap iframe` 的 `filter`）與整體一致。

**調背景特效強度** → 在 `<script>` 內：
- 粒子密度：`resize()` 內 `Math.min(115, Math.max(46, (W*H)/15000))` — 數字越小越密
- 連線距離：常數 `CONN = 150` — 越大連線越多
- 拖尾長度：`frame()` 內 `ctx.fillStyle='rgba(5,8,15,0.16)'` 的 alpha — 越小拖尾越長
- 三顆水晶位置 / 大小 / 顏色：`resize()` 內 `crystals=[ makeCrystal(...) ]`

---

## 7. 待補 / 待確認（TODO）

- [ ] **創意劇場確切地址**：目前地圖以「台灣設計研究院」定位、文字標「松山文創園區一帶」，待主辦提供正式地址後更新 iframe `q=` 與 Venue 文字。
- [ ] **講者名單**：簡報註明「先示意、後續提供」，現為佔位卡。
- [ ] （可選）報名 / Register 按鈕與連結
- [ ] （可選）中 / 英語言切換
- [ ] （可選）主辦・協辦・贊助 logo 牆
- [ ] （可選）`favicon`、社群分享用 OG image、網站 meta 補完
- [ ] （可選）流量分析（GA4 / Plausible 等）

---

## 8. 技術備註

- 無框架、無套件、無建置流程；唯一外部資源為 Google Fonts CDN（離線環境可改為自託管字型）。
- 互動以原生 `Canvas 2D` + `IntersectionObserver` 實作，現代瀏覽器皆支援。
- 地圖採 Google Maps 內嵌，無需 API key。

© 2026 Taiwan Design Research Institute. Organized under APEC · DoIT, MOEA.
