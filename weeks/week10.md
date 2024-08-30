---
layout: default
title: Week 10 Entity Linking
---

# Week 10-  Entity Linking

---

## Overview

This week, we dove deep into the world of entity linking, specifically focusing on connecting textual names to their corresponding entities in the DBpedia ontology. Our journey involved leveraging the power of a populated redis database and also GENRE (Generative Entity Retrieval) model and integrating it with DBpedia's vast knowledge base.

## The Challenge

Our task was to take a list of entity names and find their matching entities in the DBpedia ontology. This presents several challenges:
1. Handling variations in entity names
2. Disambiguating between similar entities
3. Efficiently querying the DBpedia knowledge base
4. Ensuring we get the most relevant DBpedia resource links

## Approach

We implemented a solution that combines the strengths of the GENRE model with DBpedia's SPARQL endpoint:

1. **GENRE Model**: We used the [genre-linking-aidayago2] model, which is specifically trained for entity linking tasks. This model generates potential entity names based on the input.

2. **DBpedia Integration**: We set up a SPARQL endpoint to query DBpedia, allowing us to match the GENRE outputs with actual DBpedia entities.


## Considerations

1. **Model Selection Matters**: The previous model used for linking entities in the Neural Extraction Framework was also a GENRE model varianst ([genre-linking-blink]), However the output wasn't really great, therefore we had to use another variants of this GENRE model that performed better, [genre-linking-aidayago2]. Using this model specifically trained for robust entity linking (GENRE) significantly improved our results compared to more general language models.

4. **Performance Considerations**: Querying external endpoints for each prediction can be slow. In a production environment, we'd need to consider other methods.

## Results

We tested our system with a diverse set of entities:
- "Joseph R. Biden Jr."
- "Delaware"
- "Supreme Court of the United States"
- "2022 Russian invasion of Ukraine"
- "Paris Agreement"

Our approach successfully linked most of these to their corresponding DBpedia entities, However entities like "Paris Agreement", which is valid, doesn't get a good Wikipedia entity match. It get mapped to entities like;
```
['Treaty on the Final Settlement with Respect to Iraq',
 'Treaty on the Final Settlement with Respect to Palestine',
 'Treaty on the Final Settlement with Respect to Yugoslavia',
 'Treaty on the Final Settlement with Respect to Paris',
 'Treaty on the Final Settlement with Respect to Morocco']
```
And this is due to the Entity linking model we used. Although we can improve this by implementing a fuzzy search algorithm. However to link each entity to its corresponding wikipedia and dbpedia match consumes a lot of time.

## Next Steps

1. Implementing the redis database for more accuracy and faster inference.
3. Making the texts match its corresponding entity based on the context it was used.

----
[genre-linking-aidayago2]: https://huggingface.co/facebook/genre-linking-aidayago2
[genre-linking-blink]: https://huggingface.co/facebook/genre-linking-blink
