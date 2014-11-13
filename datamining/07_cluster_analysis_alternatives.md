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
(p. 22)

### Cluster Validity