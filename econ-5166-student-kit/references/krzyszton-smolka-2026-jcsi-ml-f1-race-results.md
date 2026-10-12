# Application of machine learning for predicting Formula 1 race results

## Citation

Krzysztoń, S., & Smołka, J. (2026). Application of machine learning for predicting Formula 1 race results. *Journal of Computer Sciences Institute*, 38, 81–86. https://doi.org/10.35784/jcsi.8462

## Relevance to My Work

- 脈絡二（直接預測名次）的近期代表。模型組合（SVM、Gradient Boosting、Random Forest）與本研究第二階段相近。
- 作者把預測品質歸因於「遙測資料的顯著影響」，表示特徵包含賽中資訊。這正是本研究要避免的：我們只用賽前可知資訊，並檢驗單一類特徵（風格群）的邊際貢獻，而不是追求總體準確率。
- 期刊層級不高，引用時定位為「近期應用範例」，不是方法權威。

## Summary

- **Target**：比賽結果分類。
- **Method**：SVM、Gradient Boosting、Random Forest 集成，Optuna 調參。
- **Result**：驗證集 macro-F1 約 77.8%、準確率約 80.2%。
- **Limitations（本組判讀）**：頁面未列出特徵重要性；未區分賽前與賽中資訊；未檢驗某類特徵的增量價值。

（依期刊頁面摘要整理。）
