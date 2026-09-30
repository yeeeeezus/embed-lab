# embed-lab

**Local-only semantic search: paste your notes, ask in plain words, get passages ranked by meaning.**

Live: **https://yeeeeezus.github.io/embed-lab/**

## What it does

- **Semantic search** over a pasted corpus: passages and query are embedded with `all-MiniLM-L6-v2` (384-dim) running locally via transformers.js, then ranked by cosine similarity. "the meeting about hiring" finds "we decided to postpone recruitment" with zero keyword overlap.
- **Pair comparison** with practical similarity bands (paraphrase / same topic / unrelated) instead of a mystery score.
- **Similarity matrix** — a heat grid over the whole corpus, useful for spotting near-duplicate notes. Capped at 40 passages.
- Corpus splitting by blank lines or single lines; nothing is uploaded, stored, or logged anywhere.

## Why

Every "semantic search" demo routes your text through someone's API. The embedding models small enough to run in a browser are genuinely good for personal-scale corpora (notes, docs, meeting minutes), and the whole pipeline — embed, normalize, cosine — is ~40 lines of real code. This page is that pipeline with no server in sight.

Built as the browser-side counterpart to the current wave of large multimodal embedding servers: same idea, zero infrastructure, zero cost, zero data exposure.

## Model

| component | provider | license |
|-----------|----------|---------|
| embeddings | [all-MiniLM-L6-v2](https://huggingface.co/Xenova/all-MiniLM-L6-v2) (sentence-transformers, ONNX q8) | Apache-2.0 |
| runtime | [transformers.js](https://github.com/huggingface/transformers.js) | Apache-2.0 |

~23 MB one-time download from the Hugging Face Hub, cached by the browser. WebGPU when available, WASM otherwise.

## Development

Nothing to build — open `index.html`, or:

```console
$ python3 -m http.server 8000
```

## Limitations

- 384-dim MiniLM is not a frontier embedder; it is strong for its size but expect noise on very short texts.
- English-centric model; other languages degrade.
- No persistence — reload clears the corpus by design.
