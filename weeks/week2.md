---
layout: default
title: Week 2- Assessing the Co-Occurrence Algorithm
---

# Week 2 - Assessing the Co-Occurrence Algorithm 🔍 

---

## Overview

The week was a blend of learning, realignment, and accomplishment. Understanding the limitations of the co-occurrence algorithm in the context of our project was a critical step. Moreover, realigning the project's goals with the mentors' insights ensured that our efforts are directed towards achieving the desired outcomes. Exporting the triples into RDF format marked a productive end to the week, setting the stage for the next phase of the project.


## Findings and challenges 
The week kicked off with a deep dive into the co-occurrence algorithm. My task was to explore how this algorithm could be implemented into our existing project pipeline.
As I progressed, I realized that the co-occurrence algorithm might not be the optimal solution for our project's ambitions. The main issue was that the algorithm wouldn't effectively utilize our extracted relations when suggesting predicates within the DBpedia ontology for these relations. This limitation was significant as it could hinder the accuracy and relevance of the suggestions.

On meeting with my mentors to discuss these findings, it became clear that my understanding of the task's aim differed from the actual objective. The current pipeline is able to generate similar DBpedia predicates for a given relation. However, the task's ultimate aim was to suggest the creation of a new propertyy if a similar one does not exist in the DBpedia ontology. This realization was crucial as it redefined my approach and focus.

At the end of this week, I was able to contribute to the pipeline by achieving the task of exporting the extracted triples from the pipeline into RDF format using the `rdflib` library recommended by the mentors. This task was essential for ensuring that our final triples is structured and can be exported as an RDF triple for future queries and integrations.

## Next week plan
Looking ahead into the third week, the focus will be on developing a solution that can accurately suggest creation of a new property when necessary, thereby enhancing the robustness and flexibility of our project pipeline.
