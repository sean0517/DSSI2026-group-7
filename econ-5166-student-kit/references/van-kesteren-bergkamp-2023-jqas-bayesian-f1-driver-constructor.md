# Bayesian analysis of Formula One race results: disentangling driver skill and constructor advantage

## Citation

van Kesteren, E.-J., & Bergkamp, T. (2023). Bayesian analysis of Formula One race results: disentangling driver skill and constructor advantage. *Journal of Quantitative Analysis in Sports*, 19(4), 273–293. 開放版本：arXiv:2203.08489（https://arxiv.org/abs/2203.08489）；PMC10660124。

## Relevance to My Work

- 用 2014–2021 年（油電時代）資料，和本研究樣本重疊。
- 以貝氏多層次 rank-ordered logit 模型拆解名次變異，約 88% 由車隊解釋。這是本研究「賽車性能主導、車手特質只能提供邊際貢獻」的量化依據，也是假說三預期「提升有限」的理由。
- 他們的目標是事後評價車手與車隊能力，不是賽前預測，也沒有描述駕駛風格。本研究接在這之後：控制車隊之後，風格群還有沒有預測力。

## Summary

- **Main question**：如何從比賽名次中分離車手能力與車隊優勢。
- **Model / Setup**：每場比賽的完賽順序視為一次排序，以 rank-ordered logit 建模；車手與車隊（含車隊-年）為隨機效果，貝氏估計。
- **Result**：Hamilton 與 Verstappen 為油電時代最佳車手；Mercedes、Ferrari、Red Bull 明顯優於其他車隊；車隊解釋約 88% 的變異。
- **Limitations（本組判讀）**：只用完賽順序，不含圈速、進站等行為資料；不處理賽前可用資訊的限制。
- **Related Papers**：Phillips (2014) 用調整後積分處理各季參賽人數不同；Bell et al. (2016) 多層次模型。

（依摘要與 PMC 全文頁整理，尚未逐段閱讀。）
