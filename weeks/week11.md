---
layout: default
title: Week 1- Entity Linking with vector search
---

# Week 1 - Entity Linking with vector search 🛣️

---

Our objective this week was to research about the use of sentence transformers embeddings if it could efficiently search and find similar entities based on textual entity queries. To try out this method, we decided to focus on a specific class of footballers (Known as soccer players in the DBpedia KG). This involved creating embeddings for soccer player names and implementing a vector search that could leverage these embeddings.

### Performance

We Initially tried out the model with the best benchmark available on the [sentence transformer chart], the [all-mpnet-base-v2] which was fine tuned on microsoft pretrained [mpnet-base] model. Despite being the model with the best benchmark on various scores; it was really slow on inference, and the result wasn't good. It was unable to predict the full name of the two most popular Football players currently on the plannet below were the result;

`[In]`
```python
query = "Lionel Andres Messi Cuccittini"
```
`[Out]`
```
label	            score
Nicolás_Messiniti	0.760293
```
`[In]`
```python
query = "Cristiano Ronaldo dos Santos Aveiro"
```
`[Out]`
```
label	              score
Cristiano_da_Silva  0.823177
```

Then we decide to revert back to the model we used for our predicate vector search which is the [paraphrase-MiniLM-L6-v2]. This model, despite not being on the top benchmark chart scores of sentence transformer, it beats the previous model which was considered the best. The model performed really well based on the examples given. We tested the model on various examples of football players based on popularity (Lionel Messi, Cristiano Ronaldo e.t.c) using their full nama, based on non-popularity (e.g Victor Boniface, Papu Gomez, Victor Osimhen). The output gave us the exact entity we were looking for, Below is an example;

`[In]`
```python
query = "Lionel Andres Messi Cuccittini"
```
`[Out]`
```
label	        score
Lionel_Messi	0.764104
```
`[In]`
```python
query = "Cristiano Ronaldo dos Santos Aveiro"
```
`[Out]`
```
label	             score
Cristiano_Ronaldo  0.860301
```
`[In]`
```python
query = "Victor James Osimhen"
```
`[Out]`
```
label	          score
Victor_Osimhen	0.944162
```
`[In]`
```python
query = "Victor Okoh Boniface"
```
`[Out]`
```
label	            score
Victor_Boniface	  0.921432
```
```python
query = "Papu Gomez"
```
`[Out]`
```
label	            score
Papu_Gómez	      1.000000
```

We then decided to test the model based on recency to capture players who haven't started playing football before the model was trained, so as to fully decide if the model can capture outside of its training data, the output were really impressive. Below are the example of new players we tested it on;
`[In]`
```python
query = "Lamine Yamal"
```
`[Out]`
```
label	          score
Lamine_Yamal	  1.000000
```
`[In]`
```python
query = "Dani Olmo"
```
`[Out]`
```
label	          score
Dani_Olmo	      1.000000
```

### Memory
We computed the embeddings for both the uri and the labels only and there weren't much differnces between them. The embeddings with uri consumes around 973mb of data storage, while the embeddings with labels consumes around 966mb of data storage. 



----
[sentence transformer chart]: https://www.sbert.net/docs/sentence_transformer/pretrained_models.html
[all-mpnet-base-v2]: https://huggingface.co/sentence-transformers/all-mpnet-base-v2
[mpnet-base]: https://huggingface.co/microsoft/mpnet-base
[paraphrase-MiniLM-L6-v2]: https://huggingface.co/sentence-transformers/paraphrase-MiniLM-L6-v2
