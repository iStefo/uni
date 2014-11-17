# Data Mining
## Chapter 9: Anomaly Detection
### Definitions
* Data points considerably different from remainder of data, also known as "outliers, exceptions, peculiarities, surprises" etc
* **Challenges**:
	* Defining a normal region
	* Validation
	* Inprecise border between normal and outlying
	* Availability of labeled data for training/test

### Applications
* **Insurance/Fraud detection** (credit card, insurance claim, cell phone fraud, insider trading)
	* Real-time detection
	* Misclassification cost is very high
* **Intrusion Detection**: monitoring events in computer systems or networks
	* Signature-based intrusion detection cannot detect emerging cyber threats
* **Healthcare Informatic**: indicate disease outbreak, intrumentation errors
	* Only normal labels available, complex data
	* Misclassification cost ist very high
* **Industrial Damage Detection**: detect failures in complex industrial systems, suspicious events in video surveilance, abnormal energy consumption etc.
	* Huge, noisy and unlabelled data
	* Most applications exhibit temporal behaviour
	* Events require immediate intervention
* **Image Processing & Video Surveillance**: outliers in images over time, anomalous regions withn an image
	* Detect collective anomalies
	* Data sets often very large

### Issues of Anomaly Detection
* **Input Data**
	* Complex Data Types: Relationship among instances, Sequential, Temporal, Spatial, Spatio-Temporal, Graph
* **Data Labels**
	* Supervised Anomaly Detection: Labels always available, like class mining
	* Semi-supervised Anomaly Detection: Labels available for normal data
	* Unsupervised Anomaly Detection: No labels assumed
* **Types of Anomalies**
	* **Point Anomalies**: Individual data instance is anomalous w.r.t the data
	* **Contextual Anomalies**: ...within a context, requires notion of context
	* **Collective Anomalies**: collection of related data is anomalous, requires relationship
* **Output of Anomaly Detection**
	* Label: Each test instance is given normal/anomaly label
	* Score: Anomaly score

### Anomaly Detection Techniques
* Build profile of "normal" behavior
* Use the "normal" profile to detect anomalies
* Graphical based
* Statistical based
	* Most tests are for a single attribute only
	* Data distribution may not be known or hard to estimate
* Nearest Neighbor based
	* No assumptions about data distribution
	* For insufficient number of neighbors, method may fail (-> high dimensional data)
	* Expensive to compute
* Distance, Proximity or Density
	* Nearest Neighbor (NN) approach
	* Local Outlier Factor
* Clustering based
	* Unsupervised method, suitable for incremental/on-line detection from temporal data
	* Computationally expensive, fails if normal points do not create any clusters, sparseness
* Classficiation based
	* High accuracy, models can be understood easily
	* Require labels for normal and anomaly class, can't detect unknown anomalies

### Evaluation of Anomaly Detection
* Accuracy is not sufficient (a trivial classifier would have 99.9% accuracy)
* F-Measure: `2*R*P/(R+P)`, Recall `R = TP/(TP + FN)`, Precision `P = TP/(TP + FP)`

### Base Rate Fallacy