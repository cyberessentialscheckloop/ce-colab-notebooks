# Cyber Essentials — CheckLoop (Colab Notebooks)

**Repository:** [cyberessentialscheckloop/ce-colab-notebooks](https://github.com/cyberessentialscheckloop/ce-colab-notebooks)  
**Author:** S. Makombe  

This repository contains Google Colab notebooks for **CheckLoop**, an assessor-focused automation pipeline that integrates rule-based logic, semantic information extraction, and rubric-guided large language models (LLMs) to support consistent and explainable Cyber Essentials (CE) assessments.

---

## 📘 Notebooks

| Stage | Notebook file | Open in Colab | Purpose |
|:--:|:--|:--|:--|
| 1 | `Data_Preparation_and_Exploration_for_development_corpus_CLEAN_WITH_OUTPUTS.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/Data_Preparation_and_Exploration_for_development_corpus_CLEAN_WITH_OUTPUTS.ipynb) | Loads and explores the development corpus, structuring questions, answers, and notes for downstream modules. |
| 2 | `data_augmentation_&_balance_for_descriptive_data_CLEAN_WITH_OUTPUTS.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/data_augmentation_&_balance_for_descriptive_data_CLEAN_WITH_OUTPUTS.ipynb) | Performs class balancing and data augmentation for descriptive answers while preserving label integrity. |
| 3 | `Rules_SemanticFrames_Extraction_1_CLEAN_WITH_OUTPUTS.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/Rules_SemanticFrames_Extraction_1_CLEAN_WITH_OUTPUTS.ipynb) | Encodes CE marking rules (CFG) and extracts semantic frames and entities (spaCy + SRL) aligned to the marking guide. |
| 4 | `zero_shot_VS_finetune_Mistral_7B_with_rubrics_FENCES_STRICT_SAFE_WITH_OUTPUTS.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/zero_shot_VS_finetune_Mistral_7B_with_rubrics_FENCES_STRICT_SAFE_WITH_OUTPUTS.ipynb) | Compares zero-shot and fine-tuned **Mistral-7B** models under CE-specific rubrics (safe rendering for GitHub). |
| 5 | `zeroshot_fine_tuned_LLaMA_8B_with_rubrics_4_CLEAN_WITH_OUTPUTS.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/zeroshot_fine_tuned_LLaMA_8B_with_rubrics_CLEAN.ipynb) | Evaluates **LLaMA-3.1-8B** using CE-specific rubric prompts and reports comparative performance metrics. |
| 6 | `INFERENCE_ON_UNSEEN_DATA_CLEAN_WITH_OUTPUTS.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/INFERENCE_ON_UNSEEN_DATA_CLEAN_WITH_OUTPUTS.ipynb) | Runs the full inference pipeline on held-out forms and saves predictions with explanatory outputs. |
| 7 | `streamlitdeployment_clean (1).ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cyberessentialscheckloop/ce-colab-notebooks/blob/main/streamlitdeployment_clean%20(1).ipynb) | Launches the Streamlit assessor interface with override logging and exportable audit artefacts. |


---

##  Project Objectives

- **Consistency with defensible rationales** — Reduce assessor-to-assessor variability by combining deterministic rule checks, structured list validation, and rubric-guided LLM scoring. Each judgement includes an explicit rationale (rule ID, recovered frame, or rubric-aligned explanation).  
- **Human-in-the-loop control** — Provide transparent reasoning for every automated judgement and allow assessors to override outputs with audit logging, ensuring automation assists rather than replaces expert review.  
- **Dependable whole-form outcomes** — Apply conservative fusion and cross-question logic (e.g., missing A6.2.x versions → MIR; firewall “No” → downgrades dependent items) to prevent contradictory results.  
- **Pre-assessment and transparency** — Support applicant self-checks and enable scheme-level monitoring of assessor consistency.

---

##  Data & Privacy

This repository **does not** include any real Cyber Essentials applications or confidential material.  
It contains **only source code and demonstration notebooks**.

---

##  Methodology (Summary)

- **Research design.**  
  **Design Science Research (DSR)**: CE marking guidance is encoded **deductively**; labelled forms are analysed **inductively** via exploratory data analysis; both are reconciled through **abductive** reasoning.  
  Dataset: 70 applications (65 development, 5 hold-out).

- **Module 1 — Rule engine (closed-form).**  
  Machine-checkable items (IDs, Yes/No, MCQs) are mapped to explicit rules with fixed precedence (**Fail > MIR > Non-Compliant > Compliant**). Outputs include the schema label plus a concise **rule-ID** justification.

- **Module 2 — Semantic frames (lists).**  
  List answers are normalised into **slot–value** frames (provider, product, version, quantity, role/scope). The module validates completeness (e.g., A6.2.x requires version info) and checks software against an **End-of-Life (EOL)** catalogue.

- **Module 3 — Rubric-guided LLMs (descriptive).**  
  **Mistral-7B** and **LLaMA-3.1-8B** are prompted with CE-specific rubrics to predict **Compliant / Non-Compliant / MIR** (MIR only where permitted), with short rationales. Lightweight **LoRA** adapters enhance domain alignment.

- **Explainability.**  
  - *Module 1:* Rule trace (rule-ID, trigger pattern, matched span)  
  - *Module 2:* Frame trace (extracted slots, missing/invalid fields, EOL findings)  
  - *Module 3:* Rubric rationale + token-level attributions (Integrated Gradients)  
  - *Fusion:* Precedence chain and contributing module outputs  
  All artefacts are visible in the UI and exportable as JSON/CSV.

- **Fusion & dependencies.**  
  Module outputs are integrated with conservative tie-breaking and **monotonic cross-question constraints**, producing consistent whole-form results.

- **Assessor UI (Streamlit).**  
  Upload forms, review per-item rationales with token highlights, apply overrides, and export audit artefacts (CSV/JSON/PDF).

*Pilot evaluation on a hold-out set achieved ≈ 0.90 overall accuracy; the **Compliant** class reached precision 0.965 / recall 0.932 (F1 = 0.948). Ongoing work targets minority classes (NC/MIR) through data expansion.*

---

##  Authors & Contributors

- **S. Makombe** — Lead developer, primary author, and maintainer of all code and notebooks  
- **H. Ahriz** — Academic supervisor, methodology and conceptual guidance  
- **V. Obahor** — Industry collaborator, domain validation and data-access support

---

##  Suggested Citation

Makombe, S., Ahriz, H., & Obahor, V. (2025). *Cyber Essentials — CheckLoop (Colab Notebooks).* GitHub repository. [https://github.com/cyberessentialscheckloop/ce-colab-notebooks](https://github.com/cyberessentialscheckloop/ce-colab-notebooks)
