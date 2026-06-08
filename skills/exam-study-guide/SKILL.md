---
name: exam-study-guide
description: Create polished exam study-guide PDFs from lecture PDFs, slides, notes, or integrated course material. Use when the user wants course materials summarized, reorganized by exam scope, tailored to a specific exam format, and exported as a visually clean PDF with Korean explanations, key English terms, emphasis, final cram sheets, or likely exam points.
---

# Exam Study Guide PDF

## Purpose

Use this skill to turn lecture materials into a study-guide PDF for an exam. The output should help the user study efficiently, not merely restate the source.

The guide must adapt to the user’s current exam:

- Covered files or weeks
- Expected question types
- Expected number of questions
- User’s preferred depth, tone, and organization
- Language requirements
- Visual emphasis requirements
- Any professor-specific hints

## Required Inputs To Infer Or Ask

Proceed with reasonable assumptions when the answer is visible from local files or the user’s message. Ask only if a missing detail would materially change the result.

Capture these variables:

| Variable | Examples | How to use |
|---|---|---|
| Exam scope | `1-4주차, 6주차.pdf`, midterm range, selected chapters | Decide which source files to read and cite internally. |
| Source priority | integrated PDF, weekly PDFs, handwritten notes, textbook | Prefer the user’s stated primary source; use duplicates only for cross-checking order or missing text. |
| Question type | multiple choice, short answer, essay, practical ID, case-based | Change emphasis and final review format. |
| Expected count | 35 MCQ + 5 short answer, few questions, many detail questions | Decide compression level and likely yield of details. |
| Language | Korean explanation, English terms in parentheses | Match the requested output language; keep important source terms. |
| Desired style | concise, detailed, concept-heavy, memorization-heavy, visually clean | Shape section depth and density. |
| Special professor hints | “문제 수가 많지 않음”, “그림에서 낸다”, “객관식 위주” | Emphasize high-yield comparisons, definitions, numbers, figures, and exceptions. |

## Output Structure

Default structure:

1. **시험 범위 파트 정리 및 짧은 요약**
   - Scope map by week, file, chapter, or lecture part.
   - One-line “what this part is really about.”
   - High-yield axis list, such as definitions, comparisons, mechanisms, numbers, exceptions.

2. **본문: 전체 파트별 핵심과 맥락 설명**
   - Reorganize by conceptual parts, not just slide order.
   - For each part, include:
     - 대주제
     - 소주제
     - 핵심 용어 in Korean + English
     - Context explanation
     - Likely exam points
     - Important numbers, lists, causes, consequences, diagnoses, treatments, or comparisons

3. **마지막 엑기스**
   - Exam-before-review page(s).
   - Include high-yield bullet list.
   - Include short-answer candidates when relevant.
   - Include comparison tables for confusing items.

Modify this structure when the exam type requires it:

| Exam type | Add or emphasize |
|---|---|
| 객관식 | Comparison tables, true/false traps, definitions, exception lists, numeric values, “which is correct/incorrect” candidates. |
| 단답형 | Term-definition pairs, named lists, cause-effect chains, “write 3 examples” prompts, exact English terms. |
| 서술형 | Mechanism paragraphs, flow diagrams, cause-pathogenesis-clinical sign-diagnosis-treatment structure. |
| 케이스형 | Clinical reasoning, differential diagnosis, decision points, common pitfalls. |
| 그림/사진 기반 | Visual ID checklist, labeled anatomy, what to inspect first, likely labels. |
| 실습형 | Stepwise procedure, equipment, sample handling, safety, common errors. |

## Study-Guide Writing Rules

### General

- Prefer Korean explanations unless the user requests otherwise.
- Put important English terms next to Korean terms in parentheses or a small term box.
- Explain the context: why the concept matters, what it connects to, and how it can become a question.
- Do not summarize every slide equally. Weight likely exam material higher.
- Treat professor emphasis, repeated slides, objectives, take-home messages, and numbers as high yield.
- Distinguish source facts from inference when guessing likely exam points.

### High-Yield Detection

Prioritize:

- Learning objectives and contents pages
- Take-home messages
- Definitions and named frameworks
- Classifications and taxonomies
- Numeric thresholds or ranges
- Comparisons
- Exceptions and “do not” warnings
- Mechanisms and causal chains
- Disease name - pathogen/cause - signs - diagnosis - treatment
- Handling, safety, PPE, sample handling, and practical procedures
- Repeated terms across files

