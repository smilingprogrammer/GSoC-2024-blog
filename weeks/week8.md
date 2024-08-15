---
layout: default
title: Week 8- Checkpoint Reviews; Streamlining Evaluation 
---

# Week 8- Checkpoint Reviews: Streamlining Evaluation 📝

---

## Overview
In the eighth week of my GSoC journey, the spotlight was on the model evaluation process and the review of the fine-tuned checkpoints. Our preliminary observations confirmed that a post-processing stage is an indispensable prerequisite for the evaluation process.



## Findings and challenges
Having trained the StarCoder-1B model for over 23 hours (see [week 7]), we managed to save checkpoints at the 1000 and 2000 steps. The initial results are promising, although they've led to some new questions about model behavior. An example of input/output for the last fine-tuned checkpoint is given below.   

`[In]:`
> ```
> prompt = """
> # write a SPARQL query for:
> # What was the population of Italy in 2000 ?
> 
> # use following prefixes:
> PREFIX dbr: <http://dbpedia.org/resource/>
> PREFIX dbo: <http://dbpedia.org/ontology/>
> """
> ```
`[Out]:`

**Before fine-tuning:**
> ```
> # write a SPARQL query for:
> # What was the population of Italy in year 2000?
> 
> # use following prefixes:
> PREFIX dbr: <http://dbpedia.org/resource/>
> PREFIX dbo: <http://dbpedia.org/ontology/>
> PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
> PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
> PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
> PREFIX owl: <http://www.w3.org/2002/07
> ```

