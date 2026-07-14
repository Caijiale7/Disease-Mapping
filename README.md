# Disease Mapping Project

## Overview

This repository contains the workflow, instructions, and rules for mapping pharmaceutical indication data into standardized disease classifications.

The primary objectives are:

1. Mapping pharmaceutical indication descriptions to ICD-10 disease classifications;
2. Mapping ICD-10-standardized disease entities to internal disease taxonomy;
3. Identifying ambiguous and unmapped records for further review.

The overall workflow is:

```text
Raw pharmaceutical indication data

        ↓

Indication structure identification

        ↓

Disease entity normalization

        ↓

Disease modifier identification

        ↓

ICD-10 mapping

        ↓

Internal disease classification mapping

        ↓

Mapped and unmapped record review
```
---

## Project Purpose

Pharmaceutical databases often contain indication descriptions with inconsistent terminology, different disease naming conventions, and varying levels of clinical detail.

This project aims to standardize pharmaceutical indication information by:

- Identifying the core disease entity from indication descriptions;
- Separating core diseases from disease modifiers (e.g., disease stage, severity, biomarker status, genetic subtype, and clinical phenotype);
- Mapping standardized disease entities to ICD-10 classifications;
- Mapping ICD-10-standardized diseases to internal disease categories;
- Preserving ambiguous and unmapped records for manual review.

The project does not rely only on keyword matching. Clinical interpretation and disease ontology alignment are required during mapping.

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
├── references/
│   └── External reference standards
│       └── ICD10_REFERENCE.md
│
├── data/
│   └── Input pharmaceutical and internal disease classification files
│
└── outputs/
    └── Generated mapping results and review files
```

---

## Main Files

### AGENTS.md

Defines repository-level instructions for Codex execution.

This file specifies:

- Required files to read before performing tasks;
- Execution order;
- Data handling rules;
- Output requirements;
- File management rules.

---

### DISEASE_MAPPING_RULES.md

Contains detailed disease mapping methodology and rules.

The document defines:
- Input data interpretation;
- Indication field priority;
- Disease entity normalization rules;
- Disease modifier identification rules;
- ICD-10 mapping logic;
- Internal disease classification mapping;
- Confidence assessment;
- Unmapped record handling;
- Quality control requirements.

Before performing any disease mapping task, Codex should read this file.

---

### references/ICD10_REFERENCE.md

This file defines the ICD-10 reference standard used in this project.

It specifies:

- ICD-10 version;
- Official reference source;
- Version control rules;
- ICD-10 mapping principles.

The project uses:
- China ICD-10 Medical Insurance Version
- 中国医疗保障疾病分类与代码（医保版 ICD-10）

as the official ICD-10 reference standard.

The detailed ICD-10 source definition and version control are maintained in:
`references/ICD10_REFERENCE.md`

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

Internal disease mapping should consider:

- Normalized disease entity;
- ICD-10 classification;
- Internal disease taxonomy definition.

---

### 3. ICD-10 Reference

ICD-10 is used as an intermediate standardized disease layer between pharmaceutical indications and internal disease taxonomy.

The mapping framework is:

Raw indication description

↓

Normalized Disease Entity

↓

China ICD-10 Medical Insurance Classification

↓

Internal Disease Category

The ICD-10 version and reference source should be recorded for each mapping project.

---

## Workflow

Before executing any disease mapping task:

1. Read `AGENTS.md`;
2. Read `references/ICD10_REFERENCE.md`;
3. Read `DISEASE_MAPPING_RULES.md`;
4. Load pharmaceutical database files;
5. Load internal disease classification files;
6. Identify indication-related fields;
7. Identify indication structure;
8. Normalize disease entity;
9. Identify disease modifiers;
10. Map disease entity to ICD-10 classification;
11. Map ICD-10 standardized disease to internal disease taxonomy;
12. Generate mapped results and unmapped review files.

---

## Output

The final output should contain the following components.

### 1. Mapping Results

Each mapped record should include:

- Original indication description;
- Normalized disease entity;
- Disease modifier;
- ICD-10 version;
- ICD-10 code;
- ICD-10 Disease name;
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
- Identify core disease entities before mapping;
- Separate diseases from disease modifiers;
- Do not split indication descriptions unless multiple independent diseases are explicitly present;
- Do not force diseases into unrelated internal categories;
- Do not rely only on keyword similarity;
- Maintain mapping rationale for every result;
- Record uncertainty and manual review requirements.
  
---

## Codex Execution Instructions

For any disease mapping task in this repository:

1. First read `AGENTS.md`;
2. Read ICD-10 reference information from `references/ICD10_REFERENCE.md`;
3. Follow the rules defined in `DISEASE_MAPPING_RULES.md`;
4. Load the provided pharmaceutical database and internal disease classification files;
5. Perform disease entity normalization;
6. Perform ICD-10 mapping using the specified ICD-10 standard;
7. Perform internal disease classification mapping;
8. Return both mapped and unmapped results;
9. Clearly identify records requiring manual review;
10. Save generated outputs separately from original data files.

---

## Project Status

Current focus:

- Cardiovascular disease indication mapping;
- ICD-10 standardization;
- Internal disease taxonomy alignment.

Future extensions may include:

- Additional therapeutic areas;
- Disease synonym dictionaries;
- Expanded disease ontology mapping;
- Automated disease synonym dictionaries;
- Additional clinical classification standards.
- Validation workflows.
