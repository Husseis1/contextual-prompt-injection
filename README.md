# Why simple prompt-injection filters fail on contextual attacks

A small, self-contained experiment in a customer-service chatbot setting.

Two defenses built from **known, explicit** injection patterns ("ignore previous instructions", "system override", ...) are evaluated on:

- **Explicit injections** – obvious override language
- **Contextual injections** – no override language, but a request that is inappropriate *in this context* (another customer's data, refunds above policy, internal configuration, unbounded output that burns tokens)

| Defense | Benign flagged | Explicit detected | Contextual detected |
|---|---|---|---|
| Keyword / regex filter | 0% | 88% | 0% |
| TF-IDF + logistic regression | 0% | 100% | 10% |

*(held-out test set)*

Both defenses learn the surface features of known attacks rather than the underlying norm violation, so contextual attacks pass almost entirely. This illustrates the argument in Abdelnabi & Bagdasarian, [*AI Agents May Always Fall for Prompt Injections*](https://arxiv.org/abs/2605.17634) (2026), that data–instruction separation misses context-based attacks, and motivates context-aware, out-of-model policy checks.

**Limitations:** tiny hand-written dataset, simple classifiers, no LLM in the loop. Results are illustrative, not a benchmark.

## Run

```bash
pip install numpy scikit-learn matplotlib jupyter
jupyter notebook contextual_prompt_injection.ipynb
```
