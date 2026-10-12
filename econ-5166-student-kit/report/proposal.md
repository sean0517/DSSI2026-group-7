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

既有研究顯示 F1 名次約九成由車隊決定，但同樣的車交給不同車手，結果仍有差異，而這些差異是否在開賽前就能辨識、是否有預測價值，目前沒有答案。本研究以 2011–2024 年資料，在正賽開跑前預測車手是否進入前三名，回答兩個問題：

1. **賽前資訊的上限在哪裡**：只用開賽前已知的資訊（起跑位置、排位賽速度差距、車手與車隊近期表現、歷史進站傾向、賽道特性、天氣），能把前三名預測到多準？
2. **車手本身還剩多少**：在控制起跑位置與車隊實力後，由前一季單圈與進站行為分出的駕駛風格群，是否仍能提升預測表現？

無論答案是肯定或否定，都能回答「賽前能知道的車手特質，對 F1 結果有多少解釋力」。本研究為預測問題，不作因果解釋。

### 研究動機

本研究的動機可以濃縮成四句話：資料顯示 F1 的名次約九成由賽車決定；但同一台車交給不同車手，結果仍會不同；這個「人」的部分被轉播與評論形容了幾十年，卻從未被科學形塑出來，也沒人拿去預測；所以我們來測。以下依序展開。

F1 有一個經常被討論的問題：「究竟是車快，還是人快？」資料看起來偏向車：2023 年 Red Bull 在 22 場大獎賽贏了 21 場；2024 年摩納哥站前十名的完賽順序與起跑順序完全一致，比賽在週六排位賽後就幾乎定了。既有研究也支持這個印象，車隊大約能解釋九成的名次變異（van Kesteren 與 Bergkamp，2023）。如果真是如此，預測每場前三名不會太難。但 F1 也不乏令人意外的比賽：2020 年沙基爾站，Mercedes 的替補車手首次駕駛便長時間領先卻錯失勝利，最後奪冠的是第一圈事故後跌到最後一名、開著中游賽車一路追回的 Sergio Pérez。同一個運動，有時賽前資訊幾乎決定結果，有時卻完全失效。

名次約九成由賽車決定，那剩下的一成，有多少是車手本身？轉播與評論常用「激進」、「穩定」、「擅長輪胎管理」形容車手，但這些標籤從未被量化，也沒人檢驗過它們對結果有沒有預測力。既有文獻分成三條線各走各的：一條把車手能力從賽車中分離出來、事後評價誰最強（Eichenberger 與 Stadelmann，2009；Phillips，2014；Bell 等人，2016；van Kesteren 與 Bergkamp，2023），但不描述車手怎麼開；一條用機器學習直接預測名次（Krzysztoń 與 Smołka，2026），但把賽中資料也放進特徵；一條從圈速與遙測分出駕駛風格群（Secco 與 Taylor，2026），但停在描述，沒有拿去預測。三條線沒有人接起來：**在已知起跑位置與車隊實力、且只用賽前可知資訊的情況下，由歷史資料分出的駕駛風格是否仍能提供額外的預測資訊？**

因此本專案結合**分群（Clustering）與預測（Prediction）**：先用歷史賽事資料建立可量化的駕駛特徵並分出風格群，再以起跑位置與車隊實力建立基準模型，加入風格群後比較對頒獎台預測能力的提升。我們問的不是哪位車手最強，而是**在控制主要賽前資訊後，駕駛風格是否具有額外的預測價值（Incremental Predictive Value）**。即使提升有限，也能回答賽前可知的車手特質究竟能解釋多少 F1 的比賽結果。

### 資料來源

1. **Formula 1 World Championship (1950 - 2024)**（Kaggle，源自 Ergast）：已取得 14 個 CSV，含賽程、賽道、車隊、車手、排位賽、正賽與衝刺賽結果、單圈時間、進站紀錄、完賽狀態。限制：進站紀錄自 2011 年起才有，決定了樣本起點；單圈資料沒有安全車、虛擬安全車與紅旗的標記，只能以圈速門檻近似排除；沒有輪胎配方資料。
2. **FastF1**（Python 套件）：輪胎配方與賽道狀態，僅涵蓋 2018 年以後，尚未下載。列為備用來源，主分析不使用。
3. **Open-Meteo** Historical Weather API：以賽道經緯度與開賽時間查詢比賽日的逐時氣溫與降雨，尚未串接。限制：取得的是實際天氣而非賽前預報，在模型中作為預報的代理變數。

