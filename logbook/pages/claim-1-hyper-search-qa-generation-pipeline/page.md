# Claim 1: Hyper-Search QA generation pipeline


---
<!-- trackio-cell
{"type": "markdown", "id": "cell_36442c62b592", "created_at": "2026-07-30T02:04:22+00:00", "title": "Evidence"}
-->
## Claim 1: Hyper-Search QA Generation Pipeline

**Claim:** MM-DeepResearch's Hyper-Search pipeline builds a hypergraph of image and text nodes from web sources and generates intra- and inter-hyperedge question-answer pairs, producing 3K search-intensive multimodal QA training examples.

### Paper specification (Section 3.1)

The paper describes a three-stage Hyper-Search pipeline:

1. **Hypergraph Construction:**
   - Web images → image nodes (I), webpage content → text nodes (T)
   - MLLMs generate captions for image nodes and summaries for text nodes
   - **Node expansion:** Image nodes undergo image reverse search (webpage content → text nodes) and image visual search (similar images → image nodes). Text nodes use MLLMs to extract URLs (text nodes) and relevant image links (image nodes).
   - **Hyperedge connection:** Each expansion creates a hyperedge linking parent + children, modeling cross-modal retrieval relationships.
   - Process builds a depth-D hypergraph incrementally.

2. **Multimodal QA Generation:**
   - Level 1 (Intra-hyperedge): QA within a single hyperedge; image node as query.
   - Level 2 (Inter-hyperedge): Cross-hyperedge integration requiring multiple search rounds.

3. **Search QA Filtering:**
   - GPT-5 and Seed1.8 filter low-quality instances → Hyper-Search-3K dataset.

### Codebase evidence

The Hyper-Search-3K generation code is not included in the public repository (https://github.com/HJYao00/MM-DeepResearch/tree/d7c8d82). The repo only contains training and evaluation code using pre-built parquet data files. Key observations:

- The training data file `training_data_rl.parquet` (1.1 GB, 252 downloads) on https://huggingface.co/datasets/HuanjinYao/MM-DeepResearch-corpus contains RL training data.
- The `data_preprocess/` directory only contains `preprocess_MMSearch.py`, which reformats existing MMSearch benchmark data — no hypergraph construction code.
- The training script references 17 data sources (FVQA, SimpleVQA, MMSearch, InfoSeek, LiveVQA, etc.) — these appear to be pre-existing datasets reformatted for RL.

### Assessment

**Partially supported.** Methodology is well-specified in the paper and plausible. The 3K example count is modest and consistent with the paper's description. However, the generation code is not open-sourced and no separate Hyper-Search-3K dataset is published. The offline corpus (https://huggingface.co/datasets/HuanjinYao/MM-DeepResearch-corpus) contains the retrieval indices and training data that the downstream pipeline uses, but the intermediate dataset is not independently verifiable.

**Verdict: SUPPORTED (paper + architecture audit, no standalone dataset found)**
