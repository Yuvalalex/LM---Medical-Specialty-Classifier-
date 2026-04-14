## Medical Specialty Classification System
**Project Documentation & Technical Report**
**Developer:** Yuval Alexandrony

---

**Project Link:**
[Google Colab - Medical Specialty Classification](https://colab.research.google.com/drive/1VzxOWtEsQzNJzob1K__tQze8TwKusQWe?usp=sharing)

Use dataset: https://www.kaggle.com/datasets/tboyle10/medicaltranscriptions

---

### 1. Abstract
This project presents an advanced Artificial Intelligence (AI) system designed to classify medical transcriptions into 29 distinct clinical specialties. The system was engineered to address core challenges in the Health-Tech domain: significant **Class Imbalance**, complex professional terminology, and the critical requirement for **High Recall** in rare or high-value clinical cases. 

The architecture is built upon Microsoft’s **PubMedBERT** and utilizes an innovative pipeline featuring **Named Entity Recognition (NER)** enrichment and **Contextual Synthetic Data Augmentation**.

---

### 2. Challenges & Solutions

| Business/Technical Challenge | Technical Solution implemented |
| :--- | :--- |
| **Data Scarcity in Rare Diseases** (e.g., only 6 samples for Dermatology) | **Smart Data Augmentation:** Utilizing BERT-based models to generate synthetic examples that maintain "Contextual Embeddings," ensuring a minimum of 50 samples per class. |
| **"Dirty" & Unstructured Medical Text** | **NER Enrichment:** Implementing a Named Entity Recognition model to automatically extract medication names and procedures, "injecting" them into the document headers as high-signal features. |
| **Bias Toward Common Classes** (Model defaults to "Surgery" due to majority frequency) | **Custom Weighted Loss:** Development of a specialized loss function that heavily penalizes the model for misclassifying minority classes. |
| **Need for Clinical Domain Expertise** | **SOTA Model Selection:** Transitioning from standard BERT to **PubMedBERT**, pre-trained from scratch on biomedical literature to understand nuanced clinical contexts. |

---

### 3. System Pipeline
The data processing workflow is designed to be linear and modular:

#### **Phase 1: Data Hygiene & Filtering**
* Loading the [mtsamples.csv](https://www.kaggle.com/datasets/tboyle10/medicaltranscriptions) dataset.
* Filtering out generic categories with low insurance/clinical value (e.g., "General Surgery") to sharpen the model’s focus on specific diagnoses (e.g., "Neurosurgery").

#### **Phase 2: Contextual Augmentation (Synthetic Engine)**
* Utilizing the `nlpaug` library with the `bert-base-uncased` model.
* The algorithm identifies underrepresented classes (<50 samples) and generates semantic variations of existing texts to "bulk up" the data and balance the distribution.

#### **Phase 3: Fine-Tuning Strategy**
* **Model:** `microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract-fulltext`.
* **Loss:** Weighted Cross-Entropy.
* **Optimization:** Employing **Mixed Precision (FP16)** for hardware acceleration and **Early Stopping** to prevent overfitting.
* **Transfer Learning:** Preserving pre-trained weights to leverage the model’s foundational medical knowledge.

---

### 4. Key Metrics & Performance
The system was evaluated on a separate test set and demonstrated robust capabilities:

* **Macro F1 Score:** A high score reflecting a balance across all classes, not just the majority ones.
* **High Recall on Critical Classes:** * **Neurosurgery:** Reliable identification of brain-related procedures.
    * **Endocrinology / Diet:** Precise detection of metabolic diseases (Diabetes/Obesity) that were nearly invisible in the baseline model.
    * **Dentistry / Sleep Medicine:** Achieved up to 100% detection in specific test scenarios.

---

### 5. Setup & Installation Guide

#### **System Requirements**
* **Environment:** Google Colab (Recommended) or a local server with GPU support.
* **Hardware:** NVIDIA GPU (T4 or higher) is mandatory due to BERT and NER processing requirements.

#### **Execution**
1.  Open the provided notebook (`.ipynb`).
2.  Ensure the runtime is set to **GPU**.
3.  Upload the `mtsamples.csv` file.
4.  Run all cells. The system will automatically handle installations, preprocessing, training, and final report generation.
