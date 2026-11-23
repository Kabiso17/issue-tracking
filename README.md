# Obsidian Issue Tracker for Code Agents

用 Obsidian 當 issue tracker：你在 Obsidian 裡開 issue，Claude Code、Codex 等 code agent 讀取、認領、處理，再把結果寫回同一個檔案。

- **只用 Obsidian 內建功能**：Bases 做看板、Templates 做範本，不需要安裝社群外掛
- **issue 就是 Markdown 檔**：agent 不需要 API，讀寫檔案就能工作
- **人負責決定，agent 負責執行**：agent 做完只能設成「待驗收」，結案由你決定
- **所有溝通留在 issue 裡**：agent 的提問、你的回覆、完工報告都寫在同一個檔案

## 快速開始

1. Clone 這個 repo
   ```bash
   git clone https://github.com/Kabiso17/issue-tracking.git
   ```
2. 在 Obsidian 選「開啟資料夾作為儲存庫」，選這個資料夾
3. 打開 **Issue 看板** 筆記
4. 到「設定 → 外觀 → CSS 程式碼片段」確認 **issue-dashboard** 已開啟（讓看板滿版、欄位平均分配）

需要 Obsidian 1.10 以上（Bases 的分組功能）。

`Issues/` 裡有幾張標了 `範例` 標籤的示範 issue，涵蓋每一種狀態。agent 會跳過它們；不需要時直接刪掉即可。

## 開一張 issue

1. 在 `Issues/` 資料夾新增筆記，檔名就是標題
2. `Ctrl+P` →「範本：插入範本」→ **Issue**
3. 填好問題描述、驗收條件、`repo`（程式碼路徑）、`assignee`（`claude`／`codex`／`any`／`human`）
4. 把 `status` 從 `draft` 改成 `open`

## 叫 agent 處理

### Claude Code

在這個資料夾啟動。程式碼在別處的話，用 `--add-dir` 加進來：

```bash
claude --add-dir /path/to/your/project
```

| 指令 | 作用 |
|---|---|
| `/issue-next` | 挑出最優先的一張並開始處理 |
| `/issue-work <ID 或檔名>` | 處理指定的 issue |
| `/issue-new <描述>` | 根據描述建立新 issue |
| `/issue-list [篩選]` | 列出 issue 狀態 |

### 其他 agent

Codex 等支援 `AGENTS.md` 的 agent，在這個資料夾啟動時會自動讀取工作守則。其他 agent 可以直接告訴它：

> 讀 `AGENTS.md`，然後處理 `Issues/` 裡最優先的 issue。

## 狀態流程

```
draft ──人寫完──▶ open ──agent 認領──▶ in-progress ──做完──▶ review ──人驗收──▶ done
                   ▲                      │
                   └──人回覆── needs-info ◀┘（卡住、需要人決定）
任何狀態 ──人決定不做──▶ wontfix
```

| status | 誰設定 | 意思 |
|---|---|---|
| `draft` | 人 | 還在寫，agent 不會碰 |
| `open` | 人 | 可以開始做 |
| `in-progress` | agent | 已認領，正在做 |
| `needs-info` | agent | 有問題要問你，寫在 issue 的「討論」區 |
| `review` | agent | 做完了，等你驗收 |
| `done` | 人 | 驗收通過 |
| `wontfix` | 人 | 不做了 |

看板上的「等我回覆」視圖會列出所有 `needs-info` 和 `review`，平常看這裡就好。

## 檔案結構

```
├── Issue 看板.md              首頁：等我回覆＋依階段分組的看板
├── Issues.base               Bases 資料庫（看板／等我回覆／Agent 佇列／全部／已結案）
├── Issues/                   所有 issue，一個檔案一張
├── Templates/Issue.md        新 issue 範本
├── AGENTS.md                 agent 工作守則
├── CLAUDE.md                 Claude Code 設定（引用 AGENTS.md）
├── 使用說明.md                完整使用說明
├── .claude/commands/         Claude Code 斜線指令
└── .obsidian/                Obsidian 設定（範本資料夾、屬性型別、CSS snippet）
```

完整的欄位說明和回覆 agent 的格式請看 [使用說明.md](使用說明.md)；agent 的完整工作規則請看 [AGENTS.md](AGENTS.md)。
