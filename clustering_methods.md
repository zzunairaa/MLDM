# Clustering Methods Overview

This guide summarizes **KMeans**, **Agglomerative Clustering**, and **DBSCAN**, including their hyperparameters, step-by-step procedures, and tips for selecting the best clusters.  

---

## 1️ KMeans

**Hyperparameter:**  
- `n_clusters = k` (number of clusters)

**Procedure to find the best k:**

1. Test KMeans with `k` from 2 to 10 (or another reasonable range).  
2. For each k, compute:
   - **Inertia**: Sum of squared distances of points to their cluster centroid.
   - **Silhouette score**: Measures how well-separated clusters are, ranges from -1 to 1.
3. **Elbow method:** Plot inertia vs k → find the “elbow point” where adding more clusters **doesn’t reduce inertia much**.  
4. Check silhouette score → pick k with higher silhouette (ideally > 0.5).

**Best k = elbow + high silhouette**

---
## 2️ Agglomerative Clustering

**Hyperparameters:**  
- `n_clusters = k`  
- `linkage = ['ward', 'complete', 'average', 'single']`

**Procedure to find best k and linkage:**

1. Test all combinations of k (2–10) and linkage methods.  
2. For each combination, compute **silhouette score**.  
3. Pick the combination with **highest silhouette score**.  

 **Best k = number of clusters with highest silhouette score**, and linkage method is chosen along with it.

---

## 3️ DBSCAN

**Hyperparameters:**  
- `eps` = maximum distance between points in a cluster  
- `min_samples` = minimum points to form a dense cluster  

**Procedure to find best parameters:**

1. Test a **grid of eps and min_samples values**.  
2. For each combination, compute:
   - **Number of clusters formed**
   - **Silhouette score** (computed only on points assigned to clusters, ignore noise `-1`)
   - **Percentage of points labeled as noise** (want it low, ideally < 10%)  
3. Pick the combination where:
   - Silhouette score is **high** (good separation)  
   - Noise percentage is **low** (most points clustered)  
   - Number of clusters is reasonable (optional, e.g., similar to KMeans/AC)  

 **Best eps & min_samples → high silhouette + low noise**  

> Unlike KMeans, DBSCAN **doesn’t require the number of clusters in advance**.

---

### Key Differences Summary

| Method        | Hyperparameter to tune | How to pick best clusters                        |
| ------------- | ---------------------- | ------------------------------------------------ |
| KMeans        | `n_clusters = k`       | Elbow in inertia + high silhouette               |
| Agglomerative | `n_clusters + linkage` | Max silhouette                                   |
| DBSCAN        | `eps, min_samples`     | Max silhouette + low noise + reasonable clusters |

---

###  Tips for DBSCAN

- Too small `eps` → most points become noise → silhouette is undefined for them.  
- Too big `eps` → clusters merge → silhouette drops.  
- `min_samples` controls density → higher values require denser clusters.


