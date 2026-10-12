# Virtual strategy engineer: Using artificial neural networks for making race strategy decisions in circuit motorsport

## Citation

Heilmeier, A., Thomaser, A., Graf, M., & Betz, J. (2020). Virtual strategy engineer: Using artificial neural networks for making race strategy decisions in circuit motorsport. *Applied Sciences*, 10(21), 7805. https://doi.org/10.3390/app10217805

## Relevance to My Work

- 說明進站時機與輪胎配方是車隊在比賽**進行中**依局勢做的決策，不是賽前就決定好的。這支持本研究把 `n_pit_stops`、`avg_pit_duration` 改成「過去 5 場的歷史進站傾向」，而不是當場數值。
- 訓練資料為 2014–2019 年計時資料，與本研究樣本重疊。
- 屬於策略模擬文獻，不是名次預測，引用時定位為補充。

## Summary

- **Goal**：讓賽事模擬器自動替所有車手決定策略，不必人工輸入。
- **Method**：兩個神經網路，一個決定本圈是否進站，一個決定換哪種配方；整合進 FAST 賽事模擬器。
- **Result**：決策合理且會隨比賽情境改變；在範例比賽中結果接近真實。
- **Related**：Tulabandhula & Rudin (2014) 的賽中預測；後續強化學習策略研究。

（依出版社與 TUM 頁面摘要整理。）
