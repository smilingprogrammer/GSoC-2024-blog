---
layout: default
title: Week 4- The devil is "always" in the detail!
---

# Week 4 - The devil is "always" in the detail! 🔬

---

## Overview

The fourth week of my GSoC journey started by focusing on refining the *formatting of prompts*. This was an essential step to establish a baseline for translating raw natural language questions into their corresponding SPARQL queries before tweaking the original parameters of the pre-trained models. It can be viewed as inspecting the initial behavior of the candidate models in terms of their initial capabilities in handling the task of `text-to-sparql`. This week, the release of smaller variants of the [StarCoder models by BigCode], sparked our interest. We decided to include the 3B variant in our evaluation process for the fourth week. 


## Findings and challenges 
This week we revisited the challenge we encountered last week, that is, taking control of the length of the generated sequences. The soloution seems to be lied within the proper use of special tokens, in particular, `EOS` (End Of Sequence) token. The preassumption was that the tokenizer *might* automatically add special tokens to the input sequence. However, we observed despite using the `tokenizer.encode_plus` method, EOS token is not presented within the encoded input sequence. The following code blocks refer to our experiemnts with the `bigcode/starcoderbase-3b` checkpoint. 




## Next week plan
For the upcoming week, our plan is to:
- Process existing datasets for creating our intended prompt-tuning dataset.
- Explore exisiting models that can be used for DBpedia entity resoloution. 
- Search for instruct-tuned versions of code generation models. 


----
[StarCoder models by BigCode]: https://huggingface.co/bigcode
[dedicated repo directory]: https://github.com/dbpedia/neural-qa/tree/gsoc-mehrzad/gsoc/mehrzad

