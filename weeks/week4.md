---
layout: default
title: Week 4- Entailment Technique for Predicate Suggestion
---

# Week 4 - Entailment Technique for Predicate Suggestion 🔬

---

## Overview

### Achieving Accurate Cluster Representation

This week has been a remarkable journey in refining a method to accurately determine representative words or phrases for clusters of similar terms. By leveraging the Entailment technique and deep learning models, I managed to enhance the process of identifying the most representative terms for each cluster. Here’s a detailed account of the progress made and the methods employed.

#### Initial Steps and Approach

The initial task involved determining the best strategies for representing clusters of words. Given a list of words and their respective clusters, the objective was to find the most representative term for each cluster. The words were as follows:

```python
words = ["large", "fat", "wide", "is going to vote for", "supports", "big", "endorses", "is very likely going to support"]
cluster_labels = [0, 0, 0, 1, 1, 0, 1, 1]
```

The preliminary approach involved using the Sentence Transformers model to generate embeddings for these words and then applying clustering algorithms like KMeans. This method initially yielded promising results, but refinement was necessary to improve the accuracy of the representative terms.

#### Employing the Entailment Technique with DeBERTaV2

To enhance the precision, I incorporated the entailment technique using the DeBERTaV2 model, which is fine-tuned for natural language inference (NLI). This method evaluates how well one sentence (or word) entails another, providing a robust metric for determining the representativeness of terms within clusters.

1. **Model Initialization**:
    ```python
    from transformers import DebertaV2TokenizerFast, DebertaV2ForSequenceClassification
    import torch

    class EntailmentModel:
        def __init__(self):
            self.tokenizer = DebertaV2TokenizerFast.from_pretrained("microsoft/deberta-v2-xxlarge-mnli")
            self.model = DebertaV2ForSequenceClassification.from_pretrained(
                "microsoft/deberta-v2-xxlarge-mnli", torch_dtype=torch.float16
            ).to('cuda', dtype=torch.float16)
    ```

2. **Inference and Scoring**:
    Using the model, I calculated entailment scores for pairs of words within each cluster. The scores indicate the likelihood that one word entails another, which helps in identifying the most central terms.

3. **Building a Directed Graph**:
    A directed graph was constructed where nodes represented words and directed edges (with weights) represented the entailment scores. This graph was then used to compute centrality measures.

4. **Transitive Closure**:
    To ensure comprehensive analysis, I computed the transitive closure of the graph. This step ensured that indirect relationships were considered, i.e., if A entails B and B entails C, then A entails C.

5. **Centrality Calculation**:
    Using in-degree centrality, I ranked the words to determine the most central (and hence representative) terms within each cluster.

#### Visualization and Results

To enhance understanding, I implemented the example code provided by the mentor to visualize the original and transitive closure graphs for each cluster. The visualization helped in understanding the relationships and centrality within the clusters.

```python
import networkx as nx
import matplotlib.pyplot as plt

def visualize_graph(G, title):
    fig, ax = plt.subplots()
    nx.draw_circular(G, ax=ax, with_labels=True, labels={n: n[2:-2] for n in G.nodes()},
                     node_color='#addfff', edge_color='gray', node_size=2000, arrowsize=20)
    ax.set_xlim(-2.0, 2.0)
    ax.set_ylim(-2.0, 2.0)
    plt.title(title)
    plt.show()
```

By visualizing the graphs, I identified the top two representative words for each cluster. For example, in one cluster, "supports" and "endorses" were identified as the top representatives, ensuring accurate representation of the cluster's essence.

#### Achievements and Learnings

This week’s efforts culminated in a robust method for identifying representative words within clusters using entailment techniques and graph-based analysis. The key achievements include:
- **Accurate Representation**: Enhanced the precision of identifying representative terms using entailment and centrality measures.
- **Transitive Closure**: Ensured comprehensive analysis by considering indirect relationships within clusters.
- **Visualization**: Provided clear visual representations of relationships within clusters, aiding in better understanding and validation of results.

Overall, this week has been a testament to the power of combining advanced NLP techniques with graph-based analysis to achieve precise and meaningful results in clustering and representation tasks.


## Findings and challenges


## Next week plan
For the upcoming week, our plan is to:
- Process existing datasets for creating our intended prompt-tuning dataset.
- Explore exisiting models that can be used for DBpedia entity resoloution. 
- Search for instruct-tuned versions of code generation models. 


----
[StarCoder models by BigCode]: https://huggingface.co/bigcode
[dedicated repo directory]: https://github.com/dbpedia/neural-qa/tree/gsoc-mehrzad/gsoc/mehrzad

