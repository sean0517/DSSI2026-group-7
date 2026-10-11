# Econ 5166 Project Proposal

- Group: DSSI2026 第 7 組
- Date created: 20261008
- Template: [econ-5516-proposal-template.md](../templates/report/econ-5516-proposal-template.md)

第 7 組

成員：

r14546021 黃敬翔

B12303146 蔡昇諺

B11610053 黃欣予

B12303006 林丞翊

## 1. 專案敘述：研究問題、動機與研究方法（組員共同討論，PM 彙整）

### 研究問題

本次研究問題主要是針對2011-2024年的資料，根據車手的駕駛風格、車隊策略、賽道特性、歷史表現與氣候狀況，預測 F1 排位賽與正賽的名次，並探討特徵與名次的關聯性。

本研究想回答一個問題：

1. 利用歷史起跑位置、進站與換胎策略、賽道特性等資訊，能否準確預測未來比賽的名次？
2. 由歷史單圈與進站行為分出的駕駛風格群，加入模型後能否提升前三名的預測表現？


### 研究動機

主要原因是組員個人對 F1 很有興趣，平常在看比賽的時候常常盲猜誰會贏，但如果能系統性的分析，或許有機會做正式的預測。因此，這個專案同時包含**分群與預測**：不只是預測誰贏得比賽，還能進一步分析出駕駛風格（透過分群整理出）與贏得比賽的關聯。

### 資料來源

1. **Formula 1 World Championship (1950 - 2024)** 資料集：賽道資料、車隊資料、車手資料、單圈時間、進站資料、排位賽結果、賽程資料、正賽結果、衝刺賽結果
2. **FastF1** 資料集：輪胎資料（僅使用session.laps）
3. **Open-Meteo**：以串接 Historical Weather API 查詢該次比賽的經緯度和日期來取得天氣資料

### 研究方法

研究分為兩個階段：

**第一階段：非監督式分群（駕駛風格）**

以「車手-賽季」為分析單位（每位車手每季一筆）建立駕駛風格特徵。為降低車隊性能的干擾，特徵盡量以「相對隊友」或「相對該場中位數」的方式衡量：

- 圈速穩定度：每場單圈時間標準差（排除進出站圈、第一圈與安全車圈，除以該場中位數圈速）的整季平均。
- 輪胎衰退：後半場減前半場的平均圈速差（%）。
- 相對隊友速度：與隊友中位數圈速的差距（%）。
- 攻擊性：平均「起跑位置 − 完賽名次」、非機械因素退賽率。
- 進站傾向：平均進站次數相對該場中位數、第一次進站在全場圈數中的位置比例。

模型：特徵標準化後以 **K-means** 分群，群數 K 在 2–6 之間，以輪廓係數（silhouette score）與手肘法決定；並以階層式分群（Ward 法）檢驗分群結果的穩定性。

避免資料洩漏：分群標籤放入第二階段時，預測第 t 季比賽只使用第 t−1 季的分群標籤（`style_cluster_prev`），確保模型只用到開賽前已知的資訊。

**第二階段：監督式分類（名次預測）**(待修改)

以「該車手於該場比賽是否進入前三名」作為二元目標變數，特徵包含排位賽起跑位置、賽道特性、進站次數、換胎策略、車手與車隊歷史表現等。比較下列模型：

- 基準模型：Logistic regression。
- 機器學習模型：Random forest、Gradient boosting（如 XGBoost）。

為回答第二個研究問題，每個模型各估計兩個版本：(a) 不含分群結果、(b) 加入駕駛風格分群標籤，並以依賽季切分的訓練／測試集（例如以 2023 年以前訓練、2023–2024 年測試）比較兩者的準確率、F1 score 與 AUC，檢驗分群是否顯著提升預測表現。

## 2. 已有之 raw data

**circuits(賽道基本資料)**

![circuits 範例](./data_images/circuits.png)

**constructors(車隊基本資料)**

![constructors 範例](./data_images/constructors.png)

**constructor_results(車隊單場得分)**

![constructor_results 範例](./data_images/constructor_results.png)

**constructor_standings(車隊積分榜)**

![constructor_standings 範例](./data_images/constructor_standings.png)

**drivers(車手基本資料)**

![drivers 範例](./data_images/drivers.png)

**driver_standings(車手積分榜)**

![driver_standings 範例](./data_images/driver_standings.png)

**lap_times(單圈時間)**

![lap_times 範例](./data_images/lap_times.png)

**pit_stops(進站紀錄)**

