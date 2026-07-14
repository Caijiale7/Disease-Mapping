# Disease Mapping Rules

## Project Objective

The purpose of this project is to map pharmaceutical indications exported from PharmaCube to:

1. ICD-10 disease classifications.
2. Internal disease taxonomy.

For every record in the PharmaCube file, Codex must:

- Read the indication description.
- Identify the disease entity.
- Map the disease to ICD-10.
- Match the ICD-10 result to the internal disease classification.
- Return unmapped diseases.
- Explain the mapping rationale.

---

# Input Files

## File 1: PharmaCube Export

Main sheet:

```text
创新药
```

Important columns:

| Column | Meaning |
|----------|----------|
| 登记号 | Unique record ID |
| 主试验药 | Drug name |
| 适应症 | PharmaCube summarized indication |
| 适应症描述 | Detailed indication description |
| 试验题目 | Trial title |

Important:

The primary field for disease identification is:

```text
适应症描述
```

The following columns are supplementary only:

```text
适应症
试验题目
```

Codex must NOT use:

```text
疾病领域
```

as the primary source for disease mapping.

---

## File 2: Internal Disease Classification

Main sheet:

```text
Disease Mapping
```

Important columns:

| Column | Meaning |
|----------|----------|
| B | TA |
| C | Disease (EN) |
| D | Disease (CN) |

Examples:

- 心力衰竭
- 心律失常/房颤
- 冠心病
- 高血压
- 高甘油三酯血症
- 高胆固醇血症
- 急性冠脉综合征

Score columns must NOT be used for disease matching.

---

## File 3: ICD-10 Reference

ICD-10 is used as the standardized disease classification layer between PharmaCube indication descriptions and internal disease taxonomy.

The project primarily uses:

```text
China ICD-10 Medical Insurance Version (医保版 ICD-10)

---

# Overall Workflow

For every row in the PharmaCube file:

```text
登记号
      ↓
主试验药
      ↓
适应症描述
      ↓
Determine indication structure
      ↓
Normalize Disease Entity
      ↓
Map to ICD-10
      ↓
Match internal Disease(CN)
      ↓
Generate output
```

---

# Disease Identification Rules

## Rule 1: Always prioritize indication description

Priority:

1. 适应症描述
2. 适应症
3. 试验题目
4. 历史备注

Never prioritize:

- 疾病领域

---


## Rule 2: Indication Granularity

In the current dataset, each pharmaceutical indication description represents one clinical indication.

Therefore:

- Codex should NOT split indication descriptions by default.
- Each indication description should be treated as one mapping unit.
- The original indication description should be preserved.

The workflow is:

适应症描述

↓

Disease Entity normalization

↓

ICD-10 mapping

↓

Internal disease classification


Only perform splitting when multiple independent diseases are explicitly present and cannot be represented as one disease entity.

Example requiring split:

"动脉粥样硬化性心血管疾病；肥胖"

This contains two independent disease entities:

1. 动脉粥样硬化性心血管疾病
2. 肥胖


Example NOT requiring split:

"非家族性高胆固醇血症和混合型高脂血症"

This represents different lipid disorder subtypes under the same clinical indication category.

The indication should be normalized into:

Disease Entity:

高脂血症


and mapped as one disease entity.

---
## Rule 3: Disease Subtype Normalization

Some indication descriptions may contain multiple disease subtypes, phenotypes, or clinical variants.

If these subtypes belong to the same disease entity, Codex should NOT split them into separate indications.

Instead:

1. Identify the common disease entity.
2. Normalize the indication into one Disease Entity.
3. Map the unified disease entity to ICD-10.
4. Preserve the original indication description.


Examples:


Input:

非家族性高胆固醇血症和混合型高脂血症


Correct:

Disease Entity:

高胆固醇血症


Mapping:

One ICD-10 mapping


Incorrect:

Split into:

- 非家族性高胆固醇血症
- 混合型高脂血症


because these represent subtypes within lipid metabolism disorders.

The final internal disease mapping should follow the available internal disease taxonomy.

---

Additional examples:


Input:

轻度至中度心力衰竭

Disease Entity:

心力衰竭


Disease severity, molecular subtype, or clinical stage should not create separate disease entities unless they correspond to different ICD-10 categories.

---

## Rule 4: Disease Entity and Disease Modifier

Indication descriptions may contain disease modifiers.

Examples:

- Disease stage
- Severity
- Biomarker status
- Genetic subtype
- Patient population


These modifiers should not create separate disease entities.


Examples:

Input:

重度高甘油三酯血症

Disease Entity:

高甘油三酯血症

Modifier:

重度


Input:

杂合子型家族性高胆固醇血症

Disease Entity:

高胆固醇血症

Modifier:

杂合子型家族性

---

## Rule 5: Disease Entity Mapping Principle

The default assumption is:

One indication description
        ↓
One primary disease entity


However, one indication description may contain:

- Disease subtypes;
- Disease phenotypes;
- Clinical variants.

These should be normalized into one disease entity when they represent the same clinical disease category.

Multiple disease entities should only be generated when clearly independent diseases are present.

---


# ICD-10 Mapping Rules

The ICD-10 mapping should follow:

```text
PharmaCube indication description
          ↓
Standardized disease entity
          ↓
China ICD-10 Medical Insurance classification
          ↓
Internal disease classification


For every indication:

Codex must return:

| Field |
|--------|
| ICD-10 Version |
| ICD-10 Code |
| ICD-10 Disease Name |
| ICD-10 Category |
| ICD-10 Confidence |

