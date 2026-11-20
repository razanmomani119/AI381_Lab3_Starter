# Model selection

## The question

The model predicts whether a customer is likely to leave the company (customer churn). A wrong prediction can cause the company to lose customers or waste resources on unnecessary retention offers.

## Candidates

| Candidate | Why I tried it | AUC | F1 | F1 per slice (lowest) | p95 latency (ms) | Size (KB) | MLflow run ID |
|---|---|---:|---:|---:|---:|---:|---|
| Logistic Regression | Simple and interpretable baseline | 0.798 | 0.495 | 0.2444 | 4.8 | 4 | e26c5d12692440538f51db721f8494e7 |
| Random Forest | Captures nonlinear patterns using multiple trees | 0.759 | 0.454 | 0.2222 | 17.2 | 3160 | 693e4c37c29c49e3ba2a4718eeb98fbd |
| Gradient Boosting | Learns patterns by improving previous trees | 0.748 | 0.417 | 0.2222 | 4.7 | 142 | 06405ffc410647bdb5d6de9a19b567f7 |

## The champion

I selected Logistic Regression as the champion because it achieved the highest AUC (0.798) and F1 score (0.495). It also had the lowest model size (4 KB) and a low p95 latency of 4.8 ms.

I accepted losing some performance on individual contract slices compared to other models because Logistic Regression provided the best overall results.

## My promotion rules

- Latency budget (p95): **25 ms**, because the model should respond quickly and leave room for production overhead.
- Minimum gain to replace the champion (F1 points): **0.005**, because a new model should provide a meaningful improvement instead of a very small change.

## What I would check next

I would monitor model performance on each contract type, especially groups with fewer customers, because overall performance may hide poor predictions for a specific group. I would also monitor changes in customer behavior over time.