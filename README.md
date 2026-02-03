# Gengram: Retrieval-Augmented Genomic Foundation Models

<div align="center" style="line-height: 2;">
  <a href="https://huggingface.co/noname20260128/Gengram-10B-L3_6_10-W21-8k" target="_blank">
      <img alt="Hugging Face" src="https://img.shields.io/badge/🤗%20Hugging%20Face-Gengram%20-ffc107"/>
  </a>

</div>



## 1. Introduction

Gengram is a novel conditional memory module designed for genomic foundation models (GFMs) that introduces explicit motif memory retrieval to enhance Transformer-based DNA sequence modeling. Unlike traditional GFMs that rely on dense computation to implicitly infer multi-nucleotide motifs, Gengram provides an efficient lookup mechanism for biological patterns through a genomic-specific hashing scheme.

Figure 1 illustrates the overall architecture of Gengram, together with the evaluation pipeline used to assess its effectiveness across multiple genomic benchmarks.

![Gengram](./images/gengram_model.png)

### ✨ Key Features

- **🎯 Explicit Motif Memory**: Stores and retrieves k-mers (k=1-6) via hash-based lookup tables
- **🧬 Local Window Aggregation**: 21bp window mechanism aligned with DNA helical structure
- **⚡ Computational Efficiency**: Linear time complexity with minimal overhead
- **🔧 Architecture Agnostic**: Compatible with various attention mechanisms (MHA, GQA, MLA)
- **⚖️ Stable Training**: Improves load balancing in Mixture-of-Experts models
- **🔍 Biological Interpretability**: Learns meaningful motif representations

### ✨ Biological Interpretability
Gengram exhibits clear biologically grounded behaviors, including:
- **Reverse-complement symmetry** in memory embeddings
- **Context-dependent gating** aligned with functional regions
- **Hierarchical representation** from shallow to deep layers

## 2. Model Information

### Model Configuration

The following details the model configuration, including the parameterization of Gengram, MoE routing strategies, and training hyperparameters used across all experiments.

- **Gengram Parameters**

  These parameters control how Gengram operates within the Transformer layers, including which layers to apply it to, the n-gram sizes, and embedding dimensions.
<div align="center">

| Parameter | Description | Example |
|-----------|-------------|---------|
| `--gengram-enabled` | Enable Gengram | `true` |
| `--gengram-layer-ids` | Layers to apply Gengram | `3 6 10` |
| `--gengram-ngram-sizes` | N-gram sizes for DNA processing | `1 2 3 4 5 6` |
| `--gengram-embed-dim-per-ngram` | Embedding dimension per n-gram | `1024` |
| `--gengram-window-size` | window size | `21` |

</div>

- **Mixture of Experts (MoE)**

  These parameters define the Mixture-of-Experts architecture, including the number of experts, routing top-k, and load balancing strategies during training.

<div align="center">

| Parameter | Description | Default |
|-----------|-------------|---------|
| `--num-experts` | Number of experts | `8` |
| `--moe-router-topk` | Top-k experts to route to | `2` |
| `--moe-router-load-balancing-type` | Load balancing strategy | `aux_loss` |
| `--moe-aux-loss-coeff` | Auxiliary loss coefficient | `1e-3` |

</div>

- **Training Parameters**

  These parameters specify the training setup, including sequence length, batch sizes, precision, and attention optimizations.

<div align="center">

| Parameter | Description | Example |
|-----------|-------------|---------|
| `--seq-length` | Maximum sequence length | `8192` |
| `--micro-batch-size` | Micro batch size per GPU | `1` |
| `--global-batch-size` | Global batch size across all GPUs | `1024` |
| `--bf16` | Use BF16 precision | `true` |
| `--use-flash-attn` | Enable Flash Attention | `true` |

</div>

### Pre-training Data
- **Human Sequences**: HPRC Release 2, GRCh38, CHM13
- **Non-human Primates**: NCBI RefSeq database
- **Total**: 200B tokens (8k context) + 100B tokens (32k context)

## 3. Performance Evaluation

Gengram demonstrates strong performance across multiple genomic benchmarks, achieving competitive results despite being trained on significantly fewer tokens and with a smaller model size.

<div align="center">

| Metric | [Gengram-10B](https://huggingface.co/noname20260128/Gengram-10B-L3_6_10-W21-8k) | [Genos-10B](https://huggingface.co/BGI-HangzhouAI/Genos-10B) | [Evo2-40B](https://huggingface.co/arcinstitute/evo2_40b) |
|--------|:-------------:|:-----------:|:----------:|
| **Trained Tokens** | 200B | 2.2T | 9.3T |
| **Multi-species Exon Classification** | **0.9832** | 0.9755 | 0.9332 |
| **Splice Site Identification** | 0.9009 | 0.7990 | **0.9138** |
| **Human OCR Ensembl** | **0.7714** | 0.7623 | 0.7635 |

</div>

- **Key Observations**

  - Data Efficiency: Achieves comparable performance using ~10×–40× fewer tokens
  - Motif-Dominated Tasks: Up to 14% improvement
  - Long-Context Modeling: Enhanced performance with shorter sequences
  - Training Efficiency: Better parameter utilization and stable MoE training

- **Evaluation Benchmarks**
  - Genomic Benchmarks (GB)
  - Nucleotide Transformer Benchmarks (NTB)
  - Long-Range Benchmarks (LRB)
  - Genos Benchmarks (GeB)

## 4. Quickstart

### Model Download
Gengram model is available for download from Hugging Face. We provide `torch` version.

<div align="center">

| **Model** | **Activated Params** | **Hugging Face** | **Format** 
|:---------:|:----------------:|:----------------:|:----------------:|
| Gengram-10B | 2.87 B | [🤗 Hugging Face](https://huggingface.co/noname20260128/Gengram-10B-L3_6_10-W21-8k) | torch |


</div>

### Pre-training

Run the pre-training script with the following command:

```bash
cd Gengram
bash Gengram_layer3-6-10_win21_pp2.sh
```

## 5. License

This repository and the Gengram model weights are licensed under the [Apache License 2.0](LICENSE). 

Please note that the primary use of Gengram model is to support genomics research, providing researchers with advanced analytical capabilities and long-context modeling tools powered by large-scale foundation models for the human genome. It is not intended for use in any manner that violates applicable laws or regulations, nor for any activities prohibited by the license agreement.

