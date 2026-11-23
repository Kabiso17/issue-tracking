---
description: 從 issue 佇列挑出最優先的一張並開始處理
---
依照 AGENTS.md 的「找 issue」規則，在 `Issues/` 裡找出你可以接的 issue（`status: open`、`assignee` 為 `claude` 或 `any`、`claimed_by` 空白），依 priority 再依 created 排序，取第一張。

- 找不到就列出目前各狀態的 issue 數量，以及有哪些 `needs-info`／`review` 在等使用者，然後停下。
- 找到就告訴使用者你挑了哪一張和原因，接著照 AGENTS.md 的流程認領、處理、回報（等同 `/issue-work`）。
