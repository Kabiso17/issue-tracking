---
description: 認領並處理指定的 issue
argument-hint: <issue ID 或檔名>
---
要處理的 issue：$ARGUMENTS

1. 在 `Issues/` 找出這張 issue（用 `id` 比對，或用檔名部分比對）。找不到或有多張符合，就列出候選請使用者選。
2. 檢查能不能接：
   - `draft`：不要做，告訴使用者它還是草稿。
   - `done`／`wontfix`：不要做，除非使用者明確要求重開。
   - `claimed_by` 是別的 agent：先問使用者要不要接手。
   - `needs-info`：先看「討論」區人類是否已回覆；沒有回覆就停下，提醒使用者回答問題。
3. 照 AGENTS.md 的流程：認領 → 處理 → 卡住就提問並設 `needs-info` → 完成就寫工作紀錄並設 `review`。
4. 結束時用兩三句話告訴使用者結果，以及 issue 的新狀態。
