<div align="center">

# FIND

### Toward Multimodal Financial Reasoning and Question Answering for Indic Languages

**Bridging finance, vision, language, and numerical reasoning**

**Sarmistha Das\*** · **Vaibhav Vishal\*** · **Syed Ibrahim Ahmad\*** · Manish Gupta · Sriparna Saha

Indian Institute of Technology Patna, India · Microsoft, India

<sub>*Equal contribution (as stated in the published PDF).</sub>

[![ACL 2026 Findings](https://img.shields.io/badge/ACL_2026-Findings-173B57?style=for-the-badge)](https://aclanthology.org/2026.findings-acl.1639/)
[![Paper PDF](https://img.shields.io/badge/Paper-PDF-287D77?style=for-the-badge)](https://aclanthology.org/2026.findings-acl.1639.pdf)
[![DOI](https://img.shields.io/badge/DOI-10.18653%2Fv1%2F2026.findings--acl.1639-526478?style=for-the-badge)](https://doi.org/10.18653/v1/2026.findings-acl.1639)

[Abstract](#abstract) · [Benchmark](#finvqa-benchmark) · [Framework](#find-framework) · [Results](#experimental-highlights) · [Resources](#resources-and-reproducibility) · [Citation](#citation)

---

| **18,900** | **6** | **14** | **4** | **3** | **9** |
| :---: | :---: | :---: | :---: | :---: | :---: |
| QA instances | Languages | Domains | Question formats | Difficulty levels | VLM configurations |

**English · हिन्दी · বাংলা · मराठी · ગુજરાતી · தமிழ்**

</div>

## Abstract

**Can vision–language models reason reliably about financial information across linguistically diverse and multimodal contexts?**

Financial intelligence requires more than language understanding—it demands numerical precision, domain-specific knowledge, and faithful interpretation of visual evidence. Existing financial reasoning benchmarks remain predominantly English-centric, overlooking the linguistic diversity of real-world financial environments.

We introduce **FinVQA**, a multilingual financial reasoning benchmark comprising **18,900 instances across 14 financial domains**, covering English and five major Indic languages: Hindi, Bengali, Marathi, Gujarati, and Tamil. FinVQA integrates textual and visual financial information across three difficulty levels and four question formats.

Building upon this benchmark, we propose **FIND**, a financial reasoning framework integrating parameter-efficient supervised fine-tuning and language-aware constrained decoding to improve answer accuracy and structural consistency.

Our experiments across Qwen2.5-VL, Qwen3-VL, and Gemma 3 reveal persistent challenges in cross-lingual numerical reasoning, visual grounding, and mathematical execution, highlighting the importance of supervised alignment for multilingual financial intelligence.

Together, **FinVQA and FIND** advance the development and evaluation of reliable, inclusive, and multilingual financial AI systems.

<sub>Adapted from the published abstract and experimental findings. See the <a href="https://aclanthology.org/2026.findings-acl.1639/">ACL Anthology</a> for the original abstract.</sub>

## FinVQA benchmark

<div align="center">
  <img src="assets/finvqa-overview.png" width="92%" alt="FinVQA examples covering multiple choice, fill in the blanks, true or false and table matching">
  <br><sub>Figure 1 from the paper — representative questions across four formats.</sub>
</div>

FinVQA is designed to evaluate financial knowledge, numerical computation, and visual–textual reasoning in multilingual settings. Its source questions are drawn from NCERT educational materials and the ICMAI–CMA program. The benchmark contains approximately 3,150 underlying source questions translated across six languages.

### Dataset at a glance

| Dimension | Coverage |
| :--- | :--- |
| Total instances | 18,900 |
| Text-only | 15,114 (80.0%) |
| Text + image | 3,786 (20.0%) |
| Difficulty | Easy: 6,282 · Moderate: 6,384 · Hard: 6,234 |
| Formats | Multiple choice · Fill-in-the-blank · Table matching · True/false |
| Languages | English, Hindi, Bengali, Marathi, Gujarati, Tamil |


### Multilingual visual grounding

<div align="center">
  <img src="assets/multilingual-ocr.png" width="94%" alt="OCR-based translation of an English chart into five Indic languages">
  <br><sub>Figure 6 from the paper — OCR-based text replacement preserves the visual layout across languages.</sub>
</div>

The multilingual curation pipeline uses GPT-4o translation, back-translation, multilingual sentence-embedding similarity, native-speaker review, and OCR-based text replacement to preserve the semantic and numerical content of questions and visual material.

## FIND framework

<div align="center">
  <img src="assets/find-framework.png" width="96%" alt="FinVQA dataset construction and FIND framework with zero-shot, constrained decoding and supervised fine-tuning">
  <br><sub>Figure 4 from the paper — dataset construction (left) and the three experimental variations of FIND (right).</sub>
</div>

| Variation | Approach | Output |
| :--- | :--- | :--- |
| I | Zero-shot inference with a pretrained VLM | Reasoning and predicted answer |
| II | Language-aware constrained decoding (vLLM) | Valid answer option |
| III | Answer-only SFT with LoRA, followed by constrained decoding | Valid answer option |

FIND adapts query, key and value attention projections using LoRA (rank 8, alpha 32, dropout 0.1). The constrained decoder limits predictions to valid option tokens, accounting for language-dependent tokenization. The SFT variant uses answer-only supervision rather than explicit reasoning-trace supervision.

## Experimental highlights

The paper evaluates three model families at multiple parameter scales: **Qwen2.5-VL (3B, 7B, 32B)**, **Qwen3-VL (4B, 8B, 32B)**, and **Gemma 3 (4B, 12B, 27B)**. Answer accuracy is the primary automatic evaluation metric.

<div align="center">
  <img src="assets/sft-results.png" width="96%" alt="Language-wise accuracy of supervised fine-tuned models for text-only and multimodal questions">
  <br><sub>Figure 5 from the paper — SFT performance across languages and modalities.</sub>
</div>


**Key observations:** Constrained decoding improves structural validity; supervised fine-tuning often strengthens Indic-language answer accuracy; and visual extraction and multi-step arithmetic remain challenging, particularly for smaller models. Human evaluation in the zero-shot setting measures six dimensions, including financial understanding, mathematical correctness, formula application, reasoning consistency, problem interpretation, and formatting.

## Resources and reproducibility

- [Published paper and metadata](https://aclanthology.org/2026.findings-acl.1639/)
- [Full PDF](https://aclanthology.org/2026.findings-acl.1639.pdf): dataset construction (Section 3), modeling (Section 4), results (Section 5), and prompts (Appendix C).
- [Repository](https://github.com/sarmistha-D/FIND_FinVQA): see the repository files for available implementation and dataset resources.

<details>
<summary><b>Training configuration reported in the paper</b></summary>

| Setting | Value |
| :--- | :--- |
| Epochs | 3 |
| Learning rate | 1e-5 |
| Optimizer | 8-bit AdamW |
| LoRA rank / alpha / dropout | 8 / 32 / 0.1 |
| Hardware | 1 NVIDIA A100 80 GB GPU |
| Supervision | Correct answer sequence |

</details>

<!-- MAINTAINERS: Once code and data are uploaded, add actual paths, exact environment requirements, runnable training/inference commands, dataset access URL and applicable licenses here. Do not advertise unverified releases. -->

## Intended use and limitations

FinVQA and FIND are intended for research and evaluation, not personalized financial advice. The multilingual instances are translations of approximately 3,150 underlying source questions; they should not be interpreted as 18,900 independently authored questions. Valid answer formatting alone does not guarantee correct numerical reasoning. See Sections 7–8 of the paper for additional limitations and ethical considerations.

## Citation

If you use FinVQA or FIND in your research, please cite our paper. Author order below follows the published PDF.

```bibtex
@inproceedings{das-etal-2026-find,
  title     = {{FIND}: Toward Multimodal Financial Reasoning and Question Answering for {Indic} Languages},
  author    = {Das, Sarmistha and Vishal, Vaibhav and Ahmad, Syed Ibrahim and Gupta, Manish and Saha, Sriparna},
  booktitle = {Findings of the Association for Computational Linguistics: ACL 2026},
  year      = {2026},
  month     = jul,
  pages     = {32750--32770},
  publisher = {Association for Computational Linguistics},
  address   = {San Diego, California, United States},
  doi       = {10.18653/v1/2026.findings-acl.1639},
  url       = {https://aclanthology.org/2026.findings-acl.1639/}
}
```


<div align="center">

**FIND · FinVQA**  
Financial reasoning beyond linguistic boundaries.

</div>
