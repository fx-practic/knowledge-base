Path: 003-inference-run/004-prompt-tokenization-step.md

# 004. Prompt Tokenization Step

The user prompt is converted into token IDs.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| User text prompt. | Tokenizer splits text into tokens and maps each token to an integer ID. | Prompt token IDs, shape `T`. |

Example:

```text
"Explain attention"
-> ["Explain", " attention"]
-> [8492, 5634]
```

## What this step means

The model itself does not read letters directly. The inference runtime first
uses the tokenizer that belongs to the model.

## Related page

See [Tokens](../004-tokens.md).
