# Can Unsupervised Learning Methods Reveal Distinct Driving Styles Among Formula 1 Drivers?

## Citation

Secco, E. L., & Taylor, A. (2026). Can Unsupervised Learning Methods Reveal Distinct Driving Styles Among Formula 1 Drivers? *International Journal of Modeling and Optimization*, 16(1–2). https://ojs.ejournal.net/index.php/ijmo/article/view/22205

## Relevance to My Work

- 和本研究第一階段幾乎同題：用 FastF1 的遙測與圈速資料建立相對圈速、圈速穩定度、輪胎衰退、超車效率等特徵，比較 K-Means、階層式（Ward）、DBSCAN、GMM，得到 3–5 群，K-Means k=4 與 Ward 最穩定。
- 作者明確指出「原始圈速主要反映賽車性能」，並以**隊內相對指標**部分控制。本研究第一階段的相對隊友設計有了先例。
- **缺口所在**：他們停在分群與描述，沒有把群標籤放進任何預測模型。本研究的假說三就是補這一步。
- 使用 FastF1，所以只有 2018 年以後；本研究用 Kaggle 圈速與進站資料可以回推到 2011 年，是資料面的差異。

## Summary

- **Main question**：非監督式方法能否從 F1 遙測與圈速資料分出可解釋的駕駛風格群？
- **Data**：FastF1 遙測與圈速。
- **Method**：特徵工程（相對圈速、穩定度、輪胎衰退、超車效率、分段平均）、PCA、四種分群法、silhouette 與 Davies-Bouldin 評估。
- **Result**：3–5 群；K-Means（k=4）與 Ward 階層式最佳。
- **Limitations**（作者自述）：遙測涵蓋不完整；賽車性能在原始圈速中占主導，限制分群的解釋力。

（依期刊頁面摘要整理。）
