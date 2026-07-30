# Claim 3: Training pipeline (SFT + GRPO RL)


---
<!-- trackio-cell
{"type": "markdown", "id": "cell_6e9459d24f18", "created_at": "2026-07-30T02:04:46+00:00", "title": "Evidence"}
-->
## Claim 3: Training Pipeline (SFT + GRPO RL)

**Claim:** The training pipeline combines SFT on DR-TTS trajectories with multi-turn GRPO reinforcement learning using format-compliance and LLM-judged accuracy rewards.

### Codebase evidence — FULLY CONFIRMED

#### SFT stage

The SFT stage uses LLaMA-Factory (external, not in repo). The SFT checkpoint is published on Hugging Face:
- https://huggingface.co/HuanjinYao/MM-DeepResearch-8B-SFT

The repo README instructs using this checkpoint as the base for RL training.

#### GRPO RL stage (fully in repo)

**Training entry point:** `run_qwen3vl-8b_instruct_search_multiturn_sglang_mmdeepresearch.sh`

Key hyperparameters confirmed in code:

| Hyperparameter | Value |
|---|---|
| Algorithm | GRPO (`algorithm.adv_estimator=grpo`) |
| Train batch size | 64 |
| Rollouts per prompt (n) | 5 |
| Max prompt length | 60,000 |
| Max response length | 5,000 |
| Max turns (user/assistant) | 4 / 4 |
| Learning rate | 1e-6 |
| KL loss coefficient | 0.001 (low_var_kl) |
| Entropy coefficient | 0 |
| PPO mini-batch size | 16 |
| Total epochs | 35 |
| GPUs | 8x H20 |
| Rollout engine | SGLang (async mode) |
| Format | Hermes |

**Reward function:** `search_r1_like_qa_em_candidate_model_judge.py`:

1. **Format reward** (weight = 0.1): Checks correct tag pairing and ordering of `<think>`, `<tool_call>`, `<tool_response>`, and `<answer>` blocks.
2. **Accuracy reward** (weight = 0.9): Two-stage evaluation:
   - **Exact match** against golden answers and candidate answers (normalized)
   - **LLM judge fallback** using Qwen3.5-35B-A3B when no exact match
3. Penalty for exceeding 10 `</answer>` tags (divides reward by 4)

**Data sources** (17 total, all routing to same reward function):
```
searchR1_fvqa, searchR1_SimpleVQA, searchR1_nq, searchR1_triviaqa,
searchR1_popqa, searchR1_hotpotqa, searchR1_2wikimultihopqa,
searchR1_musique, searchR1_bamboogle, searchR1_MMSearch_infoseek,
searchR1_MMSearch, searchR1_EcomBench, searchR1_WIKI_Train,
searchR1_LiveVQA, searchR1_M3CoT, searchR1_MMK12, searchR1_WeMath
```

**Tool interaction format:** Multi-turn agent loop with `<think>` reasoning, `<tool_call>` JSON invocations, `<tool_response>` results, and `<answer>` final answers.

**Offline retrieval engines:**
- Text: E5 FAISS GPU index (port 9000) — FastAPI server
- Image: Jina-CLIP FAISS GPU index (port 9001) — FastAPI server
- Model: Judge/expert model via OpenAI-compatible API

### Assessment

**FULLY SUPPORTED by code audit.** The complete GRPO training pipeline with the exact reward structure described in the paper is implemented and verified.

**Verdict: CONFIRMED**
