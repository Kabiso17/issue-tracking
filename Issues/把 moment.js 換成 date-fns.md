---
id: ISS-20261119-101005
status: in-progress
type: refactor
priority: P2
assignee: any
claimed_by: claude-code
repo: C:\code\shop-web
labels:
  - 範例
  - tech-debt
related: []
created: 2026-11-19
updated: 2026-11-23
---
## 問題描述
moment.js 已經停止維護，而且佔了 bundle 大約 70 KB（gzip 後）。專案裡共有 31 個檔案在用。

## 預期結果
全專案改用 date-fns，移除 moment 依賴，畫面上的日期顯示完全不變。

## 驗收條件
- [x] 盤點所有使用 moment 的地方
- [ ] 全部改寫成 date-fns
- [ ] `package.json` 移除 moment
- [ ] 現有測試全部通過
- [ ] bundle 大小有下降（附上前後數字）

## 脈絡與線索
- `grep -r "from 'moment'" src/`
- 時區處理用 `date-fns-tz`

## 限制
- 一次一個模組，分多個 commit，方便 review


---
## 討論


## Agent 工作紀錄
- 2026-11-23 claude-code：認領，開始處理。
- 2026-11-23 claude-code：盤點完成，共 31 個檔案、58 處呼叫，大多是 `format()` 和 `diff()`。正在改 `src/utils/`。
