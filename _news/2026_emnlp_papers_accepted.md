---
layout: post
title: Papers to be presented at EMNLP 2026!
date: 2026-09-18 10:00:00+0300
inline: false
related_posts: false
---

We are delighted to share that several of our papers have been accepted at [**EMNLP 2026**](https://2026.emnlp.org/), **Findings of EMNLP**, [**ArabicNLP 2026**](https://arabicnlp2026.sigarab.org/), and co-located workshops in **Budapest, Hungary**. The papers cover multilingual and multimodal reasoning, speech language models, culturally grounded evaluation, harmful-content detection, and resilience against misinformation.

---

## EMNLP Main

### MemeLens: Multilingual Multitask VLMs for Memes

**Authors:** Ali Ezzat Shahroor, Mohamed Bayan Kmainasi, Abul Hasnat, Dimitar Iliyanov Dimitrov, Giovanni Da San Martino, Preslav Nakov, Firoj Alam <br/>
**Summary:** MemeLens consolidates 38 public meme datasets into a shared taxonomy of 20 tasks and trains a unified multilingual, multitask vision-language model that produces both predictions and explanations. The model substantially outperforms unimodal and zero-shot baselines, showing that joint image-text training and heterogeneous multitask data improve transfer, although humor, sarcasm, and cross-dataset generalization remain challenging. <br/>
**Paper:** [arXiv:2601.12539](https://arxiv.org/pdf/2601.12539) <br/>
**Data:** [QCRI/MemeLens](https://huggingface.co/datasets/QCRI/MemeLens) <br/>
**Model:** [QCRI/MemeLens-VLM](https://huggingface.co/QCRI/MemeLens-VLM)

---

### MENASpeechBank: A Reference Voice Bank with Persona-Conditioned Multi-Turn Conversations for SpeechLLMs

**Authors:** Zien Sheikh Ali, Hunzalah Hassan Bhatti, Rabindra Nath Nandi, Shammur Absar Chowdhury, Firoj Alam <br/>
**Summary:** MENASpeechBank provides about 18,000 high-quality utterances from 124 speakers across MENA countries and uses them in a controllable pipeline to create roughly 417,000 persona-conditioned, multi-turn conversations in English, Modern Standard Arabic, and regional Arabic varieties. Initial evaluations show strong transfer from synthetic to human speech for leading closed models, while fine-tuning substantially improves open models and audio-native systems do not consistently outperform ASR-to-LLM pipelines. <br/>
**Paper:** [arXiv:2602.07036](https://arxiv.org/pdf/2602.07036) <br/>
**Data:** [QCRI/MenaSpeechBank](https://huggingface.co/datasets/QCRI/MenaSpeechBank)

---

## Findings of EMNLP

### Multi-turn Conversational AI from Text to Multimodal Interaction: Data, Models, Evaluation, and Open Challenges

**Authors:** Syeda Faiza Ahmed Sara, Zien Sheikh Ali, Hunzalah Hassan Bhatti, Firoj Alam, Shammur Absar Chowdhury <br/>
**Summary:** This survey organizes multi-turn conversational AI across text, speech, vision, video, tool use, datasets, training strategies, and evaluation methods. It finds that modality support has advanced faster than session-level competence: current systems still struggle with persistent memory, cross-turn grounding, assumption revision, spoken timing, reproducible evaluation, and cultural alignment. <br/>
**Paper:** [arXiv:2608.17605](https://arxiv.org/pdf/2608.17605)

---

## ArabicNLP 2026

### AHA-Memes: A Fine-Grained Multimodal Benchmark for Hate Detection in Arabic Memes

**Authors:** Mohamed Bayan Kmainasi, Ali Ezzat Shahroor, Abul Hasnat, Md. Rafiul Biswas, Wajdi Zaghouani, Firoj Alam <br/>
**Summary:** AHA-Memes introduces 5,000 manually annotated Arabic memes with fine-grained labels for hatefulness, attack type, and target, together with about 66,000 silver-labeled examples for weak supervision. Experiments show that Arabic-specific text encoders provide strong baselines and visual features improve performance when fused with text, but implicit hate, rare categories, and longer dialectal text remain difficult even for large vision-language models. <br/>
**Paper:** [arXiv:2607.27393](https://arxiv.org/pdf/2607.27393)

---

### ImageEval 2026: Culturally Grounded Arabic Multimodal Evaluation

**Authors:** Samir Abdaljalil, Hunzalah Hassan Bhatti, Ahlam Bashiti, Farina Amir, Md Arid Hasan, Basel Mousi, Nadir Durrani, Fahim Dalvi, Zien Sheikh Ali, Erchin Serpedin, Hasan Kurban, Mustafa Jarrar, Shammur Absar Chowdhury, Firoj Alam <br/>
**Summary:** ImageEval 2026 combines spoken visual question answering and image-grounded hallucination detection in English and Modern Standard Arabic with an evaluation of cultural accuracy in text-to-image generation. Results from 14 teams show a substantial Arabic speech gap driven partly by ASR errors, demonstrate that task formulation strongly affects hallucination detection, and reveal that cultural image scores can be influenced by structural cues rather than direct visual assessment. <br/>
**Paper:** [arXiv:2608.30475](https://arxiv.org/pdf/2608.30475) <br/>
**Data:** [QCRI/ImageEval-ArabicNLP26](https://huggingface.co/datasets/QCRI/ImageEval-ArabicNLP26)

---

### ArGuard Shared Task: Harmful Content Detection in Arabic Memes and LLM Prompts

**Authors:** Firoj Alam, Md. Rafiul Biswas, Mohamed Bayan Kmainasi, Ali Ezzat Shahroor, Hamdy Mubarak, George Mikros, Abul Hasnat, Wajdi Zaghouani <br/>
**Summary:** ArGuard introduces a two-track benchmark for Arabic content safety: multimodal hateful-meme detection and harmful-prompt detection for LLM interactions. Each track evaluates both binary decisions and fine-grained categories, emphasizing Arabic-specific challenges such as dialect variation, sarcasm, code-switching, cultural references, and implicit harmful intent. <br/>
**Data:** [QCRI/ArGuard-Task1](https://huggingface.co/datasets/QCRI/ArGuard-Task1)

---

## 5th Workshop on NLP for Positive Impact

### CritiSense: Critical Digital Literacy and Resilience Against Misinformation

**Authors:** Firoj Alam, Fatema Ahmad, Ali Ezzat Shahroor, Mohamed Bayan Kmainasi, Elisa Sartori, Giovanni Da San Martino, Abul Hasnat, Raian Ali <br/>
**Summary:** CritiSense is a multilingual, mobile-first media-literacy app that teaches prebunking and practical verification skills through short lessons, quizzes, and explanation-rich feedback in nine languages. In a study with 93 users, 83.9% reported overall satisfaction and 90.1% rated the app as easy to use; a separate pre/post study found a 7.2-point absolute improvement in identifying misleading information. <br/>
**Paper:** [ACL Anthology](https://aclanthology.org/2026.acl-demo.48/) <br/>
**Website:** [CritiSense](https://critisense-web.digitqr.net/) <br/>
**iOS:** [Download on the App Store](https://apps.apple.com/sa/app/critisense/id6749675792) <br/>
**Android:** [Get it on Google Play](https://play.google.com/store/apps/details?id=com.critisense)

---

## SALMA 2026: Speech and Audio Language Models Workshop (2nd Edition)

### MENASpeechBank: A Reference Voice Bank with Persona-Conditioned Multi-Turn Conversations for SpeechLLMs

**Authors:** Zien Sheikh Ali, Hunzalah Hassan Bhatti, Rabindra Nath Nandi, Shammur Absar Chowdhury, Firoj Alam <br/>
**Summary:** MENASpeechBank provides about 18,000 high-quality utterances from 124 speakers across MENA countries and uses them in a controllable pipeline to create roughly 417,000 persona-conditioned, multi-turn conversations in English, Modern Standard Arabic, and regional Arabic varieties. Initial evaluations show strong transfer from synthetic to human speech for leading closed models, while fine-tuning substantially improves open models and audio-native systems do not consistently outperform ASR-to-LLM pipelines. <br/>
**Paper:** [arXiv:2602.07036](https://arxiv.org/pdf/2602.07036) <br/>
**Data:** [QCRI/MenaSpeechBank](https://huggingface.co/datasets/QCRI/MenaSpeechBank)

---

## ORACLE: The First Workshop on Open Reasoning Across Cultures and Languages

### Building Multimodal QA for Everyday Knowledge

**Authors:** Firoj Alam, Ali Ezzat Shahroor, Hunzalah Hassan Bhatti, Shammur Absar Chowdhury <br/>
**Summary:** This work presents a practical approach to building multimodal question answering resources around culturally grounded, everyday knowledge rather than simple object recognition. It brings together localized images, multilingual text, and spoken questions to evaluate whether models can combine visual evidence with commonsense and culture-specific context in realistic interactions.

---

## IMPACT-SPEECH: Identifying, Measuring, Preventing, and Assessing Consequences of Bias in Speech LLMs

### Multi-turn Conversational AI from Text to Multimodal Interaction: Data, Models, Evaluation, and Open Challenges

**Authors:** Syeda Faiza Ahmed Sara, Zien Sheikh Ali, Hunzalah Hassan Bhatti, Firoj Alam, Shammur Absar Chowdhury <br/>
**Summary:** This survey organizes multi-turn conversational AI across text, speech, vision, video, tool use, datasets, training strategies, and evaluation methods. It finds that modality support has advanced faster than session-level competence: current systems still struggle with persistent memory, cross-turn grounding, assumption revision, spoken timing, reproducible evaluation, and cultural alignment. <br/>
**Paper:** [arXiv:2608.17605](https://arxiv.org/pdf/2608.17605)

---
