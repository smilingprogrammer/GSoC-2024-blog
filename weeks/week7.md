---
layout: default
title: Week 7- Finding Unique Textual Predicate
---

# Week 7 - Finding Unique Textual Predicate

---

## Overview

This week we focused on generating unique and meaningful triples using the NEF (Neural Extraction Framework). The process involved running Wikipedia abstracts through the NEF to extract triples, then carefully selecting those whose predicates (or similar ones) could not be found in DBpedia. 


### A Week of Challenges and Discoveries in Ontology and Predicate Extraction

Wow, what a week it has been—one of the most "time-taking" since the start of this program! This week was all about dives into generating triples with unique predicate by ontology matching and predicate extraction, which required precision, and a touch of exploration. The goal was to generate triples with unique textual predicates using the NEF (Neural Extraction Framework) from various Wikipedia abstracts. What I didn’t anticipate was just how repetitive (predicates) and similar the results would be across different abstracts.

#### The Challenge of Ontology Matching

Initially, I began with focusing on running various Wikipedia abstracts through the REBEL model, then discover it doesn't totally cover our goal. Then, i proceed with  running various Wikipedia abstracts through the NEF (Neural Extraction Framework). The framework would output triples, and my task was to select those triples where the predicate (or a similar one) could not be found in DBpedia. While this might sound simple, it turned out to be quite rigid. Ensuring that the selected predicates were not just unique in wording but also in meaning required some attention. This process also involved verifying that the relationships expressed in the triples were novel and meaningful which can be determined by the confidence score.

#### The Struggle for Unique Predicates

A significant part of the challenge was generating new, unique predicates. Despite working with a range of abstracts, I found that the NEF often returned nearly identical predicates across different entities. This repetition made it difficult to produce a diverse and interesting set of triples. The process of verifying and refining these predicates to ensure they were both accurate and unique was painstaking. Each abstract seemed to yield the same or very similar relationships, which won’t align with our  project’s goals.

#### Diversifying the Domains

To overcome this problem, I decided to diversify the domains of the abstracts I was working with. Instead of sticking to a single domain, I explored abstracts from a variety of fields, including movies, people, science, history, and culture. For instance, in the sport domain, I worked with abstracts related to Lionel Messi, Michael Jordan which introduced predicates like "season of club or team" and "sports season of league or competition." In the science domain, abstracts about String Theory and drugs like Pennicilin yielded predicates like "main subject" and "drug used for treatment." This approach brought some much-needed variety to the predicates, introducing new relationships and concepts that were previously missing.

As i proceed, i will continue exploring different domains and updating the [data on github].

Gracias!!!

----

[data on github]: https://github.com/dbpedia/neural-extraction-framework/blob/abdulsobur-gsoc-2024/GSoC24/PredicateSuggestion/new_triples.csv
