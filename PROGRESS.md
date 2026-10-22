# PROGRESS: rca-bench fine-tune (Qwen3-8B, Kaggle free GPU)

A fresh agent must be able to resume from this file alone.

## Current state

| item | value |
|---|---|
| current step | **0, setup: BLOCKED.** Kaggle dataset `rca-bench-ft-data` is not visible from the Kaggle account `romansipula` |
| base model | `Qwen/Qwen3-8B` (Apache-2.0; chosen by the user 2026-10-22). Reason: standard architecture, thinking mode can be switched off, fits a T4 with QLoRA at 8k (estimate ~10-12 GB), and Q4_K_M (~5 GB) fits a GTX 1080 Ti |
| Kaggle account | `romansipula` (API token available in the agent session as an env variable) |
| Kaggle kernel | – (not created yet) |
| HF account | `Roman82x`; `HF_TOKEN` is set as a Kaggle Secret (user confirmed 2026-10-22). It must be enabled per notebook in the UI (Add-ons → Secrets) after the first push |
| HF repo for checkpoints | `Roman82x/rca-bench-ft` (private; not created yet, the first kernel will create it) |
| last checkpoint | – |
| GPU hours used (this project) | 0.0 |
| Kaggle weekly quota | see `kaggle quota` (week starting 2026-10-17: 30 h total) |

## Results so far

none yet

## Decisions (made on val only, never on test)

* Chinese-origin models are allowed for this project (user, 2026-10-22).
* Qwen3-8B chosen over Qwen3.5-9B: Qwen3.5 uses hybrid linear attention (Gated DeltaNet). Its fast
  kernels are unlikely to run well on T4 or a GTX 1080 Ti, and it is multimodal with a 248k vocabulary.

## Next action

The user has to make `rca-bench-ft-data` (train/val/test jsonl + README) available as a **private Kaggle
dataset on the account `romansipula`**. Then verify with:
`kaggle datasets files romansipula/rca-bench-ft-data` (expect 4 files).

## Plan (from the task spec)

0. setup → 1. model (done: Qwen3-8B) → 2. baseline on 100 val + full test (`responses_base_test.jsonl`)
→ 3. smoke fine-tune (~50 steps, kill + resume from the hub checkpoint, measure tokens/s) → 4. full QLoRA
(answer-only loss, r 16-32, 1 epoch) → 5. val evaluation → 6. test inference once
(`responses_ft_test.jsonl`) → 7. export the adapter + GGUF Q4_K_M → 8. report.
