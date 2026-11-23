---
id: ISS-20261110-090000
status: done
type: bug
priority: P1
assignee: any
claimed_by: claude-code
repo: C:\code\shop-web
labels:
  - 範例
  - performance
related: []
created: 2026-11-10
updated: 2026-11-13
---
## 問題描述
首頁行動版 Lighthouse Performance 只有 54，LCP 4.8 秒。

## 預期結果
Performance 分數 80 以上。

## 驗收條件
- [x] 行動版 Performance ≥ 80
- [x] LCP < 2.5 秒
- [x] 版面沒有變化

## 脈絡與線索
- 首頁：`src/pages/index.tsx`

## 限制


---
## 討論
> [!note] 使用者 · 2026-11-13
> 實測 86 分，驗收通過，謝謝。

## Agent 工作紀錄
- 2026-11-11 claude-code：認領，開始處理。

### 2026-11-12 · claude-code
**結果：** Performance 54 → 87，LCP 4.8s → 1.9s。
**變更檔案：**
- `src/pages/index.tsx` — 主視覺圖改 `priority` 載入，輪播第 2 張之後改 lazy
- `src/components/Analytics.tsx` — 追蹤腳本改成 `afterInteractive`
- `next.config.js` — 開啟 AVIF
**驗證方式：** Lighthouse 行動版跑 5 次取中位數 87；Playwright 截圖比對無差異。
**Commit／PR：** PR #477