![pit_stops 範例](./data_images/pit_stops.png)

**qualifying(排位賽成績)**

![qualifying 範例](./data_images/qualifying.png)

**results(每場比賽的車手成績)**

![results 範例](./data_images/results.png)

**sprint_results(衝刺賽成績)**

![sprint_results 範例](./data_images/sprint_results.png)

**status(完賽狀態代碼)**

![status 範例](./data_images/status.png)

## 3. 主要分析用資料需求（組員共同討論，DA 負責撰寫，DE 確認可執行）
(DA DE 各輸出一版 10/10)

樣本時間：預計使用 2011–2024 年（單圈時間與進站資料自 2011 年起較為完整），實際範圍待 DE 確認資料完整度後定案。由於第一階段的分群標籤採用前一季結果，2011 年資料主要用於建立第一季的分群與歷史表現特徵，第二階段的模型樣本實際上從 2012 年開始。

範圍：各賽季所有大獎賽的全部參賽車手（不含衝刺賽），一筆觀察值為「一位車手在一場比賽」。2012–2024 年共約 267 場、5,500 筆觀察值，其中前三名約占 15%。

| raceId | year | circuitId | driverId | constructorId | grid | quali_gap_pct | n_pit_stops | avg_pit_duration | lap_time_mean | lap_time_sd | drv_avg_finish_l5 | con_points_l5 | street_circuit | air_temp_mean | precip_mm | style_cluster | podium |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1101 | 2023 | 3 | 830 | 9 | 1 | 0.00 | 2 | 23.4 | 93.21 | 1.05 | 1.8 | 38.2 | 0 | 27.5 | 0.0 | 2 | 1 |
| 1101 | 2023 | 3 | 1 | 131 | 4 | 0.41 | 3 | 24.1 | 93.87 | 1.32 | 5.6 | 21.0 | 0 | 27.5 | 0.0 | 1 | 0 |
| … | … | … | … | … | … | … | … | … | … | … | … | … | … | … | … | … | … |

（表中數值皆為假想示意，非實際資料。）

欄位說明：

- `grid`：排位賽起跑位置。
- `n_pit_stops`、`avg_pit_duration`：進站次數與平均進站時間（換胎策略代理變數）。
- `lap_time_mean`、`lap_time_sd`：單圈時間的平均與標準差（駕駛風格特徵）。
- `style_cluster`：第一階段分群得到的駕駛風格群標籤。
- `podium`：目標變數，完賽名次在前三名為 1，否則為 0。
- `quali_gap_pct`：該車手最佳排位圈速與桿位圈速的差距（%），補充起跑位置無法反映的速度差距。
- `drv_avg_finish_l5`：該車手前 5 場正賽的平均完賽名次（車手歷史表現）。
- `con_points_l5`：該車隊前 5 場正賽的平均積分（車隊歷史表現，控制賽車性能）。
- `street_circuit`：賽道特性，街道賽為 1、常設賽道為 0（人工標記）。
- `air_temp_mean`、`precip_mm`：比賽日於賽道所在地的平均氣溫（°C）與降雨量（mm），對應研究問題中的氣候狀況。

欄位來源與合併方式：

| 欄位 | 來源 |
| --- | --- |
| `raceId`、`year`、`circuitId`、`driverId`、`constructorId`、`grid`、`podium` | Kaggle `results.csv`、`races.csv`；`podium` 由 `positionOrder` ≤ 3 編碼 |
| `quali_gap_pct` | Kaggle `qualifying.csv`（q1／q2／q3 取最佳） |
| `n_pit_stops`、`avg_pit_duration` | Kaggle `pit_stops.csv` |
| `lap_time_mean`、`lap_time_sd` | Kaggle `lap_times.csv` |
| `drv_avg_finish_l5`、`con_points_l5` | Kaggle `results.csv`、`constructor_results.csv`，僅以該場之前的比賽計算 |
| `street_circuit` | Kaggle `circuits.csv` ＋人工標記 |
| `air_temp_mean`、`precip_mm` | Open-Meteo 歷史天氣 API，以 `circuits.csv` 的經緯度（lat／lng）與 `races.csv` 的比賽日期查詢 |
| `style_cluster` | 第一階段輸出，以 `driverId` ＋ `year` 合併，使用前一季標籤 |

待 DE 確認：

- `pit_stops.csv` 與 `lap_times.csv` 是否從 2011 年起就完整。
- Open-Meteo 用比賽日的整日資料，還是開賽時段的逐時資料。
- 如果要使用 FastF1 的輪胎資料，據我所知只涵蓋 2018 年以後。

