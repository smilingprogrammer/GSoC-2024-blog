---
layout: default
title: Week 4- The devil is "always" in the detail!
---

# Week 4 - The devil is "always" in the detail! 🔬

---

[python] Here is an article summarizing the tasks you accomplished this week, including clustering embeddings and generating textual predicates.

---

### Weekly Summary Report

#### Clustering Word Embeddings

This week, my primary focus was on clustering word embeddings to identify similar words within a dataset. The challenge was to determine the optimal number of clusters, \( k \), as this was not known beforehand. To address this, I employed four common methods: the Elbow Method, Silhouette Score, and Gap Statistic. Here's a detailed breakdown of each approach:

**1. Elbow Method:**
The Elbow Method involves running k-means clustering on the dataset for a range of \( k \) values and then plotting the within-cluster sum of squares (WCSS) against the number of clusters. The optimal \( k \) is identified at the 'elbow point' of the graph, where the WCSS starts to diminish at a slower rate. I implemented this method using the following steps:
- Converted the text data into TF-IDF features.
- Computed WCSS for different values of \( k \) and plotted the results to find the elbow point.

**2. Silhouette Score:**
The Silhouette Score measures how similar an object is to its own cluster compared to other clusters. A high value indicates well-defined clusters. I calculated the silhouette scores for different \( k \) values and plotted them to determine the optimal number of clusters.

**3. Gap Statistic:**
The Gap Statistic compares the total within intra-cluster variation for different numbers of clusters with their expected values under null reference distribution of the data. I used this method to statistically validate the optimal number of clusters.

After determining the optimal number of clusters using these methods, I performed k-means clustering on the word embeddings. Here’s the summarized code for the clustering process with the best output (Silhouette Score):

```python
def cluster_embeddings(embeddings):
    silhouette_scores = []
    for k in range(2, len(embeddings)):
        kmeans = KMeans(n_clusters=k, init='k-means++', max_iter=300, n_init=10, random_state=42)
        kmeans.fit(embeddings)
        score = silhouette_score(embeddings, kmeans.labels_)
        silhouette_scores.append(score)

    optimal_k = np.argmax(silhouette_scores) + 2

    kmeans = KMeans(n_clusters=optimal_k, init='k-means++', max_iter=300, n_init=10, random_state=42)
    kmeans.fit(embeddings)

    cluster_centers = kmeans.cluster_centers_
    cluster_labels = kmeans.labels_

    return cluster_centers, cluster_labels
```

The optimal number of clusters was determined, and the cluster labels were printed for further analysis.

#### Generating 100 Triples with Textual Predicates

In addition to clustering embeddings, I generated 100 triples with textual predicates that are not available in the DBpedia ontology. This task involved creating subject-predicate-object triples where the predicate is a textual description not found in the existing DBpedia ontology. The purpose was to enrich the ontology with new, meaningful relationships extracted from various textual sources.

Here’s an example of how these triples might look:

1. **Subject:** "Albert Einstein"  
   **Predicate:** "formulated the theory of"  
   **Object:** "relativity"

2. **Subject:** "Marie Curie"  
   **Predicate:** "won the Nobel Prize in"  
   **Object:** "Physics"

3. **Subject:** "Alan Turing"  
   **Predicate:** "developed the concept of"  
   **Object:** "a universal machine"

The generation of these triples involved:
- Extracting meaningful relationships from textual data.
- Ensuring the predicates were unique and not present in the existing DBpedia ontology.
- Structuring the triples in a standardized format for easy integration into the ontology.


---

This summary encapsulates the work done on clustering embeddings and generating new triples, highlighting the methodologies used and the outcomes achieved.




## Overview




## Findings and challenges 
This week we revisited the challenge we encountered last week, that is, taking control of the length of the generated sequences. The soloution seems to be lied within the proper use of special tokens, in particular, `EOS` (End Of Sequence) token. The preassumption was that the tokenizer *might* automatically add special tokens to the input sequence. However, we observed despite using the `tokenizer.encode_plus` method, EOS token is not presented within the encoded input sequence. The following code blocks refer to our experiemnts with the `bigcode/starcoderbase-3b` checkpoint. 




## Next week plan
For the upcoming week, our plan is to:
- Process existing datasets for creating our intended prompt-tuning dataset.
- Explore exisiting models that can be used for DBpedia entity resoloution. 
- Search for instruct-tuned versions of code generation models. 


----
[StarCoder models by BigCode]: https://huggingface.co/bigcode
[dedicated repo directory]: https://github.com/dbpedia/neural-qa/tree/gsoc-mehrzad/gsoc/mehrzad

