---
id: ISS-20261123-000000
status: draft
type: feature
priority: P3
assignee: any
claimed_by:
repo:
labels:
  - 範例
related: []
created: 2026-11-23
updated: 2026-11-23
---
> [!example] 這是示範用的 issue
> 狀態是 `draft`，agent 不會處理它。看完可以刪掉。

## 問題描述
報表頁目前只能匯出 TSV，財務同事要用 Excel 開，每次都得手動轉換分隔符號。

## 預期結果
報表頁的「匯出」按鈕多一個 CSV 選項，下載下來的檔案 Excel 直接雙擊就能正確開啟。

## 驗收條件
- [ ] 匯出選單出現「CSV」選項
- [ ] CSV 用 UTF-8 with BOM 編碼，Excel 開啟中文不會亂碼
- [ ] 欄位內含逗號或換行時，有正確加上雙引號跳脫
- [ ] 原本的 TSV 匯出行為不變
- [ ] 有對應的單元測試

## 脈絡與線索
- 匯出邏輯在 `src/report/export.ts`
- TSV 的測試在 `tests/report/export.test.ts`，可以參考

## 限制
- 不要新增第三方套件


---
## 討論
> [!question] claude-code · 2026-11-23
> （示範）CSV 要設成預設格式，還是 TSV 維持預設？

> [!note] 使用者 · 2026-11-23
> （示範）維持 TSV 預設，CSV 放第二個。

## Agent 工作紀錄
- 2026-11-23 claude-code：（示範）認領，開始處理。
