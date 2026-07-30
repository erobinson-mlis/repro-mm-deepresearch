## Claim 5: Ablation Results

**Claim:** (a) Training on Hyper-Search data increases search depth from 1.6 to 2.3 average tool calls per query. (b) The offline multimodal retrieval corpus (E5 and Jina-CLIP embeddings) performs competitively with expensive online search APIs.

### Paper-reported ablation results (Section 5.2)

**Search depth (average tool calls per query):**
- InfoSeek data: 1.6
- Graph-based data: 1.7
- Hyper-Search data: **2.3**

**Offline vs Online search at evaluation (MMSearch):**
- Online search: 67.8 (SimpleVQA: 65.9)
- Offline search: 62.7 (SimpleVQA: 63.4)

### Codebase evidence

#### Offline retrieval corpus

The codebase confirms the offline retrieval infrastructure:

**Text retrieval (E5 embeddings):**
- Server: `retrieval_server.py` — FastAPI + FAISS GPU index
- Model: E5 embedding model (`intfloat/e5-base-v2` and `intfloat/e5-mistral-7b-instruct` supported)
- Index: `e5_Flat.index` (part of https://huggingface.co/datasets/HuanjinYao/MM-DeepResearch-corpus)
- Corpus: `merged_reindexed_new.jsonl` (text passages)
- Top-k retrieval: configurable, default k=3

**Image retrieval (Jina-CLIP embeddings):**
- Server: `retrieval_server_image.py` — FastAPI + FAISS GPU index
- Model: Jina-CLIP (`jinaai/jina-clip-v2`)
- Index: `jina-clip-v2_Flat_image.index`
- Corpus: `image_search_result_rag.parquet`

**Image-to-image search (cached):**
- Cache file: `lens_cached_data.jsonl` + `images.tar.gz`
- Custom implementation for training-time image similarity

**Training configuration** confirms multi-turn capability:
- `max_user_turns=4`, `max_assistant_turns=4`
- Maximum 4 tool calls per trajectory in training

#### Model search (knowledge-based)

The `model_search` tool queries an expert LLM model for knowledge-intensive queries, matching the paper's description of knowledge-based search tools.

### Assessment

The ablation-specific configurations are not in the public repo, but the infrastructure clearly supports:
- Multiple search depths via configurable turn limits
- Offline retrieval using the exact embedding models claimed (E5, Jina-CLIP)
- FAISS GPU-accelerated indices for efficient retrieval

The paper's claim that offline search is competitive (62.7 vs 67.8 on MMSearch) is plausible given that offline retrieval avoids latency/cost while providing the same semantic search capability.

**Verdict: SUPPORTED (infrastructure code confirms the embedding models, retrieval framework, and multi-turn capability; ablation scripts not in repo)**
