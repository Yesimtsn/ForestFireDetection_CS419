# Unsupervised Forest Fire Detection Using Multispectral Sentinel-2 Imagery

This repository contains the implementation of an unsupervised wildfire detection framework developed as part of a computer science project at **Sabancı University**. The project leverages multispectral **Sentinel-2 imagery** and specialized spectral indices to identify fire-affected regions without requiring pixel-level ground truth annotations.

---

## Project Overview

Accurate identification of fire-affected regions is essential for disaster response and environmental monitoring. While supervised learning techniques are common, they require costly pixel-level labeled datasets that are often unavailable at scale.

Our approach addresses these challenges by:
* **Leveraging Multispectral Power:** Utilizing Near-Infrared (NIR) and Shortwave-Infrared (SWIR) bands to capture fire-induced changes in vegetation structure and moisture.
* **NBR-Based Feature Engineering:** Focusing on the **Normalized Burn Ratio (NBR)** to enhance the separability and spatial coherence of burned regions in an unsupervised setting.
* **Event-Based Data Selection:** Using active fire detections from NASA's **FIRMS** system to identify wildfire events and guide data selection.

---

## Key Features

* **Unsupervised Framework:** Exploits physical spectral characteristics without relying on dense, manual ground truth.
* **Diverse Segmentation Strategies:** Evaluates multiple strategies including fixed NBR thresholding, adaptive percentile thresholding, and K-Means clustering.
* **Automated Preprocessing:** Features a pipeline for cloud and cloud-shadow masking using the Sentinel-2 Scene Classification Layer (SCL).
* **"Silver-Standard" Evaluation:** Employs ground truth masks derived from the **Difference Normalized Burn Ratio (dNBR)** for proxy quantitative comparisons.

---

## System Architecture

The pipeline consists of five primary stages:



1. **Event Selection:** Wildfire events are identified and grouped using NASA FIRMS point-based detections.
2. **Sentinel-2 Retrieval:** Corresponding scenes are retrieved via the Sentinel Hub STAC catalog using event-based bounding boxes.
3. **Preprocessing:** Cloud masking and spatial alignment are performed to ensure input consistency.
4. **Feature Engineering:** Calculation of spectral indices, primarily the Normalized Burn Ratio (NBR).
5. **Unsupervised Segmentation:** Rule-based or clustering-based methods are applied to delineate the final burned area output.

---

## Spectral Methodology

The core of the detection logic relies on the **Normalized Burn Ratio (NBR)**, which exploits the contrast between NIR and SWIR bands.

$$NBR = \frac{NIR - SWIR}{NIR + SWIR}$$

* **Healthy Vegetation:** Strong NIR reflectance and low SWIR reflectance result in higher NBR values.
* **Burned Areas:** Characterized by vegetation moisture loss and the presence of char, leading to lower or negative NBR values.

---

## Results and Findings

| Method | F1 (Mean) | IoU (Mean) | Precision (Mean) | Recall (Mean) |
| :--- | :--- | :--- | :--- | :--- |
| **NBR_binary** | 0.4601 | 0.3565 | 0.6575 | 0.4853 |
| **KMeans (k=3)** | 0.2913 | 0.2039 | 0.4747 | 0.5775 |
| **NDVI_0.2** | 0.2304 | 0.1540 | 0.2422 | 0.6672 |
| **SWIR_p90** | 0.0962 | 0.0535 | 0.1377 | 0.2122 |

**Key Insight:** NBR-based methods consistently demonstrated superior separability and spatial coherence compared to single-band or vegetation-only approaches. While fixed thresholds offer transparency, data-driven K-Means clustering adapts better to scene-specific distributions.

---

## Contributors

* **Yeşim Tosun:** Segmentation strategy identification, qualitative analysis, and interpretation of results.
* **İlke Demirkır:** Dataset construction and preprocessing pipeline development.
* **Doruk Yeşil:** Feature engineering, implementation of segmentation methods, and experimental evaluation.

