# Liver-Segmentation
The project builds on prior work, incorporating more sophisticated methods to improve liver segmentation results. While post-processing enhances the image, it’s insufficient without robust preprocessing — which together ensure better region separation and minimize the loss of meaningful information.

The workflow starts with the **Watershed algorithm**, a morphology-based technique that segments images into distinct catchment basins. Two strategies are applied:

- **h-minima transformation**: Reduces the number of local minima.
- **Marker imposition**: Uses labeled regions to guide segmentation.

These methods are evaluated for their effectiveness in reducing noise, preserving liver boundaries, and minimizing false detections.

---

## 2️⃣ Results and Analysis

### 2.1 Watershed Without Markers

- Segmentation was performed in 3D (axial, coronal, and sagittal views), resulting in **6530**, **2249**, and **1742** regions respectively.
- Without markers, the algorithm suffered from **oversegmentation**, detecting excessive irrelevant intensity variations.
- Watershed lines were formed at the intersection of catchment basins during the flooding process, often leading to fragmented and unclear organ boundaries.

### 2.2 Watershed With h-minima Markers

- The **h-minima transform** reduced oversegmentation by filtering out shallow intensity valleys.
- A threshold of 5% of the intensity range was used to suppress insignificant minima.
- **Connected components** from h-minima were used as markers for better organ boundary localization.
- Despite improvements, segment count remained high. Hence, additional strategies were tested:
  - **Region growing** was considered but discarded due to intensity similarity with surrounding organs (e.g., pancreas).
  - The **largest connected component** method was used to focus on the largest structure post-thresholding.
  - **A priori thresholding**: Combined a high (95%) and low (50%) threshold to exclude artifacts and non-liver tissues.

- **Ground Truth Comparison** showed:
  - Watershed had a **Jaccard index of 0.651**, but subsegmentation persisted.
  - **Morphological gradients** helped in better boundary detection.
  - **Nearest Neighbor (NN) classifier with KDTree** was used to label seed points based on intensity histograms from 3D neighborhoods, improving organ identification.

### 2.3 Validation and Testing

- Hyperparameters such as **threshold ranges** and **structuring element size** were tuned.
- Best results in validation were achieved with:
  - Max threshold: **0.90–0.95**
  - Min threshold: **0.67–0.69**
- Validation Jaccard index exceeded **0.50**.
- Testing results:
  - Jaccard dropped to **0.29**, indicating possible **overfitting** to the validation set.
  - False positives and under-segmentation were observed, particularly in lower axial slices.
  - Suggested improvement: balance generalization and threshold specificity.

---

## 3️⃣ Conclusions

- The method performs consistently across different views but is affected by **gradient sensitivity** and **volume anisotropy**.
- **Proper thresholding** is essential for accuracy.
- **Marker-based Watershed** (with h-minima) significantly improves segmentation quality.
- The **largest connected component** strategy helps filter noise and maintain spatial coherence.
- Combining classical methods with **machine learning (NN classifier)** enhances performance but relies on well-labeled seeds.
- Avoiding overfitting during validation is critical for real-world performance.

---
