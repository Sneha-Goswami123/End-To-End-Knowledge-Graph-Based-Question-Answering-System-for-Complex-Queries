# 🚀 Knowledge Graph Question Answering (KGQA) without Gold Entities

## 📌 Overview
This project presents an end-to-end **Knowledge Graph Question Answering (KGQA)** system designed to answer natural language questions without relying on pre-annotated (gold) entities. The system integrates **Named Entity Disambiguation (NED)**, **knowledge graph retrieval**, and **transformer-based language models** to generate accurate answers.

The implementation explores multiple configurations using **T5** and **Longformer**, along with a **reranking mechanism** to improve answer selection.

---

## 🧠 Pipeline

The system follows a structured pipeline:

```
Question
   ↓
Named Entity Disambiguation (NED)
   ↓
Knowledge Graph Retrieval (Wikidata triples)
   ↓
Context Construction
   ↓
Transformer Model (T5 / Longformer)
   ↓
Top-k Answer Generation (Beam Search)
   ↓
Reranking (Model Confidence)
   ↓
Final Answer
```

---

## 🔬 Methodology

### 🔹 1. No Gold Entities
- The system does **not use ground-truth entities**
- Entities are automatically extracted from the question using heuristics and Wikidata search

---

### 🔹 2. Knowledge Graph Retrieval
- Uses **Wikidata API + SPARQL**
- Retrieves relevant triples based on detected entities
- Constructs context in the format:
  ```
  question: <question> context: <triples>
  ```

---

### 🔹 3. Models

#### ✅ T5 (Text-to-Text Transformer)
- Treats QA as a text generation problem
- Strong baseline even without KG

#### ✅ Longformer (LED)
- Handles long input contexts (important for multiple triples)
- Better suited for KG-augmented QA

---

### 🔹 4. Reranking (Extension)
- Generates **top-k answers** using beam search
- Ranks candidates using **model confidence scores**
- Improves:
  - Hit@5
  - MRR

---

## 🧪 Experiments

We evaluate different configurations across models with and without NED, along with reranking:

| Model        | With NED | Without NED | Reranking |
|-------------|----------|-------------|----------|
| T5          | ✅       | ✅          | ✅       |
| Longformer  | ✅       | ✅          | ✅       |


Rigel model is included as a baseline model for comparison. Its reported results are taken from prior work and not reimplemented due to its architectural complexity.

---


## 📊 Evaluation Metrics

- **Hit@1** – Exact match accuracy  
- **Hit@5** – Correct answer in top 5  
- **MRR** – Mean Reciprocal Rank  
- **F1 Score** – Token-level overlap  
- **Accuracy**

---

## 📈 Key Findings

- T5 performs competitively even without KG due to strong pretrained knowledge.
- KG integration improves performance only when relevant and high-quality facts are retrieved.
- Longformer benefits significantly from KG context due to its ability to process long inputs.
- NED improves answer grounding by focusing retrieval on relevant entities.
- Reranking:
  - significantly improves Hit@5 (candidate coverage)
  - may slightly reduce Hit@1 due to reordering effects
- KG context increases candidate diversity but may introduce noise if retrieval is not precise.

---

## 📂 Dataset

- **Mintaka Dataset (KGQA benchmark)**

⚠️ Due to size limitations, the dataset is not included.

Download from:
👉 https://github.com/amazon-science/mintaka

Place files in project directory:
```
mintaka_train.json
mintaka_dev.json
mintaka_test.json
```

---

## ⚙️ Setup

```bash
pip install transformers datasets sentencepiece evaluate tqdm
```

---

## ▶️ Usage

Train model:
```python
trainer.train()
```

Generate predictions:
```python
predict_answer(question, context)
```

Run evaluation:
```python
# computes Hit@1, Hit@5, F1, MRR
```

---

## 🔮 Future Work

- 🔹 Retrieval-Augmented Generation (RAG)
- 🔹 Integration with Large Language Models (LLMs)
- 🔹 Improved entity linking (advanced NED)
- 🔹 Better knowledge retrieval (beyond simple Wikidata queries)
- 🔹 Semantic reranking methods

---

## 🏁 Conclusion

This project demonstrates that **transformer-based models can effectively perform KGQA without gold entities**. While knowledge graphs enhance reasoning, pretrained models already encode substantial factual knowledge. Reranking further improves answer selection by leveraging multiple candidate outputs.

---

## 🛠 Tech Stack

- Python  
- PyTorch  
- HuggingFace Transformers  
- Wikidata API + SPARQL  
- Google Colab  

---

## 📎 Notes

- Uses simplified KG retrieval instead of full CLOCQ system  
- Focus is on **end-to-end QA without manual annotations**

---

## 🔬 Additional Approach (Semantic Parsing)

In addition to the language model-based approach, the project also explores a semantic parsing-based KGQA framework. This approach focuses on generating structured SPARQL queries from natural language questions and executing them over a knowledge graph.

Transfer learning techniques are applied using datasets such as LC-QuAD2 and QALD to improve generalization. This method enables structured reasoning over knowledge graphs and complements the language model-based approach.
