---
layout: default
title: Week 8- Revisiting Relation Extraction With Another Model
---

# Week 8- Revisiting Relation Extraction With Another Model 📝

---

## Overview
Over last week, we noticed some consistent repetition of predicates in the ReBEL model and also some Invalid triples. Our first step to tackle this is to approach the model that performs our relation extraction in the Neural Extraction Framework.


**Extracting Knowledge Triplets Using `SciPhi/Triplex`

The task was to extract triples from text through Named Entity Recognition (NER) and relations extraction, by utilizing the `SciPhi/Triplex` quantized `Triplex-Q4_K_M.gguf` model.

### Task Overview

The goal was to extract knowledge graph triplets from text. However, initial model requires a really high computational power, therefore we subtitute to using the quantized model. To achieve the usage of the quantized model, We made use of the Llama-cpp-python library and implemented a function, `triplextract()`, which enabled us perform both NER and relationship extraction. Unlike the REBEL model, the Trplex model makes use of prompt as its input format, and also require specifying the `entity types` and `Relation` we will like to extract. Therefore we targeted entities like **LOCATION**, **DATE**, and **CITY**, and relations such as **POPULATION** and **AREA**.

### Implementation

The `triplextract()` function is designed to:
- Perform NER to identify specific entities in the text.
- Extract relationships between entities based on predefined predicates.

```python
def triplextract(model, text, entity_types, predicates):
    input_format = """Perform Named Entity Recognition (NER) and extract knowledge graph triplets from the text. NER identifies named entities of given entity types, and triple extraction identifies relationships between entities using specified predicates.

        **Entity Types:**
        {entity_types}

        **Predicates:**
        {predicates}

        **Text:**
        {text}
        """
    message = input_format.format(
                entity_types=json.dumps(entity_types),
                predicates=json.dumps(predicates),
                text=text)

    output = model.create_completion(
        message,
        max_tokens=2048,
        stop=["</s>", "Human:", "H:"],  # Add appropriate stop tokens
        echo=False
    )
    return output['choices'][0]['text'].strip()
```

### Testing the Code

I tested this code on the example snippet about San Francisco, focusing on extracting its population. Here’s an example of the input:

```text
San Francisco, officially the City and County of San Francisco, is a commercial, financial, and cultural center in Northern California.

With a population of 808,437 residents as of 2022, San Francisco is the fourth most populous city in the U.S. state of California behind Los Angeles, San Diego, and San Jose.
```

However given the example above, the output were incosistent; Below are two different output from thesame input;
```json
{
    "entities_and_triples": [
        "[1], CITY:San Francisco",
        "[2], COUNTRY:United States",
        "[3], LOCATION:Northern California",
        "[4], NUMBER:808,437",
        "[1] POPULATION [4]",
        "[5], DATE:2022",
        "[6], POSITION:fourth most populous city",
        "[7], LOCATION:California",
        "[8], CITY:Los Angeles",
        "[9], CITY:San Diego",
        "[10], CITY:San Jose",
        "[11], NUMBER:808",
        "[1] POPULATION [4]",
        "[12], LOCATION:San Francisco County",
        "[13], NUMBER:24"
    ]
}
```
And this
```json
{
    "entities_and_triples": [
        "[1], CITY:San Francisco",
        "[2], COUNTRY:United States",
        "[3], LOCATION:Northern California",
        "[4], NUMBER:808,437",
        "[1] POPULATION [4]",
        "[5], DATE:2022",
        "[6], CITY:Los Angeles",
        "[7], CITY:San Diego",
        "[8], CITY:San Jose",
        "[9], LOCATION:California",
        "[10], POSITION:fourth most populous city",
        "[11], LOCATION:Northern California",
        "[1] AREA [11]"
    ]
}
```

From the above we can notice how we go two differnt output triples (
[1] POPULATION [4] -> San Francisco POPULATION 808,437
[1] AREA [11] -> San Francisco AREA Northern California
), from the same input. 
However, this is not our main target, as we require a model that doesn't need a pre-assigned entity type and Relation before performing relation extraction. Therefore, i went on to practice different prompt to fit this need as our input, Below are two different input and their output;

`[In]:`
```
input_format = """Perform Named Entity Recognition (NER) and extract knowledge graph triplets from the text. Identify relevant entity types and predicates based on the content of the text.

        **Instructions:**
        1. Identify relevant entity types present in the text.
        2. Identify relevant predicates (relationships) between entities in the text.
        3. Extract entities and their types.
        4. Extract triplets (subject-predicate-object) based on the identified entities and predicates.

        **Text:**
        {text}

        """
```
`[Out]:`
```json
{
    "entities_and_triples": []
}
```

And also;
`[In]:`
```
input_format = """Perform Named Entity Recognition (NER) and extract knowledge graph triplets from the text. Identify relevant entity types and predicates based on the content of the text.

        **Instructions:**
        1. Identify relevant entity types present in the text. Include at least LOCATION, CITY, COUNTRY, NUMBER, and DATE if applicable.
        2. Identify relevant predicates (relationships) between entities in the text. Include at least POPULATION and LOCATION_IN if applicable.
        3. Extract entities and their types.
        4. Extract triplets (subject-predicate-object) based on the identified entities and predicates.

        **Text:**
        {text}

        """
```
`[Out]:`
```json
{
    "entities_and_triples": [
        "[1], CITY:San Francisco",
        "[2], CITY:City and County of San Francisco",
        "[3], LOCATION:Northern California",
        "[1] LOCATION_IN [3]",
        "[4], NUMBER:808,437",
        "[1] POPULATION [4]",
        "[5], DATE:2022",
        "[6], LOCATION:California",
        "[1] LOCATION_IN [6]",
        "[7], CITY:Los Angeles",
        "[8], CITY:San Diego",
        "[9], CITY:San Jose",
        "[10], COUNTRY:United States",
        "[6] LOCATION_IN [10]",
        "[11], LOCATION:Northern California region",
        "[1] LOCATION_IN [11]"
    ]
}
```

From the example above, we noticed how we got a null output when we prompt the model without a targeted Entity and Relation, and also when we try to provide a little reference for it as a form of "guidance" giving us an output triples.

We then proceed to perform an entity linking using our NEF already available GENRE model implementation.

Going forward, if we are unable to engineer the best prompt as an input to extract the triples without a pre-assigned targeted Entity and Relation. The `Trplex` model doesn't really fit our goal as we require unique predicates, not a pre-assigned predicates.
