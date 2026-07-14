# ICD-10 Reference

## Purpose

This document defines the ICD-10 reference standard used in the Disease Mapping Project.

The purpose of ICD-10 mapping is to standardize pharmaceutical indication descriptions before mapping them into the internal disease taxonomy.

The mapping workflow follows:

Raw pharmaceutical indication description

↓

Standardized disease entity

↓

China ICD-10 Medical Insurance Disease Classification

↓

Internal disease classification


---

## ICD-10 Version and Reference Standard

This project primarily uses:

**China ICD-10 Medical Insurance Version (医保版 ICD-10)**

as the standard disease classification reference.

All disease mappings should follow the terminology and coding structure of the China medical insurance ICD-10 classification.

WHO ICD-10, ICD-10-CM, or other international ICD-10 adaptations should not be mixed into the same mapping project unless explicitly required.


---

## ICD-10 Usage Principles

ICD-10 is used as an intermediate standardized disease layer.

The final internal disease classification should be determined based on:

1. Original pharmaceutical indication description;
2. Standardized disease entity;
3. China ICD-10 Medical Insurance classification;
4. Internal disease taxonomy definition.


ICD-10 codes should support disease standardization but should not automatically determine the final internal disease category.


---

## Mapping Rules

### 1. Use the most clinically appropriate ICD-10 category

The selected ICD-10 code should:

- Match the disease entity described in the indication;
- Follow China ICD-10 Medical Insurance terminology;
- Use the most specific available classification supported by the indication.


Do not assign a more specific ICD-10 code if the indication description does not provide sufficient evidence.


---

### 2. Avoid over-inference

Do not infer additional disease information that is not explicitly stated.

Do not automatically infer:

- Disease subtype;
- Disease severity;
- Disease stage;
- Disease etiology;
- Disease complications.


Examples:

Correct:

"急性心肌梗死"

→ Map to acute myocardial infarction related ICD-10 category


Incorrect:

"冠心病"

→ Automatically mapped to acute myocardial infarction


---

### 3. Disease groups and clinical syndromes

Disease groups, risk categories, and clinical syndromes should not automatically be expanded into individual diseases.

Examples:

"动脉粥样硬化性心血管疾病（ASCVD）"

should not automatically become:

- 冠心病;
- 缺血性卒中;
- 外周动脉疾病;

unless the indication explicitly specifies the disease subtype.


---

### 4. Multiple possible ICD-10 mappings

If one indication can correspond to multiple ICD-10 categories:

Return:

- Primary ICD-10 mapping;
- Alternative possible mappings;
- Mapping confidence;
- Manual review requirement.


---

## ICD-10 Output Requirements

Each mapped indication should include:

| Field | Description |
|---|---|
| ICD-10 Version | China ICD-10 Medical Insurance Version |
| ICD-10 Code | ICD-10 code |
| ICD-10 Disease Name | Standard disease name |
| ICD-10 Category | Disease category |
| Mapping Confidence | High / Medium / Low |
| Mapping Rationale | Explanation of mapping |
| Review Required | Yes / No |


---

## Version Control

Before each mapping task, record:

- ICD-10 version;
- Source/reference used;
- Mapping date.


Different ICD-10 versions should not be mixed within the same project.

If a disease cannot be reliably mapped using China ICD-10 Medical Insurance classification:

- Keep the record;
- Mark as "Unmapped";
- Provide the reason;
- Do not substitute another ICD-10 version.
