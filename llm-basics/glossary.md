# LLM Basics — Glossary of shorthands

Every acronym and term of art used in the sessions, in one place. "S#" = the session where it
first appears. Numbers refer to the course's reference model, Llama 3 8B, unless stated.

| Short | Full name | Meaning | S# |
|---|---|---|---|
| LLM | large language model | the whole machine: the generation loop + weights | 0 |
| BGP | Border Gateway Protocol | inter-domain routing protocol (analogy source) | 1 |
| OSPF | Open Shortest Path First | intra-domain routing protocol (analogy source) | 1 |
| RIB | Routing Information Base | control-plane route database (analogy: training side) | 1 |
| FIB | Forwarding Information Base | fast-path forwarding table (analogy: weights) | 1 |
| LPM | longest prefix match | FIB lookup discipline (analogy: inference code) | 1 |
| OSI | Open Source Initiative | body whose definition most "open" AI licenses fail | 1 |
| RCE | remote code execution | why pickle-based .bin/.pt weight files are dangerous | 1 |
| mmap | memory map | load file by mapping bytes into address space, zero-copy | 1 |
| GGUF | GGML universal format | llama.cpp's weight file format | 1 |
| FP32 / FP16 / BF16 | 32/16-bit floating point, bfloat16 | number formats; bytes per weight (4/2/2) | 1 |
| INT8 / INT4 | 8-/4-bit integer | quantized weight formats (1 / 0.5 bytes per weight) | 1 |
| RAG | retrieval-augmented generation | fetch documents, splice into context per request | 1 |
| d_model | model dimension | width of each token's vector (Llama 3 8B: 4,096) | 2 |
| FFN | feed-forward network | same thing as MLP; also names its width (14,336) | 2 |
| MLP | multi-layer perceptron | per-token half of each layer; ~81% of each layer, ~70% of all weights | 2 |
| Q / K / V / O | query / key / value / output | the four attention projection matrices (K and V are 1,024 wide in Llama 3 8B because of GQA) | 2 |
| SiLU | sigmoid linear unit | the MLP's nonlinear gating function | 2 |
| FLOP | floating-point operation | one multiply or one add | 2 |
| MAC | multiply–accumulate | one multiply + one add on one fetched weight = 2 FLOPs | 2 |
| FMA | fused multiply–add | hardware instruction implementing a MAC in one clock | 2 |
| KV cache | key/value cache | saved per-token K,V so the loop doesn't redo old work (128 KB per token) | 0 |
| softmax | (not an acronym) | turns raw scores into probabilities summing to 1 | 0 |
| GPU | graphics processing unit | throughput processor that runs the matmuls | 2 (part 4) |
| VRAM | video random-access memory | GPU-attached memory holding weights + activations | 1 |
| HBM | high-bandwidth memory | the VRAM technology on datacenter GPUs (H100: ~3.35 TB/s) | 2 (part 4) |
| RLHF | reinforcement learning from human feedback | post-training: people rank answers, a reward model learns their preferences, the model is nudged toward higher-scored answers | 4 |
| BPE | byte-pair encoding | how the vocab is built: merge frequent pairs (Llama 3: 128,256 entries); a frozen compression dictionary | 0 |
| OOV | out-of-vocabulary | "unknown word" failure of older NLP; impossible with byte-level BPE | 0 |
| NLP | natural language processing | the field; pre-LLM era = hand-written rules | 0 |
| UTF-8 | Unicode transformation format, 8-bit | byte encoding of text; its 256 byte values are the tokenizer's floor | 0 |
| MoE | mixture of experts | many parallel MLP blocks per layer + learned router picks 1–2 per token; total params ≠ active params | 0 |
| logit | (not an acronym; from "log-odds") | raw pre-softmax score sᵥ = W_head[v]·h; unbounded, order matches probability order | 2 (part 3) |
| argmax | argument of the maximum | index of the largest value; greedy decoding = argmax over logits, no softmax needed | 2 (part 3) |
| T | temperature | divides logits before softmax, P ∝ e^(s/T); T→0 = greedy, high T = flatter distribution | 2 (part 3) |
| cross-entropy | (not an acronym) | training loss: −log P(correct next token); the reason softmax must exist for training | 2 (part 3) |
| ALU | arithmetic logic unit | the circuit that performs one multiply/add; a GPU has tens of thousands | 2 (part 4) |
| SRAM | static random-access memory | fast on-chip memory (registers, L1, L2); ~50 MB total on an H100 | 2 (part 4) |
| CPU | central processing unit | latency machine: few complex cores, speculation, branchy code | 2 (part 4) |
| PCIe | Peripheral Component Interconnect Express | host-to-GPU link, ~64 GB/s — 50× slower than HBM | 2 (part 4) |
| arithmetic intensity | (not an acronym) | FLOPs performed per byte fetched; ≈ positions per pass | 2 (part 4) |
| roofline | (not an acronym) | plot of attainable FLOP/s vs intensity; ridge at 295 on an H100 | 2 (part 4) |
| prefill | (not an acronym) | the one pass over the n known prompt tokens; builds the KV cache | 2 (part 4) |
| decode | (not an acronym) | the generation phase: one pass per output token, serial | 2 (part 4) |
| TTFT | time to first token | latency until the first output token appears; a prefill metric | 2 (part 4) |
| TPOT | time per output token | the steady drip rate during generation; a decode metric | 2 (part 4) |
| ARQ | automatic repeat request | stop-and-wait protocol family; the window=1 analogy for decode | 2 (part 4) |
| GQA | grouped-query attention | query heads share K,V sets (Llama 3 8B: 32 heads, 8 K/V sets) → KV cache 4× smaller (128 KB vs 512 KB per token) | 2 (part 4) |
| MHA | multi-head attention | the full-size variant where every head has its own K,V (would be 512 KB of KV per token at Llama 3 8B's size) | 2 (part 4) |
| prefix caching | (not an acronym) | reusing KV entries for an unchanged context prefix; API "cache read" | 2 (part 4) |
| PagedAttention | (not an acronym) | storing the KV cache in fixed-size blocks, like OS virtual-memory paging | 2 (part 4) |
| EOS | end of sequence | the special token whose sampling halts the generation loop | 2 (part 4) |
| diffusion | (not an acronym) | image generation by iterative denoising from noise; not next-token | 2 (part 4) |
| VAE | variational autoencoder | compresses/decompresses between pixels and diffusion latent space | 2 (part 4) |
| cross-attention | (not an acronym) | attention where Q comes from one stream and K,V from another (e.g. prompt → image) | 2 (part 4) |
| quantization | (not an acronym) | storing each weight in fewer bits by rounding it to a coarse grid | 2 (part 5) |
| scale | (not an acronym) | the float kept per group recording what one quantization step is worth: s = max\|w\|/levels | 2 (part 5) |
| dequantize | (not an acronym) | recovering a float from the stored integer, ŵ = s·q; restores the FORMAT, never the lost precision | 2 (part 5) |
| activation | (not an acronym) | any intermediate value computed during a forward pass (the per-token stream, h, K, V) — the data, as opposed to the weights | 2 (part 5) |
| W4A16 / W8A8 | Weights 4-bit, Activations 16-bit / both 8-bit | notation for which operands are narrowed; a tensor core needs BOTH narrow for its fast path | 2 (part 5) |
| PTQ | post-training quantization | round the finished weights, calibrating on a few hundred samples | 2 (part 5) |
| QAT | quantization-aware training | simulate the rounding during training so the network learns robust weights | 2 (part 5) |
| GPTQ | (from GPT + quantization) | rounds layer by layer to minimize the layer's OUTPUT error, not the weight error | 2 (part 5) |
| AWQ | activation-aware weight quantization | protects the weights that multiply large activations | 2 (part 5) |
| NF4 | NormalFloat-4 | 4-bit format with its 16 levels at a Gaussian's quantiles instead of uniformly spaced | 2 (part 5) |
| MXFP4 / MXFP8 | microscaling FP4 / FP8 | OCP formats where a block of ~32 small floats shares a power-of-two scale | 2 (part 5) |
| OCP | Open Compute Project | the body standardizing the MX microscaling formats | 2 (part 5) |
| SmoothQuant | (not an acronym) | migrates scale difficulty out of activations into weights, where it is tolerable | 2 (part 5) |
| LLM.int8 | (method name) | mixed decomposition: keep the ~0.1% outlier dimensions in FP16, the rest INT8 | 2 (part 5) |
| outlier features | (not an acronym) | the ~0.1% of activation dimensions running 20–100× larger above ~6.7B params | 2 (part 5) |
| ADC | analog-to-digital converter | the analogy for precision-to-integrate vs precision-to-report | 2 (part 5) |
| SGD | stochastic gradient descent | the training algorithm whose sampling noise makes weights robust to perturbation | 2 (part 5) |
| MoE | mixture of experts | (already listed) — note: "mixed" here means experts, NOT mixed precision | 2 (part 5) |
| attention | (not an acronym) | soft lookup: each word's question (Q) is matched against earlier words' labels (K); the result is a softmax-weighted blend of their payloads (V) | 3 |
| head (attention head) | (not an acronym) | one of 32 parallel lookups per layer, each with its own learned question; 4,096 = 32 heads × 128 | 3 |
| causal (masking) | (not an acronym) | each word may attend only to itself and earlier words, never later ones | 3 |
| O | output projection | learned matrix that merges the 32 heads' answers into one update per word | 3 |
| RoPE | rotary position embedding | rotates Q and K by an angle ∝ position, so match scores depend on distance between words | 3 |
| RIP | Routing Information Protocol | distance-vector routing (analogy source for layer stacking) | 3 |
| ACL | access control list | router table deciding whether a packet is allowed (analogy source for heads) | 3 |
| QoS | quality of service | traffic-priority handling (analogy source for heads) | 3 |
| DSCP | Differentiated Services Code Point | IP header field that QoS tables key on | 3 |
| loss | (not an acronym) | the training grade: −log(probability given to the real next word); same as cross-entropy | 4 |
| bit / nat (of loss) | (units) | surprise measured with log₂ (bits) or natural log (nats); 1 nat ≈ 1.44 bits; good models ≈ 2–2.6 bits/token | 4 |
| perplexity | (not an acronym) | 2^(bits per token): "as unsure as choosing among this many equally likely tokens" | 4 |
| proper scoring rule | (not an acronym) | a grading rule whose best average score comes from honest probabilities; log loss is one | 4 |
| self-supervised | (not an acronym) | the text grades itself (each word is the answer key for the words before it); no human labels | 4 |
| backpropagation (backprop) | (not an acronym) | the backward pass: one reverse sweep that gives every weight its gradient | 4 |
| gradient | (not an acronym) | a weight's sensitivity: how much the loss changes per small nudge of that weight | 4 |
| credit assignment | (not an acronym) | working out which of billions of weights was responsible for an error | 4 |
| learning rate | (not an acronym) | step size: how far each weight moves per update (update = step × error × sensitivity) | 4 |
| Adam / optimizer state | (Adam = adaptive moment estimation) | the usual training optimizer; keeps two running averages per weight (~8 bytes/param) | 4 |
| master weights | (not an acronym) | precise FP32 copy of the weights that tiny updates are added to; the 16-bit / 4-bit working copy is refreshed from it | 4 |
| mixed precision | (not an acronym) | matmuls in BF16/FP16 for speed, accumulation and updates in FP32 for accuracy | 4 |
| activation checkpointing | (not an acronym) | save only some layers' inputs and recompute the rest during backprop: less memory, ~1 extra forward pass | 4 |
| base model | (not an acronym) | a pretrained model before post-training; continues documents rather than answering | 4 |
| post-training | (not an acronym) | everything after pretraining: SFT, preference tuning, RL, safety training; usually several rounds | 4 |
| mid-training | (not an acronym) | continued pretraining on curated data (code, math, long documents); adds knowledge or context length | 4 |
| SFT | supervised fine-tuning | training on example conversations; teaches the assistant role and format | 4 |
| reward model | (not an acronym) | a scorer trained on people's A-vs-B picks; RLHF nudges the model toward answers it rates higher | 4 |
| DPO | direct preference optimization | preference tuning directly on A-vs-B pairs, without a separate reward model | 4 |
| RL | reinforcement learning | learning from a score rather than an answer key; e.g. rewards from passing tests or correct math answers | 4 |
| sycophancy | (not an acronym) | flattering / over-agreeing; a known side effect of RLHF because raters favor agreeable answers | 4 |
| distillation | (not an acronym) | a small student trained to match a big teacher's full probability spread (soft targets) instead of the answer key | 4 |
| teacher / student | (not an acronym) | the big model that supplies targets / the small model being trained on them | 4 |
| soft targets | (not an acronym) | the teacher's whole probability spread (mat 40%, rug 20% …) used as the grading target | 4 |
| pruning | (not an acronym) | removing less-important parts of a trained model to get a smaller starting point (e.g. Llama 3.2 1B/3B from 8B) | 4 |
| straight-through estimator | (not an acronym) | QAT trick: forward pass feels the rounding, backward pass treats rounding as if absent (rounding has no useful slope) | 4 |
| QAT (detail) | quantization-aware training | rounding inside training so errors cancel; nudges the precise master copy; usually the last step before shipping 4-bit; does NOT itself shrink bytes — rounding does | 4 |
| greedy decoding | (not an acronym) | always pick the single most likely token; deterministic, prone to repetition loops | 4 |
| sampling | (not an acronym) | pick the next token by dice weighted by the probabilities; breaks greedy's loops | 4 |
| temperature | (not an acronym) | (T above) how adventurous sampling is: low = near-greedy, high = more random | 4 |
| top-p (nucleus sampling) | (not an acronym) | sample only among the most likely tokens that together cover p (e.g. 90%) of the probability | 4 |
| repetition / frequency penalty | (not an acronym) | lowers the score of tokens already used, discouraging loops | 4 |
| TTL | time to live | packet hop limit that breaks routing loops (analogy for max tokens / sampling) | 4 |
| output head (LM head) | language-model head | runs once after the last layer: turns the last position's vector into one score per vocabulary word; NOT the same as attention heads | 4 |
| embedding lookup | (not an acronym) | runs once at the start: token ID → its row of 4,096 numbers; layers never look tokens up again | 4 |
| ReLU | rectified linear unit | classic MLP gate: negative → 0, positive passes (an ideal rectifier / diode) | 4 |
| SwiGLU | Swish-gated linear unit | the smoother gate in Llama-type MLPs; a third matrix decides how much each detector passes | 4 |
| system prompt | (not an acronym) | hidden operator instructions placed at the start of the context; prefilled with everything else | 4 |
| prompt caching | (not an acronym) | keeping earlier turns' KV cache so only new text is prefilled (same idea as prefix caching) | 4 |
| delta-sigma modulator | (EE term) | coarse-output converter whose integrator + feedback keep the average right (analogy for QAT) | 4 |