## 4. 假說與預期主要結果（組員共同討論，DA 負責撰寫）

**假說一**：排位賽起跑位置是重要的預測因子，起跑位置越前面，進入前面名次的機率越高；進站次數與換胎策略對預測亦有顯著貢獻。

**假說二**：車手的駕駛風格並非隨機，可透過單圈時間分布與進站行為分為數個具可解釋性的群（例如「穩定型」、「激進型」），且不同風格群在特定賽道類型的前三名比率有系統性差異。

**假說三**：將駕駛風格分群標籤加入預測模型，可顯著提升前三名預測的準確率。

### 預期主要結果

以下圖表與數值皆為**假想示意**，用來說明預期的結果型態。實際結果由 DE 在模型完成後填入對應的表格。

**對應假說一：起跑位置與進站策略的預測力**

- Logistic regression 中，`grid` 的係數預期為負，也就是起跑位置越後面，進入前三名的機率越低。
- Random forest 的特徵重要性排序中，預期 `grid` 與 `quali_gap_pct` 排在最前面，其次是反映賽車性能的 `con_points_l5`。
- `n_pit_stops`、`avg_pit_duration` 的重要性預期大於 0，但明顯低於起跑位置。這表示進站策略有額外的預測力，但不是主要因素。

| 排名 | 特徵（假想） | 重要性（假想） |
| --- | --- | --- |
| 1 | `grid` | 0.32 |
| 2 | `quali_gap_pct` | 0.18 |
| 3 | `con_points_l5` | 0.14 |
| … | … | … |
| — | `n_pit_stops` | 0.04 |

**對應假說二：駕駛風格分群**

- 預期 K-means 依輪廓係數會選出 3–4 群，並能依特徵平均值命名，例如「穩定型」（圈速標準差小、輪胎衰退小）、「激進型」（名次推進多、退賽率高）、「策略型」（進站次數偏離中位數）。
- 預期不同風格群在街道賽與常設賽道的前三名比率不同。例如穩定型在不易超車的街道賽前三名比率較高，激進型則在常設賽道較高。

| 風格群（假想） | 街道賽前三名比率 | 常設賽道前三名比率 |
| --- | --- | --- |
| 穩定型 | 22% | 15% |
| 激進型 | 10% | 18% |
| 策略型 | 14% | 14% |

- 另外以「風格群 × 車隊」交叉表檢查分群是否只反映車隊。如果同一車隊的兩位車手常被分到不同群，表示分群確實捕捉到車手本身的差異。

**對應假說三：加入分群標籤是否提升預測表現**

- 預期 (b) 版（加入 `style_cluster`）的測試集 AUC 與 F1 會略高於 (a) 版（不含分群），但差距不大。
- 如果兩版差距接近 0，代表駕駛風格的資訊可能已經被起跑位置與車手歷史表現涵蓋，假說三不成立。這仍是有意義的結果。

結果表（DE 將對應模型結果寫在這）：

| 模型 | 版本 | Accuracy | F1 | AUC |
| --- | --- | --- | --- | --- |
| Logistic regression | (a) 不含分群 | | | |
| Logistic regression | (b) 含分群 | | | |
| Random forest | (a) 不含分群 | | | |
| Random forest | (b) 含分群 | | | |
| XGBoost | (a) 不含分群 | | | |
| XGBoost | (b) 含分群 | | | |

## 5. 預期限制（組員共同討論，DA 負責撰寫）：

1. **資料完整度**：單圈時間與進站資料在早期賽季缺漏較多，樣本可能須限縮在較近年的賽季，影響樣本數。
2. **類別不平衡**：每場比賽約 20 位車手中只有 3 位進入前三名，正例比率偏低，需使用適當的評估指標（F1、AUC）或重新加權。
3. **賽車性能的干擾**：F1 成績高度受車隊賽車性能影響，駕駛風格分群可能部分反映車隊而非車手本身，需控制車隊或賽季固定效果。
4. **分群結果的解釋性**：非監督式分群的群數與標籤缺乏客觀標準，分群結果的穩定性需以不同群數與方法檢驗。
5. **規則變動**：F1 技術與輪胎規則多次改變，跨賽季的可比性有限。

## 6. 角色分工表：請列出組員之分工（PM 彙整）

- Project manager: 黃敬翔
- Data engineer: 黃欣予
- Data analyst: 蔡昇諺 林丞翊
