# Elastic Certified Engineer Exam
## Nodes, Shards, Replicas and Cluster 

| Term | Example |
| ----------- | ----------- |
| **Node** | A single server in Elasticsearch that stores data and handles search and indexing operations. |
| **Index** | A collection of documents that share the same structure and are stored together. |
| **Shard** | A smaller portion of the index; Elasticsearch divides an index into shards for scalability. |
| **Replica** | A copy of a shard; ensures data availability and improves search performance. |
| **Cluster** | A group of nodes working together to manage and store data as one unit. |

### How Elasticsearch handles nodes, shards, replicas
**Example 1:**

![](https://raw.githubusercontent.com/cryptocean22/ELK/refs/heads/main/Certifications/ELastic-Certified-Engineer/Pictures/Bildschirmfoto%202024-12-07%20um%2020.49.05.png)

```JSON
PUT testindex {
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
```
- Total shards are calculated as:
  - `Total Shards = Primary Shards x (1 + Number of Replicas)`
  - `Total Shards = 1 x (1 + 0) = 1`

- There is one node in this example. A node is a single instance of Elasticsearch.
- Each index in Elasticsearch is divided into primary shards to distribute data for scalability.
  - Here, there is 1 primary shard (P1) for the index _testindex_.
- Replicas are copies of primary shards for redundancy and fault tolerance.
  - In this example, the number of replicas is set to 0, meaning no replica shard exists.
- The index contains only the primary shard (P1), with no replicas. All data resides on a single node.
- The configuration uses the PUT request to define the number of primary shards (number_of_shards: 1) and replicas (number_of_replicas: 0):


**Example 2:**
![](https://github.com/cryptocean22/ELK/blob/main/Certifications/ELastic-Certified-Engineer/Pictures/Bildschirmfoto%202024-12-07%20um%2020.49.13.png?raw=true)

```JSON
PUT testindex2 {
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
```
- Total shards are calculated as:
  - `Total Shards = Primary Shards x (1 + Number of Replicas)`
  - `Total Shards = 2 x (1 + 1) = 4`

- The setup has 2 nodes, meaning two Elasticsearch instances that can store and process data.
- The index (testindex2) is divided into 2 primary shards (P1 and P2), ensuring that the data is distributed across nodes.
- Each primary shard has 1 replica (P1R1 for P1 and P2R1 for P2).
  - Replicas are placed on a different node from their corresponding primary shard to ensure fault tolerance.
- Node 1: Stores P1 (primary shard) and P2R1 (replica shard of P2).
- Node 2: Stores P2 (primary shard) and P1R1 (replica shard of P1).


**Example 3:**
![](https://github.com/cryptocean22/ELK/blob/main/Certifications/ELastic-Certified-Engineer/Pictures/Bildschirmfoto%202024-12-07%20um%2020.49.30.png?raw=true)

```JSON
PUT testindex2 {
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  }
}
```
- Total shards are calculated as:
  - `Total Shards = Primary Shards x (1 + Number of Replicas)`
  - `Total Shards = 3 x (1 + 2) = 9`

- There are 3 nodes, meaning three Elasticsearch instances. The shards are distributed across these nodes.
- Each primary shard has 2 replicas. For example:
  - P1 has replicas P1R1 and P1R2.
	- P2 has replicas P2R1 and P2R2.
	- P3 has replicas P3R1 and P3R2.
- Elasticsearch automatically distributes primary and replica shards across nodes for fault tolerance and performance.
- Each node holds a mix of primary and replica shards. For example:
  - Node 1: Holds P1, P2R1, P3R1.
	- Node 2: Holds P2, P1R1, P3R2.
	- Node 3: Holds P3, P1R2, P2R2.





