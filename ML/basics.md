# basics

- [StatQuest Video Index](https://statquest.org/video-index/)

## ROC & AUC

- [StatQuest Video](https://youtu.be/4jRBRDbJemM)
- Sensitivity = TP / (TP + FN)
- Specificity = TN / (TN + FP)
- If correctly identifying positives is the most important thing to do with the data, we should choose a method with higher Sensitivity.
- Precision vs. False Positive Rate: If there were lots of samples that were not obese relative to the number of obese samples, then Precision might be more useful than the False Positive Rate. This is because Precision does not include the number of True Negatives in its calculation, and is not effected by the imbalance. In practice, this sort of imbalance occurs when studying a rare disease. In this case, the study will contain many more people without the disease than with the disease.
- __ROC__ curves make it easy to identify the best threshold for making a decision.
- __AUC__ can help you decide which categorization method is better.
