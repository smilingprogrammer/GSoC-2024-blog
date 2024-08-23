---
layout: default
title: Week 9- Prompt Engineering + Traditional Deep Learning Approach
---

# Week 9- Prompt Engineering + Traditional Deep Learning Approach 🤿

---

### Overview
Our focus this week was mainly to develop new ways to improve our prompt engineering to entract triples from knowledge graph. Our initial approach was to try out new prompt with this Triplex model, and if we don't get a good result try out the latest best opensource Large Language Model, the Meta Llama 3.1 (Quantized version).


### Findings and challenges

The week started with us trying out different prompt to improve the Triplex model, by giving it an input text without a specified entity type and relation, while also trying different prompts. However, we still get thesame output which is an empty triples like last week. Therefore, we decided to try out the latest best opensource Large Language Model as at the time of writing this article, the Llama 3.1 ( [Quatized Llama 3.1] ). We tried out different prompt on this quantized model and got different results based on the prompt. There are some instances where instead of the triples we expected as output we got lines of code. example;


`[In]:`
```
prompt = """

    Perform Named Entity Recognition (NER) and extract knowledge graph triplets from the given text, identifying relevant entity type and predicates. Provide the extracted entities and their types, as well as triplets in the format (Subject-Predicate-Object).

        **Text:**
    "Joseph Robinette Biden Jr. (/ˈbaɪdən/; born November 20, 1942) is an American politician who is the 46th and current president of the United States.
    A member of the Democratic Party, he previously served as the 47th vice president from 2009 to 2017 under President Barack Obama, and represented Delaware in the United States Senate from 1973 to 2009.
    Biden was born and raised in Scranton, Pennsylvania, and moved with his family to Delaware in 1953 when he was ten years old.
    He studied at the University of Delaware before earning his law degree from Syracuse University. .........(Wikipedia Joe Biden Abstract)"

"""
```

`[Out]:`
```

**Code:**
    ```python
    import spacy
    import re

    # Load the spaCy model for English
    nlp = spacy.load('en_core_web_sm')

    # Define a function to perform NER and extract knowledge graph triplets
    def extract_triplets(text):
        # Process the text using spaCy
        doc = nlp(text)

        # Initialize lists to store entities and triplets
        entities = []
        triplets = []

        # Iterate over the entities in the document
        for ent in doc.ents:
            # Extract the entity's text, type, and start/end positions
            entity_text = ent.text
            entity_type = ent.label_
            start = ent.start
            end = ent.end ...........

```

So from the instances above we had to explicitly specify to the model not to reply with lines of code but a triple. Below is an instance example;
`[In]:`
```

prompt = """

    Using your own understanding of knowledge graph. Identify the available valid triples in the text given below. Please do not return a code to run it. Perform the extraction yourself and return the valid triples in this format (subject, predicate, object). Don't include ambiguos relations and entity \n

        **Text:**
    "Joseph Robinette Biden Jr. (/ˈbaɪdən/; born November 20, 1942) is an American politician who is the 46th and current president of the United States.
    A member of the Democratic Party, he previously served as the 47th vice president from 2009 to 2017 under President Barack Obama, and represented Delaware in the United States Senate from 1973 to 2009.
    Biden was born and raised in Scranton, Pennsylvania, and moved with his family to Delaware in 1953 when he was ten years old.
    He studied at the University of Delaware before earning his law degree from Syracuse University. .........(Wikipedia Joe Biden Abstract)"

"""
```
`[Out]:`
```

    **Valid triples:**
    (Joseph Robinette Biden Jr., born in, November 20, 1942)
    (Joseph Robinette Biden Jr., is the, 46th and current president of the United States)
    (Joseph Robinette Biden Jr., previously served as, 47th vice president from 2009 to 2017 under President Barack Obama)
    (Joseph Robinette Biden Jr., represented, Delaware in the United States Senate from 1973 to 2009)
    (Joseph Robinette Biden Jr., was born and raised in, Scranton, Pennsylvania)
    (Joseph Robinette Biden Jr., moved with his family to, Delaware in 1953)
    (Joseph Robinette Biden Jr., studied at, the University of Delaware)
    (Joseph Robinette Biden Jr., earned his law degree from, Syracuse University)
    (Joseph Robinette Biden Jr., was elected to, the New Castle County Council in 1970)
    (Joseph Robinette Biden Jr., became the sixth-youngest senator in U.S. history after he was elected to, the United States Senate from Delaware in 1972)..........

