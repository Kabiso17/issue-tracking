# Issue Tracking — Agent 工作守則

這個資料夾是一個 Obsidian 儲存庫，同時也是給 code agent（Claude Code、Codex、Cursor 等）使用的 issue tracker。
人類在 Obsidian 裡開 issue，agent 讀取、認領、處理、回報。**所有溝通都寫在 issue 檔案裡**，不要只回在對話中。

## 檔案位置

| 路徑 | 用途 |
|---|---|
| `Issues/*.md` | 每個 issue 一個檔案，檔名就是標題 |
| `Templates/Issue.md` | 新 issue 的範本（Obsidian 格式，含 `{{date}}` 佔位字） |
| `Issues.base` | Obsidian Bases 看板，不需要 agent 修改 |

## Frontmatter 欄位

```yaml
id: ISS-20261123-142530   # 唯一 ID：ISS-年月日-時分秒，建立後不要改
status: open              # 見下方狀態表
type: task                # bug | feature | task | refactor | docs | question
priority: P2              # P0（緊急）| P1 | P2 | P3（有空再做）
assignee: any             # 給誰做：claude | codex | any | human
claimed_by:               # 目前實際在處理的 agent，例如 claude-code；空白表示沒人認領
repo:                     # 要改的程式碼所在路徑（絕對路徑或 git URL）；空白表示就是本儲存庫
labels: []
related: []               # 相關 issue，用 "[[檔名]]" 格式
created: 2026-11-23
updated: 2026-11-23       # 每次修改 issue 都要更新成今天
```

## 狀態流程

```
draft ──人寫完──▶ open ──agent 認領──▶ in-progress ──做完──▶ review ──人驗收──▶ done
                   ▲                      │
                   └──人回覆── needs-info ◀┘（卡住、需要人決定）
任何狀態 ──人決定不做──▶ wontfix
```

| status | 誰設定 | 意思 |
|---|---|---|
| `draft` | 人 | 還在寫，**agent 不要碰** |
| `open` | 人 | 可以開始做 |
| `in-progress` | agent | 已認領，正在做 |
| `needs-info` | agent | 卡住了，在「討論」區提問，等人回覆 |
| `review` | agent | 做完了，等人驗收 |
| `done` | **只有人** | 驗收通過 |
| `wontfix` | **只有人** | 不做了 |

**Agent 絕對不能把 issue 設成 `done` 或 `wontfix`。** 做完一律設 `review`。

## 工作流程

### 1. 找 issue

符合以下條件的 issue 才能接：

- `status: open`
- `assignee` 是你自己（`claude`／`codex`…）或 `any`
- `claimed_by` 是空的
- `labels` 沒有 `範例`（那是示範資料，**一律不處理**，就算使用者叫你做下一張也跳過）

挑選順序：`priority` 小的先（P0 > P1 > P2 > P3），同優先度 `created` 早的先。

快速搜尋：

```bash
# 檔名常含空白，不要用 xargs
grep -l "^status: open" Issues/*.md | while IFS= read -r f; do
  grep -q "^assignee: human" "$f" || grep -q "^  - 範例" "$f" || echo "$f"
done
```

### 2. 認領

開始動手前，**先**修改 frontmatter 並存檔，避免多個 agent 撞在一起：

```yaml
status: in-progress
claimed_by: claude-code      # 你的名字
updated: <今天>
```

然後在「Agent 工作紀錄」加一筆：`- 2026-11-23 claude-code：認領，開始處理。`

認領前若發現 `claimed_by` 已經有值，就換下一個 issue。

### 3. 處理

- 先完整讀過 issue，包含「討論」區裡人類後來補充的內容（後面的回覆優先於前面的描述）。
- 程式碼在 `repo` 指定的路徑；若無法存取該路徑，設 `needs-info` 並說明。
- 遵守「限制」區的每一條。
- 驗收條件做到哪條就勾哪條（`- [ ]` → `- [x]`）。做不到的不要勾，在工作紀錄說明原因。

### 4. 卡住時：提問

需要人做決定、資訊不足、或發現需求互相矛盾時，**不要猜**：

1. 在「討論」區最下面加一個提問 callout
2. `status: needs-info`，`claimed_by` 保留（人回覆後由你繼續）
3. 停下來，回報給使用者

```markdown
> [!question] claude-code · 2026-11-23
> 驗收條件第 2 條說要支援 CSV，但 repo 裡已有 TSV 匯出。要兩種都做，還是改成 CSV？
```

人類會用 `> [!note] 人名 · 日期` 回覆，並把 status 改回 `open`。

### 5. 完成

1. 在「Agent 工作紀錄」寫完整報告（格式見下）
2. `status: review`、`updated: <今天>`
3. `claimed_by` 保留，讓人知道是誰做的

工作紀錄格式：

```markdown
### 2026-11-23 · claude-code
**結果：** 一兩句話講做了什麼。
**變更檔案：**
- `src/export.ts` — 新增 CSV 輸出
- `tests/export.test.ts` — 新增 3 個測試
**驗證方式：** `npm test` 全部通過（42 passed）；手動匯出一份檔案確認欄位正確。
**未完成／注意事項：** 大於 10 MB 的檔案還沒測。
**Commit／PR：** abc1234（如果有）
```

如實回報：測試失敗就寫失敗和輸出，跳過的步驟就寫跳過，不要寫成成功。

### 6. 被退回

人驗收不通過時，會在「討論」區留言並把 status 改回 `open`（`claimed_by` 可能保留）。
接手時讀完最新的留言再動手，工作紀錄新增一筆，不要覆蓋舊的。

## 編輯規則

- **用局部編輯**（Edit）修改 issue，不要整份重寫。人可能同時在 Obsidian 裡開著這個檔案。
- **不要改人寫的內容**：「問題描述」到「限制」這幾區是人的，agent 只能勾驗收條件的核取方塊。
- 只在「討論」和「Agent 工作紀錄」兩區**往下新增**，不要刪除或改寫舊紀錄。
- 保持 frontmatter 是合法 YAML；欄位名稱和順序不要動。
- 連結其他筆記用 `[[檔名]]`，不要加 `.md`。
- 不要刪除或改名 issue 檔案。

## 開新 issue（使用者請你建立時）

1. 複製 `Templates/Issue.md` 的內容，把佔位字換掉：
   - `{{date:YYYYMMDD}}-{{time:HHmmss}}` → 現在時間，例如 `20261123-142530`
   - `{{date:YYYY-MM-DD}}` → 今天日期
2. 檔名用簡短的標題，存在 `Issues/` 底下，例如 `Issues/匯出功能支援 CSV.md`。
   檔名避免 `\ / : * ? " < > | # ^ [ ]` 這些字元。
3. 填好各區內容；驗收條件要寫成可驗證的句子。
4. 預設 `status: draft`，讓人確認後再改成 `open`。除非使用者明確說可以直接開始。

## 處理過程中發現別的問題

不要順手擴大範圍。另開一個新 issue（`status: draft`），在 `related` 互相連結，然後回到原本的工作。
