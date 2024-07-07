---
title: Home
layout: home
---
# Towards a Neural Extraction Framework in DBpedia

## Project Summary
The goal of this [project] is to develop a framework for predicate resolution of wiki links among entities.

This project will potentially generate millions of new statements. This new information could be released by DBpedia to the public as part of a new dataset. The creation of a neural extraction framework could introduce the use of robust parsers for a more accurate extraction of Wikipedia content.


### Mentors
Tommaso Soru, Ziwei XU, Mehrzad Shahinmoghadam, Yogesh Kulkarni

### Topics
Knowledge graphs, semantic web, ontology vocabularies, Extraction Framework

----

[DBpedia]: https://www.dbpedia.org/
[project]: https://summerofcode.withgoogle.com/programs/2024/projects/J4tJODFV
[Neural Extraction Framework]: https://github.com/dbpedia/neural-extraction-framework


### Project workflow
```mermaid
graph TD
    wiki_page[Wikipedia Page] --Extract plain text--> pure_text[Pure text]

    pure_text--coreference resolution-->coref_resolved_text[Coreference Resolved Text]
    coref_resolved_text-->sentences[Sentences]
    sentences--sentence_i-->rebel(REBEL)
    rebel--as text-->entities[Entities]
    rebel--as text-->relations[Relations]
    
    relations--get embedding-->vector_similarity(Vector similarity with label embeddings);
    vector_similarity-->predicate_uris[Predicate URIs]

    sentences--sentence_i-->annotation_for_genre(Annotate entities in text)

    entities-->annotation_for_genre

    annotation_for_genre--annotated text-->genre[GENRE]

    genre-->entity_uris[Entity URIs]
    
    entity_uris-->triples[Triples]
    predicate_uris-->triples[Triples]
    triples--Validate-->final_triples[Final triples]
```
