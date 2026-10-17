# Project Context

這份文件保存本組專案脈絡、教師設定的預設值與學生已確認的偏好。除明列的預設值外，未填項目不代表已確認；AI 應詢問當前任務需要的資訊。

## Repository Context

學生先自行建立小組 repo，再於 VS Code clone 並開啟，把完整工作包內容放在 repo 根目錄。正式學生產出前核對設定；AI 可唯讀檢查與提供說明，stage、commit、push、pull／sync 由學生親自操作。此空白範本不代表學生已經建好 repo；教師維護範本時不代填學生的 URL 或狀態。

| Field | Details |
| --- | --- |
| Remote repository | https://github.com/sean0517/DSSI2026-group-7.git（唯讀核對 `git remote -v`，2026-10-13） |
| Target branch | main |
| Setup status | repo 根目錄為 `DSSI2026-group-7/`，工作包位於子資料夾 `econ-5166-student-kit/`；VS Code 開啟的是 repo 根目錄 |
| Last push verification | 2026-10-13：`git fetch` 後 `origin/main` 最新為 `df531dc`（黃敬翔），`git branch -r --contains df531dc` 確認遠端包含；本地與遠端一致 |

## Research Question

- 待本組先用自己的話填寫：關心的現象或初步疑問，以及為什麼想研究。不必已有正式題目；AI 依本組想法協助釐清，教師範例不代表本組構想。

## Team

**每組預設 3–4 位學生（教師預設）**，以學生已確認的實際名單為準。AI 在學生開始使用時，主動確認全組成員姓名、各自已確認的實際工作，以及目前對話的學生是哪位。先沿用對話或本文件中已確認的資訊，只補問缺少的部分，不只是留下待填。尚未分配的工作由組員決定，未確認的欄位保留「待確認」。已確認的人數不同時依實際名單記錄，不為符合預設補造或刪除成員。

AI 只整理學生提供或明確確認的姓名與工作，不自行指派 PM／DE／DA，也不從 Git 作者、電腦帳號、檔名、範本示例或檔案提供者推定作者或責任；保留有依據的既有作者，不自動改成本次提供檔案的學生。等待回答時可先進行不依賴歸屬的檢查或討論。保留下列既有欄位，按已確認名單一人一列；尚無名單時保持空表，不先填入 3 或 4 位假成員。`Role` 記錄學生提供的角色與實際工作，確認姓名與工作不需額外要求學號或 GitHub 帳號。

目前對話的學生：待確認（依本次對話更新，不自動沿用上一次的對話者）。

| Name | Student ID | GitHub Account | Role |
| --- | --- | --- | --- |
| 黃敬翔 | r14546021 | sean0517 | Project manager (PM) |
| 蔡昇諺 | B12303146 | ky5900042-byte | Data analyst (DA) |
| 黃欣予 | B11610053 | xinyuhuang0615 | Data engineer (DE) |
| 林丞翊 | B12303006 | ericlin218 | Data analyst (DA) |


## Student Preferences

- Tool / artifact format: 待詢問學生（Python／Jupyter、R／R Markdown；明確選用 Stata 時依專用格式）
- Report language: 繁體中文（台灣用語，zh-TW；教師預設，學生明確指定其他語言時優先沿用）
- Other explicit preferences: 尚未提供

## AI Agent Context

依 `AGENTS.md` 的 `Model and Agent Selection`，記錄每位已確認學生目前使用的 AI 工具、模型與選用入口。同組成員可以使用不同模型；不要將前一位學生或教師的設定當作全組預設。姓名確認後按學生一人一列，換工具或模型時更新該列；尚無學生資訊時保持空表。

`Model` 保留學生提供或目前執行環境可靠顯示的名稱；只知道工具或模型家族時註明「完整型號未確認」，不自行猜版本。`Source` 記錄「學生本次提供」或「目前執行環境」等實際依據。先確認選用入口即可，不為填滿模型版本而打斷工作。

| Student | AI tool / client | Model | Agent entrypoint | Source |
| --- | --- | --- | --- | --- |
| 黃敬翔 | --- | --- | --- | --- |

## Data and Current Focus

| Field | Details |
| --- | --- |
| Data sources / documentation | 待填 |
| Unit of observation | 待填 |
| Current task / target variables | 待填 |
| Assignment constraints | 待填 |

## Data Artifacts

