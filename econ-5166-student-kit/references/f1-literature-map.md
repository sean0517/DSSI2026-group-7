# F1 文獻地圖：車手 vs 賽車、名次預測、駕駛風格

- Date created: 20261013
- 用途：支撐 `report/proposal.md` 研究動機中「現有研究大多分別探討駕駛風格的辨識與比賽結果的預測，較少進一步檢驗兩者之間的關聯」這句話。
- 說明：各篇筆記依據摘要、出版社頁面與二手引用整理，尚未逐篇閱讀全文；需要引用具體數字（如變異比例）時請先核對原文。

## 三條文獻脈絡

| Article Title | Authors | Journal / Outlet | Year | Role in Discussion | Link / DOI |
| --- | --- | --- | --- | --- | --- |
| Who Is The Best Formula 1 Driver? An Economic Approach to Evaluating Talent | Eichenberger, R.; Stadelmann, D. | Economic Analysis & Policy, 39(3), 389–406 | 2009 | 脈絡一（車 vs 人）：最早以計量方法把車手能力從賽車品質中分離出來，說明原始名次混合了兩者。 | [IDEAS](https://ideas.repec.org/a/eee/ecanpo/v39y2009i3p389-406.html) |
| Uncovering Formula One driver performances from 1950 to 2013 by adjusting for team and competition effects | Phillips, A. J. K. | Journal of Quantitative Analysis in Sports, 10(2), 261–278 | 2014 | 脈絡一：以調整後積分為結果變數，同時估計車手、車隊與賽季難度效果；處理各季參賽人數不同的問題。 | [De Gruyter](https://www.degruyterbrill.com/document/doi/10.1515/jqas-2013-0031/html) |
| Formula for success: Multilevel modelling of Formula One Driver and Constructor performance, 1950–2014 | Bell, A.; Smith, J.; Sabel, C. E.; Jones, K. | Journal of Quantitative Analysis in Sports, 12(2), 99–112 | 2016 | 脈絡一：多層次模型把變異拆成車隊、車隊-年、車手三層；車隊效果大於車手且逐年擴大，但在雨天與街道賽減弱。直接支持本研究假說二的賽道類型差異。 | [10.1515/jqas-2015-0050](https://doi.org/10.1515/jqas-2015-0050)、[White Rose 全文](https://eprints.whiterose.ac.uk/96995/) |
| Bayesian analysis of Formula One race results: disentangling driver skill and constructor advantage | van Kesteren, E.-J.; Bergkamp, T. | Journal of Quantitative Analysis in Sports, 19(4), 273–293 | 2023 | 脈絡一：以 2014–2021 油電時代資料建立貝氏多層次 rank-ordered logit，約 88% 的名次變異由車隊解釋。給本研究「賽車主導」的量化依據。 | [arXiv 2203.08489](https://arxiv.org/abs/2203.08489)、[PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10660124/) |
| Application of machine learning for predicting Formula 1 race results | Krzysztoń, S.; Smołka, J. | Journal of Computer Sciences Institute, 38, 81–86 | 2026 | 脈絡二（名次預測）：SVM、Gradient Boosting、Random Forest 集成，驗證集準確率約 80%；作者將預測品質歸因於遙測資料，即大量使用賽中資訊。 | [10.35784/jcsi.8462](https://doi.org/10.35784/jcsi.8462) |
| Can Unsupervised Learning Methods Reveal Distinct Driving Styles Among Formula 1 Drivers? | Secco, E. L.; Taylor, A. | International Journal of Modeling and Optimization, 16(1–2) | 2026 | 脈絡三（駕駛風格）：以 FastF1 遙測與圈速建立相對圈速、圈速穩定度、輪胎衰退等特徵，比較 K-Means、階層式、DBSCAN、GMM，得到 3–5 群；以隊內相對指標部分控制賽車差異。只做分群，未檢驗預測價值。 | [IJMO](https://ojs.ejournal.net/index.php/ijmo/article/view/22205) |
| Virtual strategy engineer: Using artificial neural networks for making race strategy decisions in circuit motorsport | Heilmeier, A.; Thomaser, A.; Graf, M.; Betz, J. | Applied Sciences, 10(21), 7805 | 2020 | 脈絡二補充（策略）：以 2014–2019 計時資料訓練兩個神經網路決定進站時機與配方，說明進站策略是賽中決策，本質上不是賽前可知資訊。 | [10.3390/app10217805](https://doi.org/10.3390/app10217805) |
| Tire Changes, Fresh Air, and Yellow Flags: Challenges in Predictive Analytics for Professional Racing | Tulabandhula, T.; Rudin, C. | Big Data, 2(2), 97–112 | 2014 | 脈絡二補充（策略）：賽車運動賽中預測的早期文獻，強調領域知識（換胎、黃旗）必須寫進統計模型。 | [10.1089/big.2014.0018](https://doi.org/10.1089/big.2014.0018) |

## 文獻缺口（支撐研究動機的論點）

1. **脈絡一**（Eichenberger & Stadelmann 2009；Phillips 2014；Bell et al. 2016；van Kesteren & Bergkamp 2023）的目標是事後評價「誰是最好的車手」，把車手效果當成一個固定或隨機效果估計，不描述車手「怎麼開」，也不做賽前預測。
2. **脈絡二**（Krzysztoń & Smołka 2026；相關碩士論文與開源專案）直接預測名次，但多數使用遙測或賽中資料當特徵，且未區分「賽前可知」與「賽中產生」的資訊，也沒有檢驗某一類特徵的邊際貢獻。
3. **脈絡三**（Secco & Taylor 2026）用非監督式方法分出駕駛風格群，作者明確指出賽車性能主導原始圈速，並以隊內相對指標處理，但停在描述分群，沒有把群標籤放進任何預測模型。
4. **因此的缺口**：尚無研究在控制起跑位置與車隊實力之後，檢驗由歷史資料分出的駕駛風格群是否對頒獎台預測有額外價值（incremental predictive value），而且全程只使用賽前可知的資訊。本研究正好接在脈絡一（控制車隊）、脈絡三（分群）之後，補上脈絡二缺少的那一步。

## 對本研究設計的直接啟示

- Bell et al.（2016）發現車隊效果在雨天與街道賽較弱，對應本研究 `street_circuit`、`wet_race` 與假說二。
- van Kesteren & Bergkamp（2023）的 88% 是本研究「天真基準與車隊變數應該已經解釋大部分變異」的預期依據。
- Secco & Taylor（2026）的特徵（相對圈速、圈速穩定度、輪胎衰退）與本研究第一階段幾乎相同，可直接引用為特徵設計的先例；他們以隊內相對指標控制賽車，本研究沿用。
- Heilmeier et al.（2020）說明進站策略是賽中即時決策，支持本研究把進站變數改為「歷史進站傾向」的做法。

## 建議先讀順序

1. van Kesteren & Bergkamp（2023）：方法最接近、資料年份重疊、有開放全文。
2. Bell et al.（2016）：賽道類型與天氣的交互作用，有 White Rose 開放版本。
3. Secco & Taylor（2026）：第一階段特徵設計的直接先例。
4. Krzysztoń & Smołka（2026）：了解現有 ML 預測用了哪些特徵，確認「賽中資料」的批評是否成立。

各篇獨立筆記見同資料夾 `*.md`。
