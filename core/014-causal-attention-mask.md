Path: core/014-causal-attention-mask.md

# 014. Causal Attention Mask

A **causal attention mask** is the left-to-right rule that lets each token position attend only to itself and earlier positions, not later positions.
During training, the model often receives a full sequence such as `dog bites man`, so the mask prevents the `dog` position from seeing `bites` or `man` while learning to predict the next token.
During live generation, the prompt `dog bites man` is already known, and the system usually reads the prediction from the last prompt position, so that position is allowed to attend to the whole prompt.
The same mask is used in both cases: it allows left/current tokens and blocks right/future tokens.
The difference is practical: in training there are future tokens inside the provided sequence, while in live generation future generated tokens do not exist yet.
