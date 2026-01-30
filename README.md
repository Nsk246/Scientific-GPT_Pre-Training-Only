# 🧬 Science-GPT: Domain-Specific LLM Pre-training

[![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)](https://pytorch.org/)
[![RunPod](https://img.shields.io/badge/RunPod-%23673AB7.svg?style=for-the-badge&logo=runpod&logoColor=white)](https://www.runpod.io/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)
[![Parameters](https://img.shields.io/badge/Parameters-89M-success?style=for-the-badge)]()

**Science-GPT** is a decoder-only Transformer language model trained from scratch on a large corpus of scientific literature.  
The project demonstrates an end-to-end large language model pipeline, from web scraping and dataset construction to distributed multi-GPU pre-training and inference.

---

## 📂 Project Structure

    ├── NSK-ML.ipynb        # Data scraping, cleaning, and dataset construction
    ├── Exp.ipynb           # Model pre-training, inference, and analysis
    ├── checkpoints/        # Saved model checkpoints , will be created after executing NSK-ML.ipynb
    ├── vocab.json          # Character-level vocabulary mapping, will be created while executing Exp.ipynb
    └── README.md

---

## 🕷️ Data Pipeline (Web Scraping)

All data collection and preprocessing are implemented in **NSK-ML.ipynb**.

The dataset is constructed by scraping the **Project Gutenberg** library, focusing on scientific and technical literature.

**Source:** Project Gutenberg Bookshelves  

**Domains Covered:**
- Physics
- Mathematics
- Chemistry
- Biology
- Geology
- Technology
- Environment

### Pipeline Overview

- **Crawling:** Iterates through domain-specific bookshelves to collect book URLs  
- **Cleaning:** Regex-based removal of headers, footers, metadata, and formatting artifacts  
- **Deduplication:** MD5 hashing to remove duplicate volumes  
- **Batching:** Aggregates cleaned text into fixed-size chunks for efficient streaming during training  

### Dataset Statistics

- **Total Books:** ~3,971 unique volumes  
- **Raw Size:** ~1.4 GB  
- **Total Characters:** ~1.42 billion  
- **Tokenization:** Character-level (~2,473 unique characters)  

---

## 🧠 Model Architecture

The model follows a standard GPT-style decoder-only Transformer architecture trained entirely from scratch.

- **Total Parameters:** 89.2 million  
- **Transformer Layers:** 12  
- **Embedding Dimension:** 768  
- **Attention Heads:** 12  
- **Context Length:** 512 tokens  
- **Dropout:** 0.1  

---

## ⚡ Training and Inference

All model training, evaluation, and inference logic is implemented in **Exp.ipynb**.

### Training Setup

- **Framework:** PyTorch  
- **Parallelism:** DistributedDataParallel (DDP)  
- **Hardware:** 2 × NVIDIA H200 GPUs  
- **Optimizer:** AdamW with cosine learning rate decay  
- **Gradient Accumulation:** Enabled to simulate larger effective batch sizes  

### Training Performance

- **Throughput:** ~196k characters per iteration  
- **Total Processed:** ~2.8 billion characters (approximately 2 epochs)  
- **Best Validation Loss:** 0.92  

---

## 🚀 Usage

### 1. Data Collection

Open and execute the scraping and preprocessing notebook:

    NSK-ML.ipynb

This notebook builds the cleaned training dataset from Project Gutenberg sources.

---

### 2. Pre-training and Inference

Open and execute:

    Exp.ipynb

This notebook handles:
- Model initialization  
- Distributed pre-training  
- Checkpointing  
- Text generation  
- Loss and perplexity analysis  

Example inference:

    prompt = "What is the chemical composition of lead sulphate?"
    response = model.generate(prompt, max_new_tokens=300)
    print(response)

Example output:

    Question: What is an electrolyte?
    Answer: Such a system as electrolysis is constant and subject to an increased energy expenditure...

---

## 🤝 Contributing

Contributions are welcome, especially for:
- Migrating from character-level tokenization to BPE or unigram language models  
- Scaling model size, depth, or context length  
- Improving training efficiency and memory optimization  

Contribution steps:
1. Fork the repository  
2. Create a feature branch  
3. Commit your changes  
4. Open a pull request  

---

## 📄 License

Distributed under the **MIT License**. See the LICENSE file for more information.
