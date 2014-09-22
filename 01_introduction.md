# Data Mining
## Chapter 1: Introduction
### Major Steps of Data Mining (Knowledge Discovery)
#### 1. Data Preprocessing
1. Data Integration: Combine multiple data sources
2. Data Cleaning: Remove noise and inconsistent data
3. Data Selection: Select task-relevant data
4. Data Transformation: Transform/consolidate selected data for further analysis

#### 2. Data Mining
Apply data mining & machine learning methods (classification, clustering) to extract patterns from data.

#### 3. Pattern Evaluation (Post Prcessing)
Evaluate and Identify truly interesting patterns

#### 4. Visualization
Present the mined patterns to users

### Data Sources
#### Flat Files
Textual format, apply DM algorithms to extract useful knowledge.

#### Relational Databases
Collection of tables, each of which has tuples. DBMS facilitate queries, but data mining aims to achieve deeper knowledge. (Usually several tables are joined to extract useful knowledge)

#### Data Warehouse
Repository of information, collection from multiple sources. Built to support On-Line Analysis Processing (OLAP), but DM is even deeper.

#### Transactional Database
Each record represents a transaction, e.g. a purchase.

#### Advanced Databases
* Object-Relational Databases
* Temporal/Time-Series Databases
	* Stock exchange, inventory control, temperature
* Spatial/Spatio-Temporal Databases
	* Geographic maps, computer-aided design, medial and satellite image
	* Raster or vector format
* Text Databases
	* Word description of objects
	* Strcutured (can be represented relational), Semistructured (XML), Unstructured
	* DM finds association with dictionaries, ontologies
* Multimedia Databases
	* Large size, image, audio, video data
	* DM constructs multimedia data cubes, pattern matching
* Heterogenus/Legacy Databases (connection different data sources)
* Data Streams
	* Only one scan, fast response time
* WWW

### Data Mining Tasks
#### Prediction Methods
##### Classification
Distinguish data instance between different classes based on attributes. Predict *categorial* target values as accurately as possible.

##### Regression
Predicting a *numeric or continuous* target value.

##### Outlier Detection
Detect significant deviations from the normal behavior (fraud detection, authentication...)

#### Descriptive Methods
##### Clustering
* Data points in the same cluster are more similar
* Data points in different clusters are less simular
* Proximity Measures:
	* Euclidean Distance for continuous attributes
	* Cosine Similatiry for document data

##### Association Rule Mining
Produce dependency rules which will **predict the occurance of an item** based on occurance of another item.

##### Sequence Pattern Mining
Given a set of objects with a timeline of events for each object, find rules that predict strong sequential dependencies among different events.

### Primitives Defining a Data Mining Task
#### 1. Task relevant Data
* Database or data warehouse name
* Database tables or data warehouse cubes
* Condition for data selection
* Relevant attributes or dimensions
* Data grouping criteria

#### 2. Types of Knowledge to be minde
* Characterization
* Discrimination
* Association
* Classification/prediction
* Clustering
* Outlier analysis

#### 3. Background Knowledge
* Schema hierarchy (Street < City < Provice or State < Country)
* Set-grouping Hierarchy (20-39 = young, 40-59 = middle aged)
* Operation-derived hierarchy (Login name < University < Country)
* Rule-based hierarchy

#### 4. Pattern Interestingness
* Simplicity (association rule length, decision tree size)
* Certainty
* Utility
* Novelty (not previously known, surprising)

#### 5. Presentation of Discovery
* Rules, Tables, Pie/Bar Chart etc.
* Concept hierarchy is important (level of Abstraction, interactivity)
* Different kinds of knowledge require different representations