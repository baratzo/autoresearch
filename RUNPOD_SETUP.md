# RunPod Setup — Hebrew Autoresearch

## 1. Pod Requirements
- GPU: H100 80GB SXM (recommended) or A100 80GB
- Template: PyTorch 2.x
- Disk: 50GB+ (for data + model)

## 2. First-time setup (run once)

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc

# Clone the Hebrew fork
git clone -b hebrew-culturax https://github.com/baratzo/autoresearch.git
cd autoresearch

# Set HuggingFace token (needed for CulturaX gated dataset)
export HF_TOKEN=<your-huggingface-token>

# Install deps + download Hebrew data + train tokenizer (~5 min)
uv sync
uv run prepare.py

# Verify data exists
ls ~/.cache/autoresearch/data/
ls ~/.cache/autoresearch/tokenizer/
```

## 3. Start the autonomous experiment loop

Follow the instructions in `program.md`. In short:

1. Agree on a run tag (e.g. `mar9`)
2. Create branch: `git checkout -b autoresearch/mar9`
3. Read all files for context: `README.md`, `prepare.py`, `train.py`
4. Create `results.tsv` with header: `commit\tval_bpb\tmemory_gb\tstatus\tdescription`
5. Run baseline first: `uv run train.py > run.log 2>&1`
6. Then loop: modify `train.py` → commit → run → check results → keep/revert

## 4. Key differences from upstream

- **Data**: Hebrew CulturaX (uonlp/CulturaX `he` subset) instead of ClimbMix English
- **Tokenizer**: BPE trained on Hebrew text (same 8192 vocab size)
- **Everything else**: Same as upstream — same model, optimizer, eval, time budget
- `prepare.py` requires `HF_TOKEN` env var to download gated dataset
