# Data Mining
## Chapter 7: Cluster Analysis – Alternative Methods
### DBSCAN
* Density based algorithm (Number of points within a specified radius (**Eps**))
* **Core Point**: Point with more than **MinPts** within **Eps**
* **Border Point**: Point with fewer than **MinPts** but neighborhood of a core point
* **Noise Point**: Every other point.
* Works well:
	* Resistant to noise
	* Clusters of different size and shape
* Doesn't work well:
	* Varying densities
	* High-dimensional data

#### Determining EPS and MinPts
* Idea: K-th nearest neighbors are at roughly the same distance
* Noise points have the K-th nearest neighbor ar farther distance
* Plot sorted distance of every point to its K-th nearest neighbor

### CURE
* Hierarchical Approach
* Uses a number of points to represent a cluster
* Representatives are found by selecting a constant number of points from cluster and *shrinking* them toward the center of the cluster
* Cluster similarity is the smiliarity of the closest pair of representative points from different clusters
* Avoids problems with noise and outliers
* Better at handling arbitrary shapes and sizes

### Graph-Based Clustering
* ![07_graph_based](img/07_graph_based.png)
* Initially fully connected proximity graph
* Starting with MIN (single-link) and MAX (complete-link)
* Clusters are connected components in the graph

#### Sparsification
* Reduce amount of data to be handled drastically (more than 99%)
* Keep connections to similar nodes, break connection to less similar points
* Nearest neighbor might belong to same class as the point itself (less impact of noise)

### Chameleon
* Adapt to characteristics of data set to find natural clusters
* Dynamic model for similarity between clusters:
	* **Relative closeness** (absolute closeness normalized by internal closeness)
	* **Relative interconnectivity** (absolute interconnectivity of two clusters normalized by the internal connectivity of cluster)
	* Combine clusters that share certain properties
	* Merging preserves self-similarity
	* Works with spatial data sets (density, shapes, difference in densities, existance of special artifacts)
	
#### Steps
* Preprocessing: Present data by a k-nearest-neighbor graph
* Phase 1: Multilevel graph partitioning algorithm to find large number of clusters
* Phase 2: Hierarchical Agglomerative Clustering to merge sub-clusters

#### Shared Near Neighbor Approach
**SNN graph**: weight of edge = number of shared neighbors between vertices (when they are connected)

1. Compute the similarity matrix
2. Sparsify the smiliraity matrix by keeping only k most smiliar neighbors
3. Construct the shared neares neighbor graph from sparsified matrix
4. Find SNN density of each Point (using Eps, count the number of nodes with similarity > Eps)
5. Find the core points (using MinPts, find points with SNN density > MinPts)
6. Form clusters from core points (two points are in one cluster if distance < Eps)
7. Discard all noise points
8. Assign all non-noise, non-core points to cluster

* Does not cluster all points
* High complexity (`O(N^2)`)
