# FTEC5660 Homework 1: Receipt Chain

Build a LangChain pipeline that reads every supermarket receipt in a folder
with the vision-capable DeepSeek Flash model and answers these two questions:

1. How much money did I spend in total for these bills?
2. How much would I have had to pay without the discount?

For this homework, **amount spent** means the final payment after the receipt's
rounding line. **Without the discount** means the sum of the original positive
item prices: add back every promotion, coupon, member, app, packaging-damage,
and percentage discount, but do not add back rounding.

## Student task

Only edit the two functions in `hw1.py` that contain `### YOUR CODE HERE`:

- `build_chain()` creates your LangChain chain.
- `answer_queries()` runs the chain on the receipt images and returns one final
  response for each question.

You may use prompt chaining, routing, parallel calls, reflection, or a
combination. Your final responses should each contain one HKD amount. Do not
hard-code filenames or public answers; grading uses unseen receipt folders.

## Setup and public test

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Put your DeepSeek key after `DEEPSEEK_API_KEY=` in `.env`, then run:

```bash
python3 hw1.py --image-folder public_test
```

The program creates `results.csv` in the current directory. Its columns are
`query`, `model_response`, and `correctness`. The public answers are in
`public_test/ground_truth.json`. The starter intentionally returns the dummy
response `please design your chain to answer these two queries.` so it runs
before you add any API code.

The required model is `deepseek-v4-flash-vision-exp`, the vision-capable
DeepSeek Flash model. JPEG, PNG, GIF, and WebP inputs are accepted by the
homework runner.


## Homework 1 solution: 

In this homework, I use a LangChain prompt, the required DeepSeek v4 model and a JSON parser to read each receipt. The Deepseek model extracts the payment after rounding and the original item line amounts. Python's Decimal adds the payments for the first answer and the original prices for the second. The second total is equivalent to the subtotal plus all discounts added back, without adding back rounding. For each question, it gets one HKD amount. Both answers passed the public test and same as the ground-truth json you provided.

```mermaid
flowchart LR
    A["Receipt images"] --> B["Prompt + DeepSeek vision"]
    B --> C["JSON parser"]
    C --> D["Decimal sums"]
    D --> E["Two HKD answers"]
    E --> F["results.csv"]
```

| Ground-truth test | Answer | Result |
|---|---:|---|
| Total paid after rounding | HK$1974.30 | correct |
| Total without discounts | HK$2348.20 | correct |
