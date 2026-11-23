---
description: 根據描述建立一張新的 issue
argument-hint: <想做的事或遇到的問題>
---
使用者想開的 issue：$ARGUMENTS

照 AGENTS.md 的「開新 issue」規則，用 `Templates/Issue.md` 在 `Issues/` 建立新檔案：

- 從描述推斷 `type`、`priority`、`repo`；推不出來就用範本預設值。
- 把描述整理進「問題描述」「預期結果」；驗收條件寫成可驗證的句子。
- 描述太模糊、連驗收條件都寫不出來時，先問使用者一兩個關鍵問題再建立。
- 需要了解程式碼才能寫清楚時，可以先讀 `repo` 裡的相關檔案，把找到的檔案路徑寫進「脈絡與線索」。
- 預設 `status: draft`；使用者說「直接開始」或類似意思時才設 `open`。

建立完成後回報檔名、ID，以及使用者需要確認或補充的地方。