### 研究方法

研究分為兩個階段：

**第一階段：非監督式分群（駕駛風格）**

以「車手-賽季」為分析單位（每位車手每季一筆）建立駕駛風格特徵。為降低車隊性能的干擾，特徵盡量以「相對隊友」或「相對該場中位數」的方式衡量：

- 圈速穩定度：每場單圈時間標準差（排除進出站圈、第一圈，以及慢於該車手該場中位數 10% 以上的圈，作為安全車與紅旗圈的近似排除；再除以該車手該場中位數圈速）的整季平均。
- 輪胎衰退：以進站圈將比賽切成數段 stint，每段內以單圈時間對圈數做簡單迴歸，斜率代表每圈掉速；取整季平均後減去隊友的同一數值。
- 相對隊友速度：與隊友中位數圈速的差距（%）。
- 攻擊性：平均「起跑位置 − 完賽順位（`positionOrder`）」減去隊友的同一數值，以及非機械因素退賽率。
- 進站傾向：平均進站次數相對該場中位數、第一次進站在全場圈數中的位置比例。

模型：特徵標準化後以 **K-means** 分群，群數 K 在 2–6 之間，以輪廓係數（silhouette score）與手肘法決定；並以階層式分群（Ward 法）檢驗分群結果的穩定性。

避免資料洩漏：分群標籤放入第二階段時，預測第 t 季比賽只使用第 t−1 季的分群標籤（`style_cluster_prev`），確保模型只用到開賽前已知的資訊。第 t 季新進的車手沒有前一季標籤，另設「無前季資料」類別，不直接刪除。

**第二階段：監督式分類（前三名預測）**

預測時點設在排位賽結束後、正賽開跑前。以「該車手於該場正賽是否進入前三名」作為二元目標變數，特徵限於此時已知的資訊：起跑位置與排位賽速度差距、車手與車隊近期表現、進站傾向與圈速表現、賽道特性、比賽日天氣，以及前一季的駕駛風格分群標籤。為確保所有特徵在開賽前即可觀察，進站與單圈相關特徵皆以該車手前 5 場比賽的平均值計算。比較下列模型：

- 基準模型：Logistic regression。
- 機器學習模型：Random forest、Gradient boosting（如 XGBoost）。

為回答第二個研究問題，每個模型各估計兩個版本：(a) 不含分群結果、(b) 加入駕駛風格分群標籤，並以依賽季切分的訓練／測試集（2012–2022 年訓練、2023–2024 年測試）比較兩者的準確率、F1 score 與 AUC，檢驗分群是否提升預測表現；兩版 AUC 的差距以 bootstrap 重抽樣估計信賴區間。由於前三名僅占約 15%，以 F1 score 與 AUC 為主要評估指標。

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
- `n_pit_stops`、`avg_pit_duration`：進站次數與平均進站時間（換胎策略代理變數），以該車手前 5 場比賽的平均值計算。
- `lap_time_mean`、`lap_time_sd`：單圈時間的平均與標準差（駕駛風格特徵），以該車手前 5 場比賽計算。
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

2011–2024 每場都有，只缺 2021 比利時站，那場因暴雨只跟著安全車跑一圈就結束、沒人進站，不是缺資料。

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