```
We also tried different other prompt. However, there is always a problem with our output, either the triples is focused on one entity as a subject, in this scenario "Joseph Robinette Biden Jr", or we get a relation that is too long, or an unimaginable length of object entitty. This makes our current problem of getting unique predicate in the Neural Extraction Framework to be unsolved yet.

### The Game Changer

Rewinding back in the previous week, we solved a particular task of predicate suggestion that opened a new perspective to me when making use of a deep learning model. It was the use of a technique called "Entailment technique" (where we made use of nodes and edges in a graph) with the [microsoft deberta model] to suggest the best relation to represent a group of similar clustered relation. This techniques shows it not always about the deep leaning model we are using, but also about how we can make use of the model in different approach. Therefore, after prompting various quantized model and got performance result which isn't really much impressive, we tried out a technique similar to this.

We implemented a knowledge graph system using Python. We defined the graph structure with Pydantic models for Nodes and Edges. To generate content, we utilized the quantized [Hermes-Llama model 3] and [3.1] repectively, guided by a custom prompt function. We constrained the AI's output to our desired format using a regex pattern derived from a JSON schema. We processed the generated data to create a more interconnected graph. We converted the output graph data into a pandas DataFrame for easier analysis and representation of the subject-predicate-object triples. 

This new method output were really impressive and operates like the end to end ReBEL model, but this time around it gives us different unique predicate. The Hermes-Llama makes use of a chat template for its prompt, Below is an instance of prompt used to fit the template to our taste (The prompt we used is to get various interconnected graph in a wikipedia abstract);

`[In]:`
```
def generate_hermes_prompt(user_prompt):
    return (
        "<|im_start|>system\n"
        "You are a world class AI model who generates complex knowledge graphs in JSON format. "
        "Create a diverse set of interconnected relationships between multiple entities. "
        "Ensure that relationships are not just centered around one entity, but form a network of connections. "
        f"Here's the json schema you must adhere to:\n<schema>\n{json_schema}\n</schema>\n"
        "Remember to create multiple nodes and establish various relationships between them.<|im_end|>\n"
        "<|im_start|>user\n"
        + user_prompt
        + "\nExpand on this scenario by adding more entities and relationships to create a complex, interconnected knowledge graph."
        + "<|im_end|>"
        + "\n<|im_start|>assistant\n"
        "<schema>"
    )
```

By using the Joe_Biden wikipedia abstract as our user_prompt, we were able to get output and visualize various nodes linked to one another with edges serving as the relation. Below is a little example of our output following the subject -> predicate -> object template;

```
Joseph R. Biden Jr.  -> 	Represented in the Senate  ->	 Delaware
Joseph R. Biden Jr. 	->  Member  ->	 United States Senate
Delaware	 ->  Part of 	->  New Castle County
University of Delaware	 ->  Located in ->	Delaware
Russia	 ->  Initiator of  ->	 2022 Russian invasion of Ukraine
Taliban 	->  Seized control of  ->  Afghanistan
Kamala Harris 	->  Served with	 ->  Joseph R. Biden Jr.

```
Although majority of the output triples still require for us to validate its domain range, On this we will be crafting out another method to achieve this.

### Extra -> A bug turn into a feature;

The initital prompt below we used to extract this triples only focuses on a single entity. We can use this as another feature to get only the entity and relation that are associated to the particular entity abstract we are trying to get its knowledge graph triples. An instance of the initial prompt is below

`[In]`
```
def generate_hermes_prompt(user_prompt):
    return (
        "<|im_start|>system\n"
        "You are a world class AI model who answers questions in JSON "
        f"Here's the json schema you must adhere to:\n<schema>\n{json_schema}\n</schema><|im_end|>\n"
        "<|im_start|>user\n"
        + user_prompt
        + "<|im_end|>"
        + "\n<|im_start|>assistant\n"
        "<schema>"
    )
```
Where we use our user_prompt as the Joe_Biden Abstract. The output was only centered to Joe_Biden as the Subject;

`[Out]`
```
0 Joseph Robinette Biden Jr.	-> presidency ->	46th president of the United States
1	Joseph Robinette Biden Jr.	-> member ->	Democratic Party
2	Joseph Robinette Biden Jr.	-> vice presidency	-> 47th vice president
3	Joseph Robinette Biden Jr.	-> vice president -?	Barack Obama
4	Joseph Robinette Biden Jr.	-> representative	-> United States Senate
5	Joseph Robinette Biden Jr.	-> residence	-> Delaware
6	Joseph Robinette Biden Jr.	-> alumnus	-> University of Delaware
7	Joseph Robinette Biden Jr.	-> alumnus	-> Syracuse University
8	Joseph Robinette Biden Jr.	-> member	-> New Castle County Council
9	Joseph Robinette Biden Jr.	-> senator	-> U.S. history
```

Overall the performance of the Quantized Hermes-Llama 3 and 3.1 combined with our knowledge graph extraction technique were really impressive.

----
[Quatized Llama 3.1]: https://huggingface.co/YorkieOH10/Meta-Llama-3.1-8B-Instruct-Q8_0-GGUF/tree/main
[microsoft deberta model]: https://huggingface.co/microsoft/deberta-v3-large
[Hermes-Llama model 3]: https://huggingface.co/NousResearch/Hermes-2-Pro-Llama-3-8B-GGUF
[3.1]: https://huggingface.co/NousResearch/Hermes-3-Llama-3.1-8B-GGUF
