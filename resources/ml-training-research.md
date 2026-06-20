# ML/AI Training Research — Distilled from Transcripts

> Synthesized from Stanford CS229 (Building LLMs), Karpathy's GPT-2 (124M) reproduction, Unsloth Studio fine-tuning guide, and Google's "From 46% to 90%: Fine-Tuning Tiny LLMs for On-Device Agents."

---

## Table of Contents

1. [LLM Training Pipeline Overview](#1-llm-training-pipeline-overview)
2. [Architecture: GPT-2 Deep Dive](#2-architecture-gpt-2-deep-dive)
3. [Tokenization](#3-tokenization)
4. [Data Pipeline](#4-data-pipeline)
5. [Scaling Laws & Compute Budgets](#5-scaling-laws--compute-budgets)
6. [Pretraining Details](#6-pretraining-details)
7. [Post-Training: SFT & RLHF/DPO](#7-post-training-sft--rlhfdpo)
8. [Evaluation](#8-evaluation)
9. [Systems & Optimization](#9-systems--optimization)
10. [Fine-Tuning for Domain Specialization](#10-fine-tuning-for-domain-specialization)
11. [On-Device / Tiny LLMs](#11-on-device--tiny-llms)
12. [Practical Reproduction: GPT-2 124M](#12-practical-reproduction-gpt-2-124m)
13. [Key Takeaways & The Bitter Lesson](#13-key-takeaways--the-bitter-lesson)

---

## 1. LLM Training Pipeline Overview

Five key components matter when training LLMs:

| Component | Description |
|-----------|-------------|
| **Architecture** | Transformer-based (decoder-only for GPT-family) |
| **Training Loss & Algorithm** | Cross-entropy / next-token prediction; AdamW optimizer |
| **Data** | Curated internet-scale corpora (~15T tokens for frontier models) |
| **Evaluation** | Perplexity, MMLU, ChatbotArena, AlpacaEval |
| **Systems** | GPU optimization, distributed training, low-precision arithmetic |

> **Key insight from CS229:** In practice, architecture and loss innovations matter least. **Data, evaluation, and systems** are what industry actually focuses on. Small architecture changes (activation functions, etc.) only shift the scaling law intercept — they don't change the slope. Training 10 hours longer or waiting for better GPUs has more impact.

---

## 2. Architecture: GPT-2 Deep Dive

### GPT-2 124M Specifications
- **12 transformer layers**, **768 embedding dimensions**
- **12 attention heads** (64 dimensions per head)
- **50,257 tokens** in vocabulary (50K BPE merges + 256 byte tokens + 1 special token)
- **Max sequence length:** 1,024 tokens
- **~124 million parameters**

### Key Architecture Differences from Original Transformer

1. **Decoder-only** — no encoder, no cross-attention (unlike original "Attention is All You Need")
2. **Pre-normalization** — LayerNorm applied *before* attention and MLP, not after
3. **Additional final LayerNorm** — added right before the classifier/head
4. **Learned positional embeddings** — sinusoidal patterns emerge during training from initially random values
5. **GELU activation** (approximate tanh version) in MLP — smoother than ReLU, no dead neuron problem

### Transformer Block = Map + Reduce
```
Input tokens
    ↓
[Token Embedding + Position Embedding]
    ↓
┌─────────────────────────────────┐
│  LayerNorm → Attention (REDUCE) │  ← tokens communicate
│         + residual              │
│  LayerNorm → MLP (MAP)          │  ← tokens think individually  
│         + residual              │
└─────────────────────────────────┘
    × 12 layers
    ↓
Final LayerNorm → Linear Head → Logits (vocab size)
```

### Why GELU over ReLU
- ReLU has a dead zone at zero → dead neurons can't recover
- GELU always contributes a local gradient → all neurons keep adapting
- Originally used `tanh` approximation because `erf` was slow in TensorFlow; no longer necessary

### Weight Tying
- Token embeddings (`wte`) and the final LM head share weights (tied)
- This reduces parameter count and improves performance

---

## 3. Tokenization

### Byte Pair Encoding (BPE)
1. Start with character-level tokens
2. Repeatedly merge the most frequent pair of adjacent tokens
3. Build vocabulary bottom-up from corpus statistics
4. Keep all sub-tokens (don't delete smaller ones) for fallback on typos/novel words

### Key Properties
- **Average token ≈ 3–4 characters**
- GPT-2 vocabulary: 50,257 tokens (50,000 merges + 256 byte-level + 1 `endoftext`)
- Pre-tokenizers handle spaces/punctuation before BPE (efficiency optimization)
- Always greedily choose the **largest** matching token when encoding

### Why Tokenizers Matter
- **Math:** Number "327" gets its own token — model can't generalize digit-by-digit arithmetic
- **Code:** Indentation (4 spaces) was poorly tokenized in older models; GPT-4 improved this
- **Cross-language:** Thai has no spaces between words — character-level issues
- **Future:** Some researchers advocate byte-level tokenization once quadratic attention is solved

> **Note:** Vocabulary size = output dimension of the language model. Different tokenizer sizes make perplexity comparisons meaningless across models.

---

## 4. Data Pipeline

### Scale
- Common Crawl: ~250 billion web pages, ~1 petabyte of raw data
- After filtering: ~15 trillion tokens for frontier models (Llama 3, GPT-4)
- That's roughly 100–1000× filtering of raw Common Crawl

### Pipeline Steps

```
Raw Internet (250B pages, ~1PB)
    ↓
1. TEXT EXTRACTION from HTML
   - Extract content from tags, handle math, remove boilerplate
    ↓
2. FILTER UNDESIRABLE CONTENT
   - Blacklist websites (NSFW, harmful, PII)
   - Train small classifiers for PII detection
    ↓
3. DEDUPLICATION
   - Remove repeated headers/footers
   - Remove duplicate URLs pointing to same content
   - Remove paragraphs repeated thousands of times (e.g., common book passages)
    ↓
4. HEURISTIC FILTERING
   - Outlier token distributions (unusual character frequencies)
   - Extremely short or long documents
   - Unusual word lengths
    ↓
5. MODEL-BASED FILTERING
   - Train classifier: Wikipedia-referenced URLs vs. random web
   - Prefer content similar to Wikipedia references (high quality signal)
    ↓
6. DOMAIN CLASSIFICATION & WEIGHTING
   - Classify: entertainment, books, code, academic, etc.
   - UPWEIGHT: code (improves reasoning), books, academic
   - DOWNWEIGHT: entertainment, low-quality forums
    ↓
7. QUALITY ANNEALING (end of training)
   - Decrease learning rate
   - Overfit on Wikipedia + high-quality human-collected data
```

### Data Mixing Matters More Than Architecture
- Training more on **code** improves general reasoning ability
- **Books** are consistently upweighted
- Domain mixing ratios significantly affect scaling behavior
- The optimal mixture is still an open research problem

### Team Size
- ~15 people on data for a team of ~70 (Llama-scale)
- Data work requires significant CPU compute for processing at scale

---

## 5. Scaling Laws & Compute Budgets

### The Fundamental Scaling Law
On log-log scale, test loss decreases **linearly** with:
- **Compute** (FLOPs)
- **Data** (number of tokens)
- **Parameters** (model size)

This means performance improvements are **predictable** from smaller-scale experiments.

### Chinchilla Optimal Ratios
| Metric | Training Optimal | Inference Optimal |
|--------|------------------|-------------------|
| Tokens per parameter | **20:1** | **150:1** |

- **Training optimal:** Minimize loss for a fixed compute budget
- **Inference optimal:** Prefer smaller models (cheaper to serve), so use more tokens per parameter

### Practical Implications
- **New training pipeline (scaling laws):**
  1. Find a scaling recipe (e.g., "larger model → lower learning rate")
  2. Hyperparameter tune on **small models** at different scales
  3. Fit scaling law, extrapolate to target size
  4. Train final large model with full compute budget
- **Old pipeline:** Tune hyperparameters directly on the large model → wastes most compute

### Architecture Comparison via Scaling Laws
- Train both Transformers and LSTMs at multiple scales
- Plot scaling curves; the one with better slope wins at scale
- Transformers have better slope AND better intercept than LSTMs
- Small architectural tweaks (activation functions) mostly change the **intercept**, not the slope

### No Evidence of Plateau (Yet)
- Scaling laws still hold as of 2025/2026
- Mathematically, log-scale lines can continue indefinitely
- Most researchers expect eventual plateau, but no empirical evidence yet

### Compute Costs (Llama 3 405B as example)
| Item | Estimate |
|------|----------|
| Parameters | 405 billion |
| Training tokens | 15.6 trillion |
| FLOPs | ~3.8 × 10²⁵ |
| GPU hours | ~26–30 million (16K H100s × 70 days) |
| GPU rental cost | ~$52M |
| Team salaries (~50 people) | ~$25M |
| **Total** | **~$75M** |
| CO₂ emitted | ~4,000 tons (≈ 2,000 JFK↔London round trips) |

> Each new generation aims for ~10× more FLOPs than the previous one.

---

## 6. Pretraining Details

### Task
- **Autoregressive language modeling:** predict the next token given all previous tokens
- Loss: **cross-entropy** between predicted distribution and one-hot target
- Equivalent to **maximizing log-likelihood** of the training text

### Loss Function
```
L = -∑ log P(x_t | x_1, ..., x_{t-1})
```
Minimizing cross-entropy loss = maximizing probability of the actual text.

### Optimizer: AdamW
- Maintains per-parameter buffers: momentum (1st moment) and variance (2nd moment)
- Normalizes each gradient element individually → faster convergence
- **Must zero gradients before each step** (`.backward()` accumulates with `+=`)

### Training Loop
```python
optimizer.zero_grad()
logits, loss = model(x, y)      # forward pass
loss.backward()                   # compute gradients (accumulates!)
optimizer.step()                  # update parameters
```

### Weight Initialization
- Critical for training stability
- GPT-2 uses residual scaling: multiply residual connections by `1/√(2·n_layers)`
  - Prevents residual stream from growing too large deep in the network
  - Each layer contributes smaller residual as depth increases

---

## 7. Post-Training: SFT & RLHF/DPO

### Why Post-Training?
Pretrained LLMs are **text completers**, not **assistants**. If you ask GPT-3 "Explain the moon landing to a six-year-old," it generates more questions instead of answering — because that's what follows questions on the internet.

### Stage 1: Supervised Fine-Tuning (SFT)

- **Same loss** as pretraining (cross-entropy / next-token prediction)
- **Different data:** human-written Q&A pairs showing desired assistant behavior
- **Different hyperparameters:** higher learning rate (~1e-5), fewer epochs
- Key insight: **SFT doesn't teach new knowledge** — it selects which "user persona" the model should optimize for from its pretrained distribution

#### How Much Data for SFT?
- **Surprisingly little:** LIMA paper showed 2,000 examples ≈ 32,000 examples
- Scaling SFT data beyond ~2K doesn't help much
- Pretrained model already has the knowledge; SFT just teaches format/style

#### Synthetic Data for SFT
- Use powerful model (e.g., text-davinci-003, Claude Opus) to generate Q&A pairs
- **Alpaca (Stanford):** 175 human examples → 52K LLM-generated examples → fine-tune LLaMA 7B
- Cost: orders of magnitude cheaper than human annotation
- **Limitation:** Can't bootstrap forever (model collapse over generations)
- **Solution:** Human-in-the-loop: LLM generates drafts, humans edit (faster than writing from scratch)

### Stage 2: RLHF (Reinforcement Learning from Human Feedback)

#### Problem with SFT
1. **Bounded by human generation ability** (humans can rank better than they can write)
2. **Hallucination risk:** Model learns to generate plausible-sounding but unverifiable claims
3. **Expensive:** Writing ideal answers is slow

#### RLHF Pipeline
```
1. Generate two answers per instruction (from SFT model)
2. Humans label which answer is better (preference pairs)
3. Train reward model (Bradley-Terry / softmax loss)
4. Optimize LLM policy against reward model using PPO
   - Regularize against overoptimization (KL penalty)
```

#### DPO (Direct Preference Optimization) — Now Standard
- **Simpler than PPO:** No reward model needed, no RL needed
- Loss: Maximize likelihood of preferred outputs, minimize non-preferred
- **Equivalent global minimum** to PPO under certain assumptions
- Much easier to implement and tune
- Now the standard in both open-source and industry

```
DPO Loss: maximize log P(preferred | input) - log P(non-preferred | input)
```

#### Human Labeling Challenges
- Humans agree with each other only ~66% of the time on binary preference tasks
- **Models are cheaper AND more consistent** labelers (50× cheaper, higher agreement)
- But models have biases: prefer longer outputs, verbose style
- Combined human + LLM labeling is the emerging standard

#### RLHF Side Effects
- Models become longer-winded (RLHF optimizes for length because humans prefer longer answers)
- After PPO, models are no longer calibrated probability distributions — they're policies
- Perplexity becomes meaningless for aligned models

---

## 8. Evaluation

### Pretraining Evaluation
- **Perplexity:** 2^(average per-token loss)
  - Range: 1 (perfect) to vocabulary size (no knowledge)
  - Intuition: "number of tokens the model is hesitating between"
  - Went from ~70 to <10 between 2017–2023
  - **Not comparable across models** (depends on tokenizer vocabulary size)
  - Still used internally for development

### Academic Benchmarks
- **MMLU:** Multi-task language understanding across domains
  - Multiple choice: compare likelihood of A/B/C/D answers
  - Can constrain model to only output answer tokens
- **HELM (Stanford)** and **Hugging Face Open Leaderboard** aggregate many benchmarks
- **Challenge:** Different evaluation methods give wildly different scores for same model
  - LLaMA-65B: 63.7% on HELM vs. 48.8% on another benchmark

### Train-Test Contamination
- Can the model's test set have been in the training data?
- Detection trick: if model is more likely to generate test examples **in order** than shuffled, it memorized the ordering → was in training data

### Post-Training Evaluation
- **ChatbotArena:** Blind A/B testing with real users, ELO-based ranking
- **AlpacaEval:** LLM-as-judge, <3 min and <$10 per evaluation
  - 98% correlation with ChatbotArena
  - **Bias:** LLMs (and humans) prefer longer outputs
  - **Fix:** Use regression/causal inference to control for length

---

## 9. Systems & Optimization

### GPU Fundamentals
- GPUs: optimized for **throughput** (parallel operations on many cores)
- CPUs: optimized for **latency** (fast single operations)
- **Matrix multiplication is king:** Everything on GPU should be expressed as matmul
- **Memory hierarchy:** closer to cores = less memory but faster
- **Communication bottleneck:** compute improves faster than memory/bandwidth
- **Model FLOP Utilization (MFU):** ~45-50% is excellent (Llama: ~45%)
  - Most GPUs sit idle waiting for data

### Low Precision Training (Mixed Precision)
```
Weights stored:   32-bit (preserve small updates)
Computation:      16-bit (faster matmul, lower memory)
Weight updates:   32-bit (learning rate changes are small)
```
- **Automatic Mixed Precision (AMP):** Hot-swaps between 16/32 bit
- ~2× speedup, same model quality

### Operator Fusion
- Naive: each PyTorch op → ship data to GPU → compute → ship back → next op
- **Fused kernels:** ship once → do all operations → ship back once
- `torch.compile()` automatically fuses operations → ~2× speedup
- Rewrites PyTorch code in C++/CUDA under the hood

### Distributed Training
- **Data Parallel (DDP):** Each GPU gets different batch, averages gradients
- **Tensor Parallel:** Split individual layers across GPUs
- **Pipeline Parallel:** Split layers across GPUs sequentially
- **Mixture of Experts (MoE):** Only activate subset of parameters per input

---

## 10. Fine-Tuning for Domain Specialization

### Unsloth Studio
- **Open-source, free** local fine-tuning tool
- Built by former NVIDIA engineer (debugged Llama and Qwen models)
- Dynamic 2.0 quantization per layer: shrinks model size while preserving accuracy
- Also functions as local model chat (competitor to Ollama/LM Studio)

### Dataset Requirements
| Type | Description |
|------|-------------|
| **Instruction Q&A (single turn)** | Most common; instruction → output pairs |
| **Conversational** | Multi-turn chat exchanges |
| **Domain expert** | Specialized niche data |
| **Reasoning/tool use** | Chain of thought, function calls |

- **Minimum:** 200–300 examples
- **Recommended:** 1,000+ examples for meaningful improvement
- **Large datasets:** 68K+ examples (like Finance Alpaca)

### Creating Custom Datasets
1. **From PDFs:** Upload a PDF → LLM generates Q&A pairs from chunks
2. **From GitHub:** Crawl issues and PRs → create instruction/answer pairs
3. **Best practice:** Use a powerful but cheap model for generation
   - **DeepSeek V4 Pro:** Best cost/quality for dataset generation
   - Much cheaper than Opus/Sonnet for thousands of API calls

### Distillation Pattern (Industry Standard)
```
Powerful Model (Opus/GPT-5) → Generate high-quality outputs
    ↓
Train smaller model on those outputs
    ↓
Small model approaches large model's quality for specific domain
```
- "The unspoken secret of the AI industry"
- Check ToS of model providers before distilling

### Key Fine-Tuning Parameters
- **Context length:** 1,024 for faster training runs
- **Batch size:** 1 for minimal hardware; scale up for production
- **Training steps:** More = better (20 for demo, thousands for production)
- **Multiple epochs:** Let it run for hours, not minutes
- **Training loss should decrease** — if not, the model isn't learning

### Hardware Considerations
- **Qwen 3.6 27B** (51 GB): Needs ≥32 GB VRAM, may fail on Apple Silicon (MLX bug)
- **Qwen 3.5 9B**: Runs on most modern hardware
- Use `safe_tensors` format for training (not GGUF — GGUF is inference-only)
- GGUF: compressed, single-file format for inference on consumer hardware
  - LlamaCPP framework; created by Georgi Gerganov (now at HuggingFace)

---

## 11. On-Device / Tiny LLMs

### Google AI Edge Stack
- **MediaPipe:** Vision/audio processing
- **LiteRT LLM:** Runtime for on-device LLM inference
- **LiteRT (formerly TensorFlow Lite):** Cross-framework runtime
- Runs on CPU, GPU, or NPU depending on hardware
- Supports 2.7+ billion devices

### Two Approaches to On-Device AI

| Approach | System GenAI | App GenAI |
|----------|-------------|-----------|
| **Model** | Pre-installed (Gemini Nano) | Bundled with app |
| **Size** | Small (Gemma 4 E2B/E4B) | Varies (tiny to small) |
| **Optimization** | Highly optimized | More customization |
| **App size** | No increase | Increases app size |
| **Effort** | Minimal | More work |

### Agent Skills On-Device
- Prompt-based skill harness on top of Gemma 4
- Skills described in system prompt; full implementation loaded on demand
- `load_skill` tool call — model decides which skill to use per query
- Skills can include custom JavaScript for UI rendering
- ~8 skills work reliably with 4B model in single conversation
- **Challenge:** Multi-skill per single turn is less robust (still working on it)

### Function Gemma (270M Parameters)
- Robust function calling from Gemma 3 technology
- **Without fine-tuning:** 46% success rate on app intents
- **With fine-tuning on synthetic data:** 90%+ success rate (8/10 functions)
- Workflow: Use Flash model to generate synthetic dataset → fine-tune Function Gemma
- 2,000 tokens/sec prefill, 140 decode on Pixel 7

### Tiny Model Pipeline
```
HuggingFace model → Export with LiteRT Torch → Deploy with LiteRT LLM
                                              → Or test on desktop with LiteRT Run
```
- Pre-built models available for visual language, transcription, etc.
- Fine-tuning critical for 100M–200M parameter models (narrow, focused tasks)

### Production Example: Eloquent (Transcription App)
- ASR engine (Gemma 3-based, few hundred million params)
- Text polishing engine (Gemma 3-based, few hundred million params)
- Chained together: compelling offline transcription with personalization
- Polishing removes ums/ahs, applies custom keyword dictionary

---

## 12. Practical Reproduction: GPT-2 124M

### What It Takes
- **Cost:** ~$10 on cloud GPU
- **Time:** ~1 hour (or less)
- **Surpasses** original OpenAI GPT-2 124M quality

### Implementation (Karpathy)
- ~100 lines of clean PyTorch (vs. 2,000 lines in HuggingFace Transformers)
- Load pretrained weights from HuggingFace to validate implementation
- Then train from scratch on new data

### Key Implementation Details
1. **Weight naming** must match HuggingFace schema for easy weight transfer
2. **Some weights transposed** from TensorFlow format (legacy issue)
3. **Autoregressive mask** (causal attention) — tokens only attend to past
4. **Top-k=50 sampling** for generation (HuggingFace default)
5. **`torch.no_grad()`** during generation — saves memory by not caching for backprop

### Training Data
- FineWebEdu dataset (filtered high-quality web text)
- Tokenized with tiktoken (GPT-2 tokenizer)
- ~3:1 compression ratio (characters to tokens)

### AdamW Optimizer Details
- Per-parameter momentum and variance buffers
- Normalizes gradient elements individually
- **Always zero gradients before each step** — `.backward()` does `+=`

---

## 13. Key Takeaways & The Bitter Lesson

### Richard Sutton's Bitter Lesson (2019)
> With scale, you get better models. Moore's law guarantees more compute over time. Therefore, only **architectures that leverage computation** matter. Systems, data, and scale trump clever algorithmic tricks.

### Practical Priorities (Ordered)

1. **Data quality and curation** — the biggest competitive advantage
2. **Systems engineering** — squeeze maximum utilization from hardware
3. **Scaling compute** — more GPUs, more tokens, more parameters
4. **Training recipes** — learning rate schedules, mixed precision, distributed training
5. **Post-training alignment** — SFT + DPO with curated preference data
6. **Architecture innovations** — least impactful; mostly shifts scaling curve intercept

### The Data Moat
- Custom datasets + fine-tuned models = defensible competitive advantage
- Creating proprietary data pipelines is more valuable than architecture research
- Companies are secretive about data precisely because it's the key differentiator

### Cost Reality Check
| Model | Training Cost | Data |
|-------|--------------|------|
| GPT-2 124M (reproduce) | ~$10 | ~9B tokens |
| Llama 3 405B | ~$75M | 15.6T tokens |
| GPT-4 (estimated) | ~$100M+ | ~13T tokens |
| Next generation | ~10× each | Growing |

### Open Problems
- **Data wall:** Running out of high-quality internet text
- **Synthetic data limits:** Can't bootstrap indefinitely without model collapse
- **Evaluation:** No reliable automated metric for open-ended generation
- **Copyright/liability:** Legal landscape still evolving
- **Carbon emissions:** Not critical yet (~4K tons), but will scale ~10× per generation

---

## Sources

1. **Stanford CS229 — Building Large Language Models (LLMs)** — Tatsunori Hashimoto, ~1:44:31
2. **Let's reproduce GPT-2 (124M)** — Andrej Karpathy, ~4:01:26
3. **Unsloth Studio: Fine-tune any AI model locally** — ~32:56
4. **From 46% to 90%: Fine-Tuning Tiny LLMs for On-Device Agents** — Cormac Brick, Google, ~21:00

---

*Last updated: 2026-06-20*