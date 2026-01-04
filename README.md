# Contract Clause Risk Tagger

## Overview
This project implements a **clause-level contract risk assessment system** by fine-tuning a lightweight language model using instruction tuning.  
Given a **single contract clause**, the model predicts the **risk level** (Low / Medium / High) and produces a **natural language assessment**.

The project is intentionally designed as a **small-scale but industry-relevant baseline** that can be trained on limited hardware and later extended to larger models and richer supervision.

---

## Problem Statement
Legal contracts contain clauses that introduce varying levels of legal and financial risk (e.g., liability disclaimers, unilateral termination rights).  
Manual identification of risky clauses is time-consuming and repetitive.

This project addresses the task:

> **Given a single contract clause, assess its legal risk level and explain the assessment in natural language.**

---

## Dataset Access

This project uses the **Contract Understanding Atticus Dataset (CUAD)**.

Due to licensing and size considerations, the dataset is not included in this repository.

You can obtain the dataset from the official source:
https://www.atticusprojectai.org/cuad

After downloading CUAD, update the dataset path in the notebook as instructed.

### Data Processing
- Only **single clauses** are used (not full contracts)
- Clause text is extracted from CUAD question–answer pairs
- Risk levels are inferred heuristically from clause semantics and question intent

### Risk Categories
- **High Risk**  
  Examples: liability disclaimers, termination without cause  
- **Medium Risk**  
  Examples: indemnification, payment-related clauses  
- **Low Risk**  
  Examples: governing law, jurisdiction clauses  

### Final Dataset Size
- ~5,000 clause-level samples
- Train / validation split: **90% / 10%**

---

## Model

### Architecture
- **SmolLM-135M-Instruct**
- Lightweight causal language model
- Context window: **256 tokens**

### Training Strategy
- Full fine-tuning (no LoRA, no quantization)
- Instruction-style supervision
- FP32 training for stability on limited GPU memory

### Instruction Format
- Instruction:
    Assess the risk level of the contract clause.

- Clause:
    contract clause text

- Assessment:
    model-generated assessment

## Reproducibility

The repository intentionally provides code rather than trained artifacts.

To reproduce the results:
1. Download the CUAD dataset from the official source
2. Open the provided Jupyter notebook
3. Run the cells sequentially to:
   - Build the clause-level dataset
   - Train the model
   - Perform inference

The full training pipeline can be executed on a single GPU (e.g., NVIDIA T4).