### Visual Emphasis

Use emphasis consistently:

- **Bold** for must-know terms or conclusions.
- Colored text only when contrast is high and the background is light enough.
- Use colored borders, pale colored backgrounds, icons/markers, or labels for exam points.
- Use tables for comparisons, disease summaries, classification, diagnostic workflows, and short-answer candidates.
- Use section bars or large headings to separate major parts.

Important accessibility rule:

- Do **not** use white text unless it is inside a clearly colored/dark text box or section bar.
- Never use white text on a white, pale, transparent, or unknown background.
- For normal paragraphs, table body cells, and pale highlight boxes, use dark text.
- When creating PDF styles, define separate styles for dark section headers and light content boxes so white text cannot leak into normal content.

## PDF Production Workflow

Use the `pdf` skill when creating or reviewing PDFs.

1. **Inventory sources**
   - List files in the working folder.
   - Identify the exam-scope files and any integrated PDF.
   - Count pages.

2. **Extract content**
   - Use `pdfplumber` or `pypdf` for text extraction.
   - If text extraction is garbled, try alternate extraction methods or use rendered pages for visual inspection.
   - Store temporary extraction output under `tmp/pdfs/`.

3. **Build the study outline**
   - First create a scope map.
   - Then reorganize into conceptual parts.
   - Create final cram sections before writing the PDF.

4. **Generate the PDF**
   - Prefer `reportlab` for deterministic layout.
   - Use a Korean-capable font, such as Malgun Gothic on Windows.
   - Write final files under `output/pdf/`.
   - Keep filenames descriptive and stable.

5. **Render and verify**
   - Render representative pages to PNG using Poppler or PyMuPDF.
   - Check:
     - Korean text renders correctly.
     - No white text is placed on a white/light background.
     - No overlapping text.
     - Tables fit within page margins.
     - Headers, footers, and page numbers are clean.
     - No blank pages.
   - If defects appear, fix styles or layout and regenerate.

## Recommended PDF Style System

Use this style logic rather than copying exact colors blindly:

| Element | Background | Text |
|---|---|---|
| Cover title | white | dark navy or black |
| Major section bar | dark/medium color | white allowed |
| Normal body | white | dark gray/black |
| Term box | pale blue/gray | dark navy |
| Exam point box | pale red/yellow with colored border | dark red/black |
| Concept box | pale teal/green with colored border | dark teal/black |
| Table header | dark/medium color | white allowed |
| Table body | white or very pale stripe | dark gray/black |

Before finalizing, scan rendered pages for any invisible or low-contrast text.

## Content Templates

### Scope Summary

```markdown
# 1. 시험 범위 파트 정리 및 짧은 요약

| 범위 | 주요 내용 | 짧은 요약 |
|---|---|---|
| 1주차 | ... | ... |
| 2주차 | ... | ... |

> 시험포인트
> - ...
> - ...
```

### Part Body

```markdown
# Part A. 대주제

## A-1. 소주제

용어: 한글명(English term) / ...

- 핵심 설명
- 맥락 설명
- 왜 시험에 나올 수 있는지

> 시험포인트
> - 정의형 후보
> - 객관식 함정
> - 단답형 암기 리스트
```

### Disease Or Concept Table

```markdown
| 항목 | 원인/정의 | 특징 | 진단 | 치료/관리 | 출제 포인트 |
|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... |
```

### Final Cram

```markdown
# 3. 시험 직전 엑기스

## 객관식 고빈도 포인트

1. ...
2. ...

## 단답형 후보

| 문제 | 핵심 답안 |
|---|---|
| ... | ... |
```

## Quality Checklist

Before final response:

- Final PDF exists under `output/pdf/`.
- Source scope matches the user’s requested files/weeks.
- The guide includes scope summary, body, and final cram unless the user requested another structure.
- Exam type and expected question count influenced the content density.
- Important English terms are included.
- High-yield material is visually emphasized.
- No white text appears outside dark/colored section bars or dark table headers.
- Rendered pages confirm text is legible and not overlapping.
- Page count and output path are reported to the user.
