## Claim 2: DR-TTS Trajectory Synthesis

**Claim:** DR-TTS (Decompose-Recompose Tool Tree Search) first trains specialized single-tool experts via RL, then recomposes them with tree search to produce 10K SFT trajectories balanced across text-to-text, text-to-image, image-to-image, and knowledge-based search.

### Paper specification (Section 3.2)

Two-stage pipeline:
1. **Decomposed training:** Search tasks categorized by required tool type using GPT. Specialized experts trained via RL for each tool.
2. **Recomposed tree search:** Tool experts collaborate through tree search. Starting from a question, the search branches over different tool experts at each level. Each node represents a reasoning step, tool call, and tool response. Incorrect answers lead to branch pruning.
3. **Trajectory extraction:** Successful trajectories extracted as ordered sequences → DR-TTS-10K dataset.

### Codebase evidence

The DR-TTS-10K generation code is **not in the repo**. However, the 4 tool types described in the paper are confirmed in the code:

**Tool config** (`search_tool_config_final_model.yaml`):
```yaml
1. image_search_by_text_query (text-to-image)
2. image_search_by_lens (image-to-image)
3. text_search (text-to-text)
4. model_search (knowledge-based)
```

Implementation files:
- https://github.com/HJYao00/MM-DeepResearch/blob/d7c8d82/verl/verl/tools/search_tool.py — text_search (E5 FAISS retrieval, topk=3, port 9000)
- https://github.com/HJYao00/MM-DeepResearch/blob/d7c8d82/verl/verl/tools/search_tool_image.py — image_search_by_text_query (Jina CLIP FAISS, port 9001)
- https://github.com/HJYao00/MM-DeepResearch/blob/d7c8d82/verl/verl/tools/search_tool_image_lens.py — image_search_by_lens (cached image similarity)
- https://github.com/HJYao00/MM-DeepResearch/blob/d7c8d82/verl/verl/tools/search_tool_model.py — model_search (expert model query)

### Assessment

**Partially supported.** The 4 tool types are confirmed and the training code uses them. The DR-TTS-10K dataset and the single-tool expert RL training pipeline are described in the paper but not open-sourced. The repository jumps directly to end-to-end GRPO training with all 4 tools simultaneously.

**Verdict: SUPPORTED (tool taxonomy confirmed; trajectory generation pipeline not independently verifiable)**
