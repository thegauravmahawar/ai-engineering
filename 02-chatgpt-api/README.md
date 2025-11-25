# ChatGPT API

## How LLMs work?

Large Language Models (LLMs) are AI systems trained on massive text datasets to generate and understand human-like language.
They work by predicting the next token (not the next word!) in a sequence.

## How LLMs are trained?

**Supervised Learning Basics**

LLMs are primarily trained through supervised learning.

- Idea - Model learns an input → output mapping using labeled data.
- Example - Restaurant reviews labeled as positive or negative sentiment.

**Predicting the next work (Token)**

Training an LLM involves breaking sentences into many training examples:

Sentence: "My favorite food is a bagel with cream cheese and lox."

Becomes examples:

- Input: "My favorite food is a" → Target: "bagel"
- Input: "My favorite food is a bagel" → Target: "with"
- Input: "My favorite food is a bagel with" → Target: "cream"
- ...and so on.

## Types of LLMs

**Base LLM**

- Trained only to predict the next token.
- Might not follow instructions well.

**Instruction-tuned LLM**

- Fine-tuned using instruction → response pairs.
- Designed to follow user requests.

## How Instruction-tuning works?

- Train Base LLM
  - Trained on hundreds of billions of words.
  - Requires huge compute; takes months.
- Supervised Fine-Tuning (SFT)
  - Trained on curated instruction → response examples.
  - Much smaller dataset.
- RLHF (Reinforcement Learning from Human Feedback)
  - Humans rate outputs for helpfulness, truthfulness, and safety.
  - Model is optimized to produce higher-rated responses.

## Tokens

LLMs don't use words — they use tokens, which are chunks of characters.

Examples:

| Word      | Tokens               |
| --------- | -------------------- |
| Learning  | "Learning"           |
| Prompting | "'prom", "pt", "ing" |
| Lollipop  | "l", "oll", "ipop"   |

This explains why models struggle with letter-level tasks:

- "Reverse lollipop" → wrong
- "l-o-l-l-i-p-o-p" → reversed correctly
