# ReportRight

**Pick the right reporting checklists for a musculoskeletal imaging AI study, before you write it up.**

[![Licence: Apache 2.0](https://img.shields.io/badge/licence-Apache%202.0-2E6E9E)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-D97757)](skills/report-right/SKILL.md)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-D97757)](#use-it-with-an-ai-assistant)
[![Version 2.2](https://img.shields.io/badge/version-2.2-555555)](skills/report-right/SKILL.md)

Reporting guidelines, quality scores and risk-of-bias tools for AI studies have multiplied, and it is
often unclear which ones apply. ReportRight is an **Agent Skill** that any LLM-based assistant can
load. It uses **CLAIM 2024 as the base checklist** for any study that applies AI to medical images and
adds **one study-specific checklist** chosen by the study design, such as STARD-AI for diagnostic
accuracy, TRIPOD+AI for prediction models, CLEAR for radiomics or FLAIR for LLM studies. An
**appraisal tool** is added for self-checking. Nine yes/no questions assign one of 12 musculoskeletal
(MSK) study types, and the skill returns the checklists, MSK-specific reminders and a
machine-readable summary.

On 50 MSK imaging AI papers published in 2024–2026, three Claude models of different size, with
repeated runs, agreed on the study type for 46 of 50 papers (Fleiss κ = 0.95) and on the recommended
checklists for 48 of 50 (κ = 0.98).

## Use it with an AI assistant

**Claude Code** (plugin marketplace):

```
/plugin marketplace add ATI-lab-UoC/ReportRight
/plugin install report-right@report-right
```

The same from a terminal:

```bash
claude plugin marketplace add ATI-lab-UoC/ReportRight
claude plugin install report-right@report-right
```

Then ask in plain language, or call the skill directly:

```
Which reporting checklists should I use for the study in draft/abstract.md?
/report-right:report-right draft/manuscript.docx
```

**Other options:**

| Environment | Method |
|---|---|
| Any Claude Code project, without the marketplace | `cp -r skills/report-right ~/.claude/skills/` |
| claude.ai or the Claude API | Zip the skill folder (`cd skills && zip -r ../report-right.zip report-right`) and upload it as a skill in Settings > Capabilities |
| Any other agent (Codex, Cursor, Gemini, local models) | Point it at [`skills/report-right/SKILL.md`](skills/report-right/SKILL.md), which is plain Markdown |

The skill needs no code, packages or internet access. It works from a title and abstract and is more
confident with the full manuscript.

## What it returns

| Symbol | Element | What it is |
|---|---|---|
| ■ | Base checklist | CLAIM 2024, for any study that applies AI to medical images. Development items that do not apply are marked not applicable. |
| ● | Study-specific checklist | The guideline written for the study design, reported together with CLAIM. |
| ○ | Additional items | Applicable items from other checklists that the first two lack. Never a second full checklist. |
| ◆ | Appraisal tools | Quality or risk-of-bias tools for self-checking before submission and for peer review. |

It also flags up to nine MSK-specific checks, such as a reference standard that does not match the
clinical target, ordinal grades collapsed to binary outcomes, or image-level instead of
patient-level data partitioning. Each answer ends with a JSON block, for example for a radiomics model
that predicts local recurrence of soft-tissue sarcoma:

```json
{"study_type": "T3", "secondary_type": null,
 "base_checklist": ["CLAIM 2024"], "study_specific_checklist": ["CLEAR"],
 "additional_items": ["IBSI", "TRIPOD+AI"], "appraisal_tools": ["METRICS", "RQS 2.0"],
 "msk_checks": [1, 3, 4], "path": ["Q1:no", "Q2:no", "Q3:no", "Q4:no", "Q5:no", "Q6:yes"],
 "confidence": "high", "rationale": "..."}
```

## Which checklists for which study

| Type | Study type | ■ Base checklist | ● Study-specific checklist | ○ Additional items | ◆ Appraisal tools |
|---|---|---|---|---|---|
| T1 | Imaging AI model development: detection, classification or grading | CLAIM 2024 | – | STARD-AI, TRIPOD+AI | QUADAS-3, PROBAST+AI |
| T2 | Segmentation or automated measurement | CLAIM 2024 | – | RIDGE, Metrics Reloaded | – |
| T3 | Radiomics (hand-crafted or deep features) | CLAIM 2024 | CLEAR | IBSI, TRIPOD+AI, GRRAS (feature reliability) | METRICS, RQS 2.0 |
| T4 | Clinical prediction model (diagnostic or prognostic risk) | CLAIM 2024 | TRIPOD+AI | CLEAR | PROBAST+AI |
| T5 | Accuracy of an existing AI tool or AI-assisted reading | CLAIM 2024 | STARD-AI | – | QUADAS-3 |
| T6 | Image reconstruction, synthesis or enhancement | CLAIM 2024 | – | STARD-AI, GRRAS (agreement, interchangeability) | – |
| T7 | LLM or multimodal generative AI in radiology | CLAIM 2024 | FLAIR | MI-CLEAR-LLM, TRIPOD-LLM, REFINE | AQAT:RoB |
| T8 | Patient-facing chatbot or health advice | – | CHART | FLAIR, MI-CLEAR-LLM | AQAT:RoB |
| T9 | Early-stage live clinical evaluation | CLAIM 2024 | DECIDE-AI | FUTURE-AI | – |
| T10 | Randomised trial of an AI intervention (or its protocol) | CLAIM 2024 | SPIRIT-/CONSORT-AI | FUTURE-AI | – |
| T11 | Systematic review or meta-analysis of AI studies | – | PRISMA 2020 / PRISMA-DTA | – | QUADAS-3, PROBAST+AI, METRICS, RQS 2.0, AQAT:RoB |
| T12 | Imaging dataset publication | – | TRAM-AI | CLAIM 2024, STANDING Together | – |

Special cases: use REFINE or TRIPOD-LLM as the study-specific checklist when an LLM or foundation
model is fine-tuned or developed. Add TRIPOD+AI items when a radiomics model predicts a future
outcome, CLEAR items when radiomic features are predictors in a clinical model, and STARD-AI items
when an in-house model's accuracy is reported against a reference standard. Consult FUTURE-AI before
deployment, and always check what the target journal requires.

## Limitations

- Agreement between models shows that the rules are applied consistently. It does not show that the
  recommendations are correct, so treat the output as a starting point for your own judgement.
- The instrument list reflects the literature up to October 2026. Check for newer versions.

## Citing this

If you use ReportRight, please cite the accompanying review (manuscript in preparation; full
reference to follow).

## Licence

Apache-2.0. See [`LICENSE`](LICENSE). The reporting guidelines and appraisal tools named here belong
to their authors; ReportRight only helps choose between them.