1. **資料範圍受進站紀錄限制**：Kaggle 的進站資料自 2011 年起才有，樣本起點因此定在 2011 年，第二階段模型實際從 2012 年開始，共約 13 季、5,500 筆。FastF1 的輪胎配方與賽道狀態只涵蓋 2018 年以後，無法用於全樣本，本研究不納入主分析，輪胎策略只能以進站次數與進站時間代理。
2. **安全車圈只能近似排除**：Kaggle 單圈資料沒有安全車、虛擬安全車與紅旗的標記，第一階段以「慢於該車手該場中位數 10% 以上」的門檻排除，可能漏抓或誤抓，影響圈速穩定度與輪胎衰退特徵的精確度。將以 2018 年以後的 FastF1 賽道狀態資料檢驗此規則的誤差。
3. **天氣為事後實際值且尚未取得**：Open-Meteo 提供的是比賽日實際天氣，不是賽前預報，模型中的天氣變數高估了真實預測情境可取得的資訊。雨戰一年僅兩三場，相關估計的變異大。天氣資料目前尚未串接，若無法取得，第二階段將先以不含天氣的版本進行。
4. **類別不平衡**：每場約 20 位車手中只有 3 位進入前三名，正例約 15%。整體準確率會被多數類別主導，因此以 F1 score 與 AUC 為主要指標，必要時對正例重新加權。
5. **賽車性能主導名次**：既有研究估計車隊可解釋約九成的名次變異，駕駛風格分群可能部分反映車隊而非車手本身。本研究以相對隊友特徵、車隊近期積分與「風格群 × 車隊」交叉表處理，但無法完全排除；季中換隊或隊友頻繁更換的車手，相對特徵的噪音較大。
6. **分群樣本小且標籤主觀**：車手-賽季只有約 260 筆，部分車手一季參賽場次少；非監督式分群的群數與命名缺乏客觀標準，需以不同群數與 Ward 法檢驗穩定性。新進車手沒有前一季標籤，只能歸入「無前季資料」類別，該類別人數少且異質。
7. **圈速資訊可能重複進入模型**：第二階段的近期圈速特徵與第一階段的分群標籤同樣來自單圈資料，兩者有部分重疊，可能壓縮 (a)、(b) 兩版的差距，使假說三的檢驗偏向保守。若兩版差距接近 0，需另以不含圈速特徵的版本確認。
8. **規則變動與跨世代測試**：F1 技術規則在 2014、2017、2022 年大改，車隊強弱重排，前一季的風格標籤在規則大改的年份可能失效。本研究以 2012–2022 年訓練、2023–2024 年測試，剛好跨過 2022 年的改制，測試集表現可能低於世代內的預測能力；這是刻意保留的設定，用來檢驗模型對規則變動的穩健性。
9. **人工標記與編碼規則**：街道賽沒有官方定義，半永久賽道需自訂規則；維修區起跑、無排位成績、退賽名次等情況也需要代入規則。不同規則可能影響結果，將在第 3 節欄位說明中列出所有編碼規則並做敏感度檢查。

## 6. 角色分工表：請列出組員之分工（PM 彙整）

- Project manager: 黃敬翔
- Data engineer: 黃欣予
- Data analyst: 蔡昇諺 林丞翊

## 參考文獻

各篇閱讀筆記與文獻地圖見 [references/f1-literature-map.md](../references/f1-literature-map.md)。

- Bell, A., Smith, J., Sabel, C. E., & Jones, K. (2016). Formula for success: Multilevel modelling of Formula One Driver and Constructor performance, 1950–2014. *Journal of Quantitative Analysis in Sports*, 12(2), 99–112. https://doi.org/10.1515/jqas-2015-0050
- Eichenberger, R., & Stadelmann, D. (2009). Who Is The Best Formula 1 Driver? An Economic Approach to Evaluating Talent. *Economic Analysis & Policy*, 39(3), 389–406.
- Heilmeier, A., Thomaser, A., Graf, M., & Betz, J. (2020). Virtual strategy engineer: Using artificial neural networks for making race strategy decisions in circuit motorsport. *Applied Sciences*, 10(21), 7805. https://doi.org/10.3390/app10217805
- Krzysztoń, S., & Smołka, J. (2026). Application of machine learning for predicting Formula 1 race results. *Journal of Computer Sciences Institute*, 38, 81–86. https://doi.org/10.35784/jcsi.8462
- Phillips, A. J. K. (2014). Uncovering Formula One driver performances from 1950 to 2013 by adjusting for team and competition effects. *Journal of Quantitative Analysis in Sports*, 10(2), 261–278.
- Secco, E. L., & Taylor, A. (2026). Can Unsupervised Learning Methods Reveal Distinct Driving Styles Among Formula 1 Drivers? *International Journal of Modeling and Optimization*, 16(1–2).
- van Kesteren, E.-J., & Bergkamp, T. (2023). Bayesian analysis of Formula One race results: disentangling driver skill and constructor advantage. *Journal of Quantitative Analysis in Sports*, 19(4), 273–293. https://arxiv.org/abs/2203.08489
