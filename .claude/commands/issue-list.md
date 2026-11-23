---
description: 列出 issue 目前的狀態
argument-hint: [篩選條件，例如 open、P0、bug]
---
讀取 `Issues/*.md` 的 frontmatter，用表格列出 issue（ID、檔名、status、priority、type、assignee、claimed_by），依 status 流程順序分組、組內依 priority 排序。

篩選條件：$ARGUMENTS（空白就列出所有未結案的，即排除 `done` 和 `wontfix`）

最後另外列出等使用者處理的項目：`needs-info`（附上 agent 的提問摘要）和 `review`。只讀不改。
