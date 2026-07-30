## Claim 4: Benchmark Results (63.0% average accuracy)

**Claim:** MM-DeepResearch-8B reaches 63.0% average accuracy across six benchmarks (SimpleVQA, MMSearch, LiveVQA, FVQA, InfoSeek, BrowseComp-VL), a 17-point improvement over the Qwen3-VL-8B baseline, while the 32B variant reaches 65.3% average accuracy.

### Paper-reported results (Section 5, Table 1)

| Method | SimpleVQA | MMSearch | LiveVQA | FVQA | InfoSeek | BrowseComp-VL | Avg |
|---|---|---|---|---|---|---|---|
| Qwen3-VL-8B | 44.5 | 37.4 | 43.0 | 40.1 | 41.8 | 69.2 | 46.0 |
| MM-DeepResearch-8B | 62.6 | 67.8 | 54.3 | 55.4 | 57.6 | 80.4 | **63.0** |
| Qwen3-VL-32B | 51.7 | 42.1 | 48.5 | 45.3 | 47.2 | 71.6 | 50.4 |
| MM-DeepResearch-32B | 67.4 | 69.4 | 58.2 | 58.9 | 61.1 | 78.7 | **65.3** |

### Codebase evidence

The evaluation infrastructure is fully implemented and was reviewed:

1. **Evaluation script:** `run_eval_mmsearch_search_mp.sh` — multi-GPU sharded evaluation
2. **Eval loop:** `eval.py` — multi-turn tool interaction loop (max 7 turns)
3. **LLM judge:** `llm_judge.py` — correctness evaluator using Qwen3.5-35B-A3B
4. **Accuracy aggregation:** `acc.py` — merges shard results and computes accuracy

**Model weights published:**
- https://huggingface.co/HuanjinYao/MM-DeepResearch-8B (8.7B parameters, BF16, ~17.5 GB)
- https://huggingface.co/HuanjinYao/MM-DeepResearch-8B-SFT (SFT checkpoint)

**Evaluation requirements:**
- 2x GPUs for MM-DeepResearch-8B inference (SGLang, tp=2)
- Additional 4x GPUs for judge/summary model (Qwen3.5-35B-A3B, tp=4)
- Paid search API keys: SerpAPI/Serper + Jina Reader
- Image upload to public server for Google Lens image-to-image search

### Attempted verification

We were unable to independently verify the benchmark results in this session due to:
1. Insufficient GPU VRAM locally (RTX 3070 8GB — needs 2x H100/A100)
2. No Hugging Face Jobs `job.write` permission
3. Paid search API keys required for evaluation

### Assessment

**Paper-supported only.** The evaluation infrastructure is well-structured and consistent with the claimed pipeline. Model weights match the expected architecture (Qwen3-VL-8B + tool interaction finetuning). The 17pp improvement over the baseline is directionally plausible given the extensive multi-turn RL training.

**Verdict: NOT INDEPENDENTLY VERIFIED (paper-reported, infrastructure confirmed)**
