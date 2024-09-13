---
layout: default
title: Week 12- Entity Disambiguation through LLM Reasoning 
---

# Week 9- Entity Disambiguation through LLM Reasoning 🤿

---

### Overview
This week we focused on narrowing down the Entity we needed based on the context it is used in a text.

### Approach
In the world of knowledge base, there are various real world entities that can be found in wikipedia. However, there are some other entities that have thesame entity name but different meaning or different context. An example of this is;
```
I love buying Apple to eat
```
```
I love buying Apple devices
```

In the example given above we can acknowledge that, the entity used "Apple" have thesame spelling but different meanings. The first Apple represents 'fruit', white the second apple represents the popularly known 'Apple' company. 
In the extraction of triples this needs to be specified, and since our approach is based on using Machine Learning model, we have to find a way to identify this. The well known Spacy library can be used for this. Based on its ability, `Spacy` can detect some few context in which an entity is used an using the above exmaple, `Spacy` can give us the output (Apple: ORG) in the second example. 

However, the Neural Extraction Framework needs something more than this, we need the ability of our model to discover not only when there is  difference in entity type but also when entities of thesame types are used in different context. Presently, we are making use of a quantized Llama 3.1 for our extraction of triples, and making use of footballers embeddings to get our entity. Therefore, we decided to try to make use of our LLM to also be able to detect the correct DBpedia entity based on its context in a text. We tried an example below;
```
Ronaldo plays for Brazil
```
We gave our model the above as it input, and also gave it two ronaldo entities to select from (Cristiano Ronaldo and Ronaldo Nazarion). Our model output shows it recognized the "Ronaldo" used in the above sentence as Cristiano Ronaldo, which is incorrect since the "Ronaldo" we were talking about in this context is "Ronaldo Nazario" that plays for brazil. We decided to enhance our output by implementing a json schema that allows our model to reason in steps and provides its output as a 'conclusion' in json format. It was able to recognize the correct `Ronaldo` entity in our context.

We then run another example with the entity "Dembele", since there are alot of dembele that plays football;
```
Dembele plays for France
```
We perform a triple extraction and `Dembele` came out as the Subject. We then proceed to find the to 5 Dembele entity in the embeddings, and provided our model to reason on it. The model was able to select the most popular `Dembele` entity `Ousmane Dembele` as its conclusion.
Now this is where its get interesting. There are players is football with thesame name but plays for different teams. an example is; Mousa Dembele that plays for France football team and another Moussa Dembele that plays for Belgium Football team. This context is one of the main challenges we want to provide solutions to. We proceed to try out our reasoning approach to solving this. We provided the examples below;

```
Mousa Dembele plays for Belgium
```
```
Moussa Dembele plays for France
```

Our model reasoning approach was able to detect the diferences and assigning each entity its correct entity in the knowledge base. 

### Limitations
Based on the capability of our model, its unable to recognize players that weren't available after its training and couldn't acccess the internet. This is a general problem as even the Llama model hosted by meta itelf also has thesame limitation.
