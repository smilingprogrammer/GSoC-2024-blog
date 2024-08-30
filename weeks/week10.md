---
layout: default
title: Week 10 Entity Linking
nav_exclude: true 
---

# Week 10-  Entity Linking

---

## Overview

This week

I apologize for the misunderstanding. You're right, let's create a blog post about the entity linking work we discussed. Here's a draft:

---

# Enhancing Entity Linking with GENRE and DBpedia

This week, we dove deep into the world of entity linking, specifically focusing on connecting textual names to their corresponding entities in the DBpedia ontology. Our journey involved leveraging the power of the GENRE (Generative Entity Retrieval) model and integrating it with DBpedia's vast knowledge base.

## The Challenge

Our task was to take a list of entity names and find their matching entities in the DBpedia ontology. This presents several challenges:
1. Handling variations in entity names
2. Disambiguating between similar entities
3. Efficiently querying a large knowledge base
4. Ensuring we get the most relevant DBpedia resource links

## Our Approach

We implemented a solution that combines the strengths of the GENRE model with DBpedia's SPARQL endpoint. Here's a breakdown of our approach:

1. **GENRE Model**: We used the "facebook/genre-linking-aidayago2" model, which is specifically trained for entity linking tasks. This model generates potential entity names based on the input.

2. **DBpedia Integration**: We set up a SPARQL endpoint to query DBpedia, allowing us to match the GENRE outputs with actual DBpedia entities.

3. **Smart Querying**: Our SPARQL query was designed to prioritize non-category resources and use flexible matching to catch variations in entity names.

4. **Fallback Mechanism**: In cases where GENRE doesn't produce useful results, we implemented a fallback to query DBpedia directly with the original entity name.

## Key Learnings

1. **Model Selection Matters**: Using a model specifically trained for entity linking (GENRE) significantly improved our results compared to more general language models.

2. **Knowledge Base Quirks**: We learned that DBpedia's structure can sometimes lead to unexpected results, such as category links instead of direct resource links. Our query had to be adjusted to handle these cases.

3. **Balancing Precision and Recall**: Tweaking the SPARQL query to use less strict matching helped catch entity variations (like "Paris Agreement"), but we had to be careful not to introduce false positives.

4. **Performance Considerations**: Querying external endpoints for each prediction can be slow. In a production environment, we'd need to consider caching mechanisms or local RDF stores for better performance.

## Results

We tested our system with a diverse set of entities:
- "Joseph R. Biden Jr."
- "Delaware"
- "Supreme Court of the United States"
- "2022 Russian invasion of Ukraine"
- "Paris Agreement"

Our approach successfully linked these to their corresponding DBpedia entities, even handling tricky cases like "Paris Agreement" which required some fuzzy matching.

## Next Steps

While our current implementation is functional, there's always room for improvement:
1. Implementing the optional trie-based constrained beam search for potentially improved accuracy.
2. Fine-tuning the GENRE model on DBpedia-specific data for better alignment.
3. Exploring more sophisticated disambiguation techniques for highly ambiguous entities.