資料建立或核對後，由 AI 依 [資料銜接規則](FINDING-RULES.md#資料銜接與檔案驗證) 填入實際紀錄，一個資料檔一列。尚無資料時保持空表，不把範例當成已完成結果。

| Dataset | Role | Data file | Documentation | Processing notebook | Verification | SHA-256 |
| --- | --- | --- | --- | --- | --- | --- |

路徑與連結相對於本專案根目錄。Role 記錄 raw／analysis／train／test 或已確認用途；Verification 說明實際執行的檢查、日期與限制，未完成時明列 pending／failed。SHA-256 由實際檔案計算。外部整理檔沒有產生程式可記 not available；原始來源可記 not applicable (source)。

## Key Contributions

**課程原則：所有 contribution 都必須由學生在 VS Code 自行 commit 並 push。** 各成員隨成果更新下表，每項貢獻一列，記錄誰、做了什麼，以及實際包含該成果的 commit 連結或 SHA。AI 可整理待提交紀錄，在學生操作後唯讀核對再回填。程式、資料處理、文件、提案及專案紀錄的修改都適用；外部成果須有已 commit 的說明或連結紀錄可追溯。

尚未 commit 的工作標示「待提交」，已有 reference 但尚未核對者標示「待核對」；兩者均不視為完成貢獻紀錄。檔案連結不能取代 commit reference。不要以 commit 次數或程式行數替代成果評估。

已核對的本地 commit 不等於已 push；在 reference 後註明「推送待核對」或經證實的「待 push」，遠端目標分支包含該 commit 的證據核對完成後才記「已推送並核對」。學生自行回報但未核對時註明來源。貢獻表回填後，由學生在下一次 commit／push 納入；reference 指向先前的成果 commit，不要求填該表更新自身的未來 hash。

姓名與貢獻歸屬須由學生提供或確認；資訊缺少時先詢問，未回答前保留待填，不以 commit 作者直接認定實際負責人。commit 用來核對成果紀錄，不取代學生確認的分工。

| Name | Contribution | Commit reference |
| --- | --- | --- |
| 黃敬翔 | 重寫提案第 1 節（研究問題、動機、資料來源、方法）與第 5 節預期限制；新增 `references/` 9 篇 F1 文獻筆記與參考文獻清單；撰寫 2026-10-08 週會紀錄草稿與給 DA、DE 的檢查意見 | [`df531dc`](https://github.com/sean0517/DSSI2026-group-7/commit/df531dc) — 已推送並核對（2026-10-13） |
| 黃敬翔 | 建立提案初稿、填入組員分工、研究問題改為名次預測並加入氣候特徵 | [`005b7ae`](https://github.com/sean0517/DSSI2026-group-7/commit/005b7ae)、[`3122305`](https://github.com/sean0517/DSSI2026-group-7/commit/3122305) — 已推送並核對（2026-10-13） |
| 黃欣予 | 待本人確認（會議紀錄分工：第 2 節 raw data 下載與各表範例圖） | `cfd2a09`、`b8c26fc`、`281662c` 在 `origin/main`，歸屬待本人確認 |
| 林丞翊 | 撰寫研究方法第一階段（車手-賽季五組駕駛風格特徵、K-means 分群與避免資料洩漏的設計），補上研究問題二；第 3 節新增欄位定義、範例表與來源表；第 4 節新增預期主要結果 | [`66c23f5`](https://github.com/sean0517/DSSI2026-group-7/commit/66c23f5) — 已推送並核對（2026-10-17） |
| 林丞翊 | 修改研究方法第二階段：目標改為前三名預測，進站與單圈特徵改以該車手前 5 場平均計算，確保只用賽前資訊 | [`3d35a5a`](https://github.com/sean0517/DSSI2026-group-7/commit/3d35a5a) — 已推送並核對（2026-10-17） |
| 林丞翊 | 依 PM 10-13 檢查意見修改第 3、4 節（`style_cluster_prev`、編碼規則、假說修正）；定稿第一階段方法（5 場門檻、特徵全部減隊友、分群只用 2011–2021 年估計、公式）；以拆解法加入車隊風格，第二階段以車隊策略傾向取代 `n_pit_stops` 並補 logistic regression 公式 | [`c7538ae`](https://github.com/sean0517/DSSI2026-group-7/commit/c7538ae) — 已推送並核對（2026-10-17） |
| 蔡昇諺 | 待本人確認（會議紀錄分工：第 3、4 節，與林丞翊共同負責） | 尚無 commit 紀錄 |
