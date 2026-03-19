# ZeroTrain.ai Architecture & Inference Engine

**Knowledge-Based Inference for Real-Time Decisioning**


## 1. Introduction

ZeroTrain.ai is a **deterministic, knowledge-based inference engine** designed for:

- High-speed execution
- Full explainability
- Rule-driven decisioning

Unlike traditional machine learning systems, ZeroTrain performs **symbolic reasoning** using explicit rules authored by:

- Subject-matter experts
- Developers
- Business operators

This document provides a technical overview of:

- Inference architecture
- Execution pipeline
- Semantic and numeric evaluation
- Deterministic guarantees
- Deployment models


## 2. Architectural Overview

ZeroTrain is composed of **five core layers**:

1. **Language Parsing Layer**
2. **Model Construction Layer**
3. **Inference Engine (Core Execution Pipeline)**
4. **Trace & Explainability Layer**
5. **Deployment Layer (API, Container, ONNX)**

Each layer is modular, enabling deployment from lightweight APIs to enterprise-grade systems.


## 3. Language Parsing Layer

### 3.1 ZeroTrain Rule Language (ZRL)

ZRL is a declarative rule language designed for clarity and precision.

A rule consists of:

- Inputs
- Conditions
- Actions
- Semantic relationships
- Variables and dynamic values
- Concepts and weights


### 3.2 Parsing Pipeline

The parsing process includes:

- Lexical analysis
- Operator classification
- Semantic path resolution
- Control-flow normalization
- Tree construction

Output:

> **Model Representation Tree (MRT)** — a deterministic structure describing rule logic.


## 4. Model Construction Layer

### 4.1 Nodes

Nodes represent:

- Conditions
- Branch entry/exit points
- MUST constraints
- Evaluation regions (numeric or semantic)
- Actions

Nodes form a **directed inference graph**.

---

### 4.2 Features & States

- **Features** = input definitions  
- **State** = runtime values

Supports:

- n-to-1 feature reduction
- Weighted features
- Semantic features (e.g., `PART_OF`, `SAME_AS`)
- Numeric and symbolic inputs


### 4.3 Ranges & Normalization

ZeroTrain tracks:

- Dynamic min/max ranges
- Value stabilization
- Concept weight normalization

This enables **proximity-based evaluation where applicable**.


## 5. Inference Engine (Core Execution Pipeline)

The engine is built on three principles:

- **Determinism**
- **Speed**
- **Traceability**

### Performance

- ~1.4M nodes processed in ~74 ms (cold)
- ~2.3 ms (hot execution)


### 5.1 Execution Plans

Each rule generates one or more **Execution Plans**.

A plan contains:

- Condition sequence
- Branch structure
- Semantic checks
- Final actions

Plans allow **O(n) traversal regardless of model complexity**.


### 5.2 Parallel Plan Execution

Plans can execute independently:

1. Receive feature set
2. Execute logic
3. Return result
4. Participate in arbitration

#### Arbitration Modes

- First to Finish
- Last to Finish
- Priority Mode

Determinism is preserved through **explicit arbitration rules**.


### 5.3 Condition Evaluation

Each condition supports:

- Literal comparisons
- Numeric comparisons
- Range checks
- Semantic relationships
- Variable substitution

Returns:

- Pass
- Fail
- Partial (weighted)
- Semantic strength


### 5.4 Branch Resolution

ZeroTrain enforces **single-path resolution**:

```text
WHEN
  THEN
ELSE
  WHEN
    THEN
  ELSE
END
