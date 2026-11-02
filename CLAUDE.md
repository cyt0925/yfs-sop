# SOP 檢索網站（cyt0925/yfs-sop）

**開工前先讀 [`NOTES.md`](NOTES.md)** —— 架構、資料來源、待決定的事都在那。
這份只放最容易踩錯的幾條，細節不重複。

## 這是什麼

營運 SOP 檢索網站，**單一 `index.html`**（約 31 萬字元，CSS/JS/資料全在裡面，沒有 build 步驟）。
目前 46 份 SOP、305 個操作步驟、521 張截圖。兩個檢視：SOP 檢索、流程圖（8 階段主幹）。

這個 repo 只有 SOP 網站。訂單系統（coupang-oms）在另一個私人倉庫 `cyt0925/yfs`，
2026-11-02 資安體檢把那邊改成私人後 GitHub Pages 就停了，所以 SOP 網站搬到這裡（Jerry 決定）。

## 紅線

- **日常更新內容是改 Google 試算表，不是改 HTML。** `index.html` 裡的 `SEED` 只是離線備援，
  網站載入後會被試算表的 CSV 覆蓋。試算表網址在 `config.js`。
- **這個 repo 是公開的。** 編輯碼、API 金鑰、任何密碼都不能進 repo——放在 Apps Script 的 `Code.gs`。
- **GitHub Pages 只看 `main`**，推到別的分支網站不會變（踩過一次）。

## 開發

```bash
python3 -m http.server 8791   # 然後開 http://localhost:8791/index.html
```

不能用 `file://` 直接開，Google 試算表的 CSV 抓取會被 CORS 擋掉。

- 分支：`main`（Pages 直接看 main，推上去就上線）
- 線上：<https://cyt0925.github.io/yfs-sop/>
- 沒有自動化測試。改完 `index.html` 用 Playwright 實際點過搜尋、篩選、流程圖切換。
- 動到列印（`#printArea`、`@media print`）要用 Playwright 印成 PDF 看分頁，不能只看畫面。

## 環境注意

此環境 `git fetch` 可能給過期 ref，`git status` 的 ahead/behind 不可信。
確認真實狀態用 `git ls-remote origin <branch>` 比對 `git rev-parse HEAD`。
