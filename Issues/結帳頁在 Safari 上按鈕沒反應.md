---
id: ISS-20261118-093012
status: open
type: bug
priority: P0
assignee: claude
claimed_by:
repo: C:\code\shop-web
labels:
  - 範例
  - frontend
  - checkout
related: []
created: 2026-11-18
updated: 2026-11-18
---
## 問題描述
iPhone Safari（iOS 18）上，結帳頁按「確認付款」完全沒反應，沒有錯誤畫面，也沒有送出請求。Chrome 和 Android 都正常。客服這兩天收到 12 件抱怨，等於 iOS 使用者完全無法下單。

## 預期結果
Safari 上按「確認付款」能正常送出訂單，並跳到付款成功頁。

## 驗收條件
- [ ] iOS Safari 18 可以完成結帳流程
- [ ] 桌機 Safari 17 可以完成結帳流程
- [ ] Chrome／Firefox 行為不變
- [ ] 找出根本原因，寫在工作紀錄

## 脈絡與線索
- 按鈕元件：`src/checkout/PayButton.tsx`
- 上週合併的 PR #482 把表單驗證改成 `structuredClone`，時間點吻合
- Sentry 有一筆：`TypeError: undefined is not a function (near '...form.requestSubmit...')`

## 限制
- 今天要能上 hotfix，先修好再說，不要順便重構


---
## 討論


## Agent 工作紀錄

