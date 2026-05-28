---
layout: post
title: "Beyond MCQ: Open-Ended Arabic LLM Evaluation Presented at LREC 2026"
date: 2026-05-14 10:00:00-0400
inline: false
related_posts: true
---

We are happy to share that our work on **moving Arabic LLM evaluation beyond multiple-choice questions** was presented at **LREC 2026** in Palma, Mallorca.

<div style="text-align: center;">
  {% include figure.liquid loading="eager" path="assets/img/publication_preview/beyond_mcq_lrec_2026.jpeg" title="Beyond MCQ — LREC 2026 poster" class="img-fluid rounded z-depth-1" width="600px" %}
</div>

<br/>

### 🧭 Motivation

Many LLM benchmarks have relied heavily on multiple-choice questions, however, real users rarely interact with AI that way. They ask **open-ended questions**, switch dialects, expect context, and look for answers that reflect how language is actually used. [Hunzalah Hassan Bhatti](https://www.linkedin.com/in/hunz/) has been exploring this problem for **Arabic-centric LLM evaluation**, moving beyond MCQ accuracy toward open-ended benchmarking across **English, Modern Standard Arabic, and multiple Arabic dialects**.

### 🧪 Approach

The idea is practical and reusable: **extend existing benchmark resources rather than building everything from scratch**. The recipe includes:

- Converting multiple-choice questions into **open-ended questions**
- Involving **native-speaker annotators** for post-editing
- Generating **chain-of-thought rationales** to fine-tune smaller models for step-by-step reasoning

An interesting aspect of the developed dataset is that it is **parallel across multiple language varieties**, which makes the evaluation directly comparable.

### 🔑 Key Takeaways

- MCQ accuracy can make a model look stronger than it really is.
- **Cultural and linguistic competence** becomes clearer when models must answer openly — especially across dialects.
- Model capabilities can be improved by **ingesting dialectal knowledge**.

### 🔗 Resources

- 📄 **Paper:** [https://arxiv.org/pdf/2510.24328](https://arxiv.org/pdf/2510.24328)
- 🤗 **Dataset:** [QCRI/ArabicCulturalQA on Hugging Face](https://huggingface.co/datasets/QCRI/ArabicCulturalQA)
