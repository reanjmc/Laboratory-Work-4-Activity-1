# Guide Questions — Answers Based on Your Results

## A. Model Evaluation Analysis
1. What were the weakest-performing classes based on the confusion matrix?
The weakest-performing classes were poppy (F1: 0.62), daisy (F1: 0.69), ixora (F1: 0.70), cosmos (F1: 0.72), and zinnia (F1: 0.73). These classes had the lowest F1-scores, meaning the model struggled most to correctly identify them.
2. How did Precision, Recall, and F1-score vary across classes?
There was noticeable variation across the 20 classes. Anthurium had the highest performance with precision, recall, and F1 all at 0.96, while poppy had the lowest F1 at 0.62. Some classes like orchid had high recall (0.92) but lower precision (0.66), meaning the model often predicted orchid when it wasn't. Others like cosmos had decent precision (0.85) but low recall (0.63), meaning many cosmos flowers were missed.
3. What does a low recall indicate in your model?
A low recall means the model is failing to detect many actual instances of that class — it produces a lot of false negatives. For example, poppy had a recall of only 0.54, meaning the model missed nearly half of all actual poppy images, likely misclassifying them as other flowers.
4. How does AUC score reflect model performance compared to accuracy?
The overall accuracy was 80%, while the AUC score was 0.9607. The high AUC indicates that the model is very good at ranking and distinguishing between classes across different probability thresholds, even when its hard predictions (accuracy) are not perfect. AUC is more reliable than accuracy because it accounts for class imbalance and probability calibration.

## B. Model Improvement
5. How did data augmentation affect validation accuracy?
The improved model used stronger augmentation (horizontal+vertical flip, rotation, zoom, contrast) compared to the baseline. However, the improved model's validation accuracy reached only about 47% after 20 epochs, which is actually lower than the baseline's 80%. This suggests the augmentation was too aggressive and the model needed more epochs or a better learning rate schedule to recover.
6. Why is Batch Normalization important in CNNs?
Batch Normalization normalizes the output of each layer during training, which stabilizes and speeds up learning. It reduces internal covariate shift, allows the use of higher learning rates, and acts as a mild regularizer. In your improved model, it was added after every Conv2D layer to help the deeper architecture train more consistently.
7. What role did Dropout play in improving your model?
Dropout randomly disables neurons during training, forcing the network to learn redundant representations and preventing it from relying too heavily on specific neurons. Your improved model used Dropout(0.4) after the conv layers and Dropout(0.5) after the dense layer, which helps reduce overfitting — especially important since your dataset has 20 classes with relatively few samples per class.
8. How did Early Stopping prevent overfitting?
Early Stopping monitors validation loss and stops training when it stops improving for a set number of epochs (patience=3). It also restores the best weights seen during training. This prevents the model from continuing to train after it starts memorizing the training data, ensuring the final model generalizes better to unseen data.

## C. Performance Comparison
9. What improvements were observed after modifying the model?
The improved model introduced BatchNormalization, stronger augmentation, a deeper architecture (up to 128 filters), a lower learning rate (0.0001), and Early Stopping. However, based on the training logs, the improved model's validation accuracy (47.29% at epoch 20) was lower than the baseline (80%). This indicates the improved model was still learning and likely needed more epochs or the augmentation was too strong for the dataset size.
10. Which enhancement contributed the most to performance improvement? Why?
Based on the results, the most impactful enhancement was Batch Normalization, as it stabilized training across the deeper architecture. However, the overly aggressive data augmentation (horizontal_and_vertical flip + contrast) likely hurt performance by making training images too different from validation images, slowing convergence significantly.
11. Did the gap between training and validation accuracy decrease? Explain.
In the improved model, training accuracy reached about 41.71% and validation accuracy 47.12% at epoch 20 — the validation was actually slightly higher than training, which is unusual and suggests the augmentation made training harder than validation. The gap was small (less than 6%), which technically shows less overfitting, but both values were much lower than the baseline, meaning the model was underfitting overall.

## D. Explainability (Grad-CAM Integration)
12. How did Grad-CAM help in understanding model predictions?
Grad-CAM visualized which regions of the input image most influenced the model's prediction by highlighting them as a heatmap. Instead of treating the model as a black box, it allowed us to see whether the model was focusing on the actual flower (petals, shape, color pattern) or on irrelevant background areas, giving insight into the model's decision-making process.
13. Did the improved model focus on more relevant regions? Provide evidence.
Based on the Grad-CAM overlay generated from the baseline model (which had 80% accuracy), the heatmap highlighted the central flower region, suggesting reasonable feature learning. Since the improved model underperformed in this run (47% val accuracy), it would likely show a more scattered or less focused heatmap, indicating weaker feature extraction due to insufficient training.
14. Why is explainability important in real-world AI applications?
In real-world applications — such as medical diagnosis, autonomous vehicles, or agricultural disease detection — it is not enough for a model to just give a correct answer. Stakeholders need to trust and verify the model's reasoning. Explainability tools like Grad-CAM help identify if a model is making decisions for the wrong reasons (e.g., focusing on image backgrounds instead of the actual object), which could lead to dangerous errors in high-stakes environments. It also helps developers debug and improve models more effectively.