ICD-10 Version should always be:

China ICD-10 Medical Insurance Version

Codex must:

- Choose the most specific ICD-10 disease supported by the indication description.
- Avoid over-interpretation.
- Keep uncertainty explicit.

---

## Example

Input:

重度高甘油三酯血症

Output:

ICD-10 Version:

China ICD-10 Medical Insurance Version

ICD-10 Code:

E78.1

ICD-10 Disease:

高甘油三酯血症

ICD-10 Category:

Disorders of lipoprotein metabolism
```

---

# Internal Disease Mapping Rules

Internal matching target:

```text
Disease Mapping
        ↓
Disease (CN)
```

Codex must determine:

1. Can the indication be mapped?
2. Which disease does it map to?
3. Why?

---

## Rule 1: Exact matching preferred

Prefer:

- Exact disease names.
- Accepted medical synonyms.
- ICD-10-supported mappings.

---

## Rule 2: Do not force mapping

If no disease exists:

Return:

```text
Can Match = No

Internal Disease = Unmapped
```

Never force diseases into existing categories.

---

## Rule 3: Multiple disease mappings

One indication may correspond to multiple internal diseases.

Example:

```text
动脉粥样硬化性心血管疾病
```

Possible diseases:

- 冠心病
- 外周动脉疾病
- 缺血性卒中

These are possible clinical associations, not automatic mappings.

Codex should only map to internal diseases if supported by the indication description and internal taxonomy definition.

Codex must:

- List all possible matches.
- Explain the rationale.
- Mark:

```text
Mapping Type:

Multiple Internal Matches
```

---

## Rule 4: Cross-TA indications

Some indications do not belong to cardiovascular diseases.

Example:

```text
肥胖
超重
2型糖尿病
```

Codex must:

- Mark:

```text
Non-cardiovascular indication
```

- Exclude them from cardiovascular mapping.

---

## Rule 5: Partial mapping

One drug may contain both cardiovascular and non-cardiovascular indications.

Example:

```text
动脉粥样硬化性心血管疾病；肥胖；超重
```

Result:

| Indication | Match |
|------------|--------|
| 动脉粥样硬化性心血管疾病 | Yes |
| 肥胖 | No |
| 超重 | No |

Return:

```text
Mapping Status:

Partially Mapped
```

---

# Mapping Status

Use one of:

```text
Fully Mapped
Partially Mapped
Unmapped
Multiple Internal Matches
Manual Review Required
```

Definitions:

### Fully Mapped

All indications match internal diseases.

### Partially Mapped

Some indications match and some do not.

### Unmapped

No internal disease exists.

### Multiple Internal Matches

One indication corresponds to multiple diseases.

### Manual Review Required

Codex cannot confidently determine the mapping.

---

# Manual Review Rules

Mark:

```text
Review Required = Yes
```

if:

- Multiple ICD-10 diseases are possible.
- Multiple internal diseases are possible.
- The indication is ambiguous.
- It is unclear whether the disease belongs to cardiovascular TA.

Codex must never force uncertain mappings.

---

# Required Output

For every indication after splitting:

| Field |
|--------|
| 登记号 |
| 主试验药 |
| 医药魔方适应症 |
| 原始适应症描述 |
| Normalized Disease Entity |
| ICD-10 Version |
| ICD-10 Code |
| ICD-10 Disease Name |
| ICD-10 Category |
| ICD-10 Confidence |
| Can Match Internal Disease |
| Internal Disease (CN) |
| Internal Disease (EN) |
| Mapping Status |
| Review Required |
| Mapping Reason |

---

# Output Examples

## Example 1

Input:

```text
登记号：

DR10624

主试验药：

DR10624

适应症：

高甘油三酯血症

适应症描述：

重度高甘油三酯血症
```

Output:

| Field | Value |
|--------|--------|
| ICD-10 Code | E78.1 |
| ICD-10 Disease Name | Hypertriglyceridemia |
| Can Match | Yes |
| Internal Disease | 高甘油三酯血症 |
| Status | Fully Mapped |

Reason:

```text
The indication explicitly states severe hypertriglyceridemia and exactly matches the internal disease classification.
```

---

## Example 2

Input:

```text
适应症描述：

急性穿支动脉梗死
```

Output:

| Field | Value |
|--------|--------|
| ICD-10 Code | I63 |
| ICD-10 Disease Name Name | Ischemic stroke |
| Can Match | No |
| Internal Disease | Unmapped |
| Status | Unmapped |

Reason:

```text
The indication can be mapped to ischemic stroke in ICD-10, but there is no corresponding disease in the internal disease table.
```

---

## Example 3

Input:

动脉粥样硬化性心血管疾病；肥胖；超重


Because these represent independent disease entities:

Split is required.


Result:

| Disease Entity | Match |
|---|---|
| 动脉粥样硬化性心血管疾病 | Yes |
| 肥胖 | No |
| 超重 | No |

Status:

Partially Mapped
```

---

# Restrictions

Codex must NOT:

- Use score columns.
- Ignore indication descriptions.
- Use disease area as the primary source.
- Force mappings.
- Delete unmapped diseases.
- Modify original files.
- Use ICD-10 versions other than China ICD-10 Medical Insurance Version unless explicitly requested.
- Mix different ICD-10 versions within one mapping task.
- Select ICD-10 codes only based on keyword similarity.
All unmapped records must be preserved.

