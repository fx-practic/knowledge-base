Path: 001-llm-general\001-006-llm-single-facts.md

# 001-006. LLM Single Facts

Related pages:

- [001. LLM General Index](./001-llm-general-index.md)
- [001-003. What An LLM Model Is Built From And What Files Store It](./001-003-model-files.md)

## Facts

| Number | Fact |
| --- | --- |
| F001 | 4096 is usually called hidden size, model dimension, or embedding dimension. |
| F002 | Hidden size means the length of one internal token vector. |
| F003 | If hidden size is 4096, one internal token vector contains 4096 numbers. |
| F004 | The common symbol for hidden size is d_model or d. |
| F005 | A feed-forward block usually maps d to d_ff and then back to d. |
| F006 | d_ff is the larger temporary internal size inside the feed-forward block. |
| F007 | The output of a feed-forward block has size d. |
| F008 | The output of a transformer layer has size d for each token. |
| F009 | The final prediction output is not one number. |
| F010 | The final prediction output has one score for every token in the vocabulary. |
| F011 | If vocabulary size is V, the final prediction output size is V. |
| F012 | Do not confuse internal vector size d with final vocabulary output size V. |
