# WILSON-ICY HOME EXPENSE

屋企每日開支系統。手機同電腦都用得，數據經 GitHub 同步。

- **記帳**：揀類別 → 撳數字 → 儲存。支援 `57+48` 連加，`¥` 掣自動將人民幣 × 1.09 轉港元。
- **分析**：每月日常開支、總開支（連固定），對比上月同去年同月、類別增減、近 6/12 個月趨勢，可以揀「不計旅行」。
- **記錄**：按月份睇、搜尋全部記錄，撳一筆就可以修改或刪除。
- **固定開支自動入帳**：到咗日子自動記低供樓、管理費、維修費、寬頻、Netflix。電費、水費、煤氣、差餉收到單再喺記帳頁揀「固定/帳單」輸入。
- **存入**：記帳頁揀「存入」，揀 Wilson (BO) / Icy (BB) / 其他（銀行回贈、旅行FUND），再揀存入「銀行戶口」或「現金POOL」。分析頁會顯示每月存入、本月結餘（存入 − 總開支）同累計存入。
- **傢私+電器、裝修工程**：喺「記錄」頁最底嘅「其他記錄」入面，可以睇總數、按類別分，亦可以新增。呢兩類唔計入每月分析。
- **旅行基金**：喺「記錄」頁最底「其他記錄 → 旅行基金」。每月輸入 %，P/L 自動按上月總資金計，亦會顯示總資金、累計 P/L、投入資金同每月走勢。
- **離線可用**：冇網絡都記到，有網絡時自動同步。
- **匯出**：可以匯出 Excel（日常開支 / 固定開支 / 存入 / 每月總結 / 傢私+電器 / 裝修工程）、CSV 同 JSON 備份。

---

## 架構

| Repo | 公開？ | 放乜 |
|---|---|---|
| `wilson-icy-home-expense` | Public（GitHub Pages 免費版要 public） | 呢個程式：`index.html`、`manifest.webmanifest`、`sw.js`、`icons/` |
| `wilson-icy-home-expense-data` | **Private** | `expenses.json`，即係你嘅開支數據，由程式自動讀寫 |

> ⚠️ **唔好**將 `expenses.json` 放入 public repo，否則全世界都睇到你屋企嘅開支。

---

## 設定步驟（一次過，大約 10 分鐘）

### 1. 上傳程式
1. 打開 `wilson-icy-home-expense` repo → **Add file → Upload files**。
2. 將呢個資料夾入面所有檔案拖入去（`index.html`、`manifest.webmanifest`、`sw.js`、`README.md` 同成個 `icons` 資料夾）→ **Commit changes**。
3. 去 **Settings → Pages**：Source 揀 **Deploy from a branch**，Branch 揀 `main`，folder 揀 `/ (root)` → **Save**。
4. 等一至兩分鐘，網址會係：`https://<你的帳號>.github.io/wilson-icy-home-expense/`

### 2. 開一個 private 數據 repo
1. 撳 GitHub 右上角 **+ → New repository**。
2. Name 填 `wilson-icy-home-expense-data`，揀 **Private**，剔 **Add a README file** → **Create repository**。
3. repo 入面唔使放任何檔案，程式第一次同步時會自動建立 `expenses.json`。

### 3. 整一條 Token（俾程式讀寫數據 repo）
1. GitHub 右上角頭像 → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**。
2. Token name：`home-expense`；Expiration 揀你想要嘅期限（例如 1 年，到期再整過一條）。
3. **Repository access** 揀 **Only select repositories**，然後只揀 `wilson-icy-home-expense-data`。
4. **Permissions → Repository permissions → Contents** 揀 **Read and write**，其他唔使改。
5. 撳 **Generate token**，然後複製嗰串 `github_pat_…`（只會顯示一次）。

### 4. 喺電腦設定，同時匯入舊 Excel 數據
1. 用瀏覽器打開第 1 步嘅網址 → **設定**。
2. 填：帳號名 = 你嘅 GitHub 帳號；數據 repo = `wilson-icy-home-expense-data`；檔案 = `expenses.json`；Branch = `main`；Token = 啱啱複製嗰串。
3. 撳 **儲存並同步**。右上角見到綠點「已同步」就成功。
4. 撳 **匯入 JSON**，揀 `expenses.json`（即係由你 Excel 轉出嚟嗰 604 筆記錄）。匯入後程式會自動上傳去 GitHub。

### 5. 喺手機設定
1. iPhone 用 Safari 打開同一個網址 → 撳分享掣 → **加至主畫面**。（Android Chrome 就撳 ⋮ → **加到主畫面**。）
2. 喺主畫面打開 app → **設定** → 填同一套 GitHub 資料 → **儲存並同步**。
3. 舊數據會自動由 GitHub 下載落嚟，**手機唔使再匯入**。

---

## 日常使用
- **買餸**：開 app → （可以撳「菜」「肉」快捷鍵）→ 撳數字，例如 `57 + 48` → **儲存**。
- **人民幣**：打金額後撳 `¥`，會自動轉港元，原本嘅人民幣金額亦會記低。
- **帳單**（電費、水費、煤氣、差餉）：記帳頁揀 **固定/帳單**，再揀類別同輸入金額。
- **電腦鍵盤**：直接打數字同 `+`，`Enter` 儲存，`Esc` 清除，`Backspace` 刪除一位。
- **月份備註**：喺分析頁底部填，例如「澳洲 trip」「差餉 Q3」，好似你 Excel 嘅 NOTE 欄咁。

## 同步係點運作
- 每一部機都會先存喺本機，所以離線都用得。
- 儲存之後大約 2 秒會自動同步；打開 app、網絡恢復時亦會自動同步。你亦可以撳右上角同步狀態，即刻同步一次。
- 兩部機同時改都唔會蓋咗對方：程式會按每筆記錄合併，同一筆記錄就以最後修改嗰次為準。
- 固定開支自動入帳時，每筆記錄有固定 ID（例如 `fx-mortgage-2026-10`），所以兩部機都唔會重複記。
- GitHub 會保留每次改動嘅紀錄，要搵返舊版本可以去數據 repo 嘅 commit history。

## 更新程式
改完 `index.html` 再上傳去 `wilson-icy-home-expense` repo 就得，數據唔會受影響。手機 app 關咗再開一兩次就會見到新版本。

## 常見問題
- **同步失敗：Token 無效** → Token 過咗期或者複製錯，照第 3 步整過一條，再喺每部機嘅設定度更新。
- **同步失敗：搵唔到 repo** → 檢查帳號名同 repo 名有冇打錯，同埋 Token 有冇揀到 `wilson-icy-home-expense-data`。
- **換新手機** → 喺新手機填返 GitHub 資料，數據就會自動落返嚟。
