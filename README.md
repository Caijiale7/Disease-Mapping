# Disease Mapping Project

## Overview

This repository contains the workflow, instructions, and rules for mapping pharmaceutical indication data into standardized disease classifications.

The primary objectives are:

1. Mapping pharmaceutical indication descriptions to ICD-10 disease classifications;
2. Mapping ICD-10-standardized diseases to internal disease taxonomy;
3. Identifying ambiguous and unmapped records for further review.

The overall workflow is:

```
Raw pharmaceutical indication data
        ↓
Disease entity extraction
        ↓
ICD-10 mapping
        ↓
Internal disease classification mapping
        ↓
Unmapped record review
```

---

## Project Purpose

Pharmaceutical databases often contain indication descriptions with inconsistent terminology, different disease naming conventions, and varying levels of clinical detail.

This project aims to standardize pharmaceutical indication information by:

- Identifying the core disease entity from indication descriptions;
- Separating diseases from treatment-related qualifiers (e.g., disease stage, biomarker status, treatment line);
- Mapping diseases to ICD-10 classifications;
- Mapping standardized diseases to internal disease categories;
- Preserving ambiguous and unmapped records for manual review.

---

## Repository Structure

```text
Disease-Mapping/

├── AGENTS.md
│   └── Repository-level instructions for Codex execution
│
├── DISEASE_MAPPING_RULES.md
│   └── Detailed disease mapping rules and methodology
│
├── README.md
│   └── Project overview and usage guide
│
├── data/
│   └── Input pharmaceutical and internal disease classification files
│
├── references/
│   └── External references (e.g., ICD-10 source information)
│
└── outputs/
    └── Generated mapping results and review files
```

---

## Main Files

### AGENTS.md

Defines repository-level instructions for Codex execution.

This file specifies:

- General execution principles;
- Required files to read before performing tasks;
- Data handling rules;
- Output requirements.

---

### DISEASE_MAPPING_RULES.md

Contains detailed disease mapping methodology and rules.

The document defines:

- Input data interpretation;
- Disease entity extraction rules;
- ICD-10 mapping logic;
- Internal disease classification mapping;
- Confidence scoring;
- Unmapped record handling;
- Quality control requirements.

Before performing any disease mapping task, Codex should read this file.

---

## Input Data

The mapping workflow uses the following input sources.

### 1. Pharmaceutical Database Data

Source:

- Pharmaceutical database exports.

Typical fields include:

- Drug name;
- Indication;
- Indication description;
- Disease area;
- Trial information.

The detailed field interpretation and priority rules are described in:

`DISEASE_MAPPING_RULES.md`

---

### 2. Internal Disease Classification

The internal disease taxonomy is used as the final business-oriented classification framework.

Typical fields include:

- Therapeutic Area (TA);
- Disease Name (English);
- Disease Name (Chinese).

The internal disease classification should be used together with ICD-10 mapping results and clinical disease interpretation.

---

### 3. ICD-10 Reference

The project uses China ICD-10 Medical Insurance Version as the standard ICD-10 reference.

Detailed ICD-10 rules are documented in:

`references/ICD10_REFERENCE.md`

ICD-10 is used as a standardized disease ontology.

The mapping process uses ICD-10 as an intermediate reference layer between:

```
Raw indication description
        ↓
Standardized disease entity
        ↓
ICD-10 classification
        ↓
Internal disease category
```

The ICD-10 version and source should be recorded for each mapping project.

---

## Workflow

Before executing any disease mapping task:

1. Read `AGENTS.md`;
2. Read `DISEASE_MAPPING_RULES.md`;
3. Load pharmaceutical database files;
4. Load internal disease classification files;
5. Identify indication-related fields;
6. Extract disease entities;
7. Map diseases to ICD-10 classifications;
8. Map standardized diseases to internal disease categories;
9. Generate mapped results and unmapped review files.

---

## Output

The final output should contain the following components.

### 1. Mapping Results

The output should include:

- Original indication description;
- Standardized disease entity;
- ICD-10 code and disease name;
- Internal disease classification;
- Mapping confidence;
- Mapping rationale.

---

### 2. Unmapped Records

All records without reliable mapping should be preserved.

Examples include:

- Missing disease information;
- Ambiguous disease descriptions;
- Multiple possible ICD-10 classifications;
- No corresponding internal disease category;
- Cases requiring manual review.

Unmapped records should never be deleted.

---

## Mapping Principles

The following principles must always be followed:

- Do not modify original input files;
- Preserve original indication descriptions;
- Do not force a disease into an unrelated internal category;
- Do not rely only on keyword matching;
- Separate core diseases from disease qualifiers;
- Maintain mapping rationale for each result;
- Record uncertainty and manual review requirements.

---

## Codex Execution Instructions

For any disease mapping task in this repository:

1. First read `AGENTS.md`;
2. Follow the rules defined in `DISEASE_MAPPING_RULES.md`;
3. Use the provided pharmaceutical database and internal disease classification files;
4. Return both mapped and unmapped results;
5. Clearly identify records requiring manual review;
6. Save generated outputs separately from original data files.

---

## Project Status

Current focus:

- Cardiovascular disease indication mapping;
- ICD-10 standardization;
- Internal disease taxonomy alignment.

Future extensions may include:

- Additional therapeutic areas;
- Automated disease synonym dictionaries;
- Expanded disease ontology mapping;
- Validation workflows.
