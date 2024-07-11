---
layout: default
title: Week 3- Predicate Suggestion
---

# Week 3 - Predicate Suggestion 💉  

---

## Overview

In the third week of my GSoC journey, our primary focus was to improve the efficiency of the similarity algorithm and improving the result of the algorithm by making predicate suggestion for a relation/phrase that do not presently have a corresponding predicate in the ontology. We started by performing pre-computation for the the dbpedia predicates embeddings and then proceed to clustering the computed embeddings into 10 central vectors. This approach significantly reduces the computational load by leveraging clustering, making semantic search more efficient while dynamically updating the ontology with new predicates.

We then proceed into making our pipeline perform the suggestion of a predicate given a scenerio where no close cluster or matching group predicate is found for the provided relation/phrase. We achieve this by computing the embeddings of the different vocabulary and finding the most related/similar word for the provided relation/phrase.


## Findings and challenges 
Through the process of clustering our computed embeddings, we learned that some of the phrases provided for the algorithm couldn't find a closer cluster to provide a similar/matching predicate given a threshold of 0.5. This is due to the current sentence transformer model `paraphrase-MiniLM-L6-v2` we are making use of to compute our embeddings. However using a model like `all-mpnet-base-v2` it drastically improve but the negative effect is that it takes much longer time to compute the embeddings. However i believe this time efficiency problem can be solved by uploading the precomputed embeddings directly into the pipelie, which reduces the time of computation anytime the pipeline is being used.

Furthermore, while trying to write the algorithm to compute the suggestion of predicate provided a similar/matching predicate isn't found for the available relation, we tried out two vocabularies (Words and the WordNet vocabulary). Trying out the two, the Word Net vocabulary stands out as it aligns with how some ontology property are derived, the word net sometimes provides the joining of words as a suggested predicate. Example;

```
> relation/phrase = "Date of birth"
> WordNet Result ==> "birth_date"
```
From the example above, Word Net provides the joining of two different words together `birth` and `date` which we can convert into our own taste of `birthDate`, the word net can also suggest a single word as a predicate if it is the most similar. Therefore this gives us more confidence of getting the best predicate suggestion on the provided relation. However, we are also facing the problem of getting the best accuracy for our provided phrase due to the current accuracy sentence transformer model used to compute our embeddings.

As stated above the significant challenge we encountered this week was getting the best pertained sentence transformer model to compute our embeddings for better accuracies. Using the the present cluster similarity search algorithm without threshold, we are able to get a similar predicate (even if it doesn't relate to the phrase given), but given a threshold of 0.5, we get if the are closer cluster if the similarities are higher or returns nothing if the similarity are lower;
For example, as can be seen from below snippet; when we provided an example "offspring of", without a given threshold, the `parent` predicate is returned, but given a threshold of 0.5, we get `No close cluster found for predicate '{pred}' with similarity above {threshold}.`. This can be improved using models with better accuracies;

`In:`
```
def dbpedia_similarity_search(pred, gensim_model, sent_tran_model, tbox, cluster_centers, embeddings, cluster_labels, threshold = 0.5):
    print("Predicting:", pred)
    pred_embedding = sent_tran_model.encode([pred], show_progress_bar=False)[0]

    pred_embedding = pred_embedding.reshape(-1)

    similarities = gensim_model.cosine_similarities(pred_embedding, cluster_centers)
    closest_cluster_idx = np.argmax(similarities)
    closest_similarity = similarities[closest_cluster_idx]

    ####****With Threshold****####

    # if closest_similarity < threshold:
    #   print(f"No close cluster found for predicate '{pred}' with similarity above {threshold}.")
    #   return ""


    print(f"Closest cluster index: {closest_cluster_idx}")
    print(f"Similarity to closest cluster: {closest_similarity}")

    group_indices = np.where(cluster_labels == closest_cluster_idx)[0]
    group_embeddings = [embeddings[i] for i in group_indices]
    group_labels = [labels[i] for i in group_indices]

    result = gensim_model.most_similar(positive=[pred_embedding], topn=1)
    out = []

    for label, score in result:
        out.append({'label': label.replace('_', ' '), 'score': score})
    df = pd.DataFrame(out)
    df.insert(0, 'URIs', df['label'].map(lambda x: to_uri(x, tbox=tbox)))
    return df

pred = "Shared border with"
db_pred = dbpedia_similarity_search(pred, gensim_model, encoder_model, tbox, cluster_centers, embeddings, cluster_labels)
print(db_pred)

```

`Out:`
```
### Without a threshold ###
Predicting: Shared border with
Closest cluster index: 3
Similarity to closest cluster: 0.2848406136035919
                                   URIs   label     score
0  [http://dbpedia.org/ontology/border]  border  0.759235

### Without a threshold ###

No close cluster found for predicate Shared border with with similarity above 0.5.
```

More examples of our week 3 experiments will be uploaded here in the [dedicated repo directory]


## Improvement

As we proceed, our plan will be to try out various pretrained model or technique to compute our embeddings in order to improve the accuracy of our pipeline.

----
[dedicated repo directory]: https://github.com/dbpedia/neural-extraction-framework/tree/abdulsobur-gsoc-2024
