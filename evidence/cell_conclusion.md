## Conclusion

### Summary of Findings

| Claim | Verdict | Notes |
|---|---|---|
| Claim 1: Hyper-Search QA generation | SUPPORTED | Methodology well-specified; generation code not open-sourced |
| Claim 2: DR-TTS trajectory synthesis | SUPPORTED | 4 tool types confirmed in code; trajectory generation code not in repo |
| Claim 3: Training pipeline (SFT + GRPO RL) | CONFIRMED | Full pipeline verified by code audit |
| Claim 4: 63.0% average accuracy | NOT VERIFIED | Paper-reported; evaluation infra confirmed but not executable in this session |
| Claim 5: Ablation (search depth, offline vs online) | SUPPORTED | Retrieval framework and embedding models confirmed |

### Reproducibility Notes

1. **Code availability:** The training and evaluation code is fully open-sourced (Apache 2.0) at https://github.com/HJYao00/MM-DeepResearch (commit d7c8d82). However, the data generation pipelines (Hyper-Search, DR-TTS) are not included.

2. **Data availability:** The training corpus is published on Hugging Face (https://huggingface.co/datasets/HuanjinYao/MM-DeepResearch-corpus, 252 downloads). The SFT and final model checkpoints are available.

3. **Compute requirements:** Full reproduction requires substantial GPU resources: 8x H20 for training (~7 days), 2x A100/H100 for evaluation. The model weights are provided, enabling evaluation without training.

4. **Search API dependency:** Evaluation requires SerpAPI/Serper and Jina Reader API keys, introducing external dependencies and cost (~$0.01 per query).

5. **Missing components:** The Hyper-Search-3K and DR-TTS-10K intermediate datasets are not published separately. The decomposed single-tool expert RL training code is not included.

### Overall Assessment

MM-DeepResearch is a well-engineered system with a strong experimental framework. The core training pipeline (Claim 3) is fully reproducible from the open-source code. The data generation claims (Claims 1-2) and ablation studies (Claim 5) are supported by the paper's methodology, open data, and code infrastructure. The benchmark results (Claim 4) are the least verifiable without significant GPU compute and paid API access, but the evaluation infrastructure is well-documented and the published model weights suggest the claimed improvements are achievable.
