# MM-DeepResearch Reproduction

Reproduction of ICML 2026 Paper #631 — "MM-DeepResearch: A Simple and Effective Multimodal Agentic Search Baseline" (Yao et al., ByteDance/NTU, arXiv:2603.01050, OpenReview: XZAOCytvKQ)

**Logbook:** https://huggingface.co/spaces/erobinson/repro-mm-deepresearch-a-simple-and-effective-multimodal-agentic-search-baseline

## Status: Code Audit + Paper Review

| Claim | Status | Evidence |
|-------|--------|----------|
| C1: Hyper-Search QA generation | Supported | Paper section audit |
| C2: DR-TTS trajectory synthesis | Supported | Codebase confirms 4 tool types |
| C3: Training pipeline (SFT + GRPO RL) | **Confirmed** | Full VeRL code audit |
| C4: Benchmark results (63.0% avg) | Paper only | Needs 2x A100 + API keys |
| C5: Ablation (offline vs online) | Supported | Code confirms E5 + Jina-CLIP + FAISS |

## Repo Contents

- `poster/` — Reproduction poster (HTML source, PNG, PDF, interactive embed)
- `evidence/` — Evidence cells and session trace
- `logbook/` — Logbook page sources (reference copy)

## Original Resources

- **Paper:** https://arxiv.org/abs/2603.01050
- **OpenReview:** https://openreview.net/forum?id=XZAOCytvKQ
- **GitHub:** https://github.com/HJYao00/MM-DeepResearch
- **Model (8B):** https://huggingface.co/HuanjinYao/MM-DeepResearch-8B
- **Model (8B-SFT):** https://huggingface.co/HuanjinYao/MM-DeepResearch-8B-SFT
- **Corpus:** https://huggingface.co/datasets/HuanjinYao/MM-DeepResearch-corpus
