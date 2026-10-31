# ReportRight

**Pick the right reporting checklists for a musculoskeletal imaging AI study, before you write it up.**

[![Licence: Apache 2.0](https://img.shields.io/badge/licence-Apache%202.0-2E6E9E)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-D97757)](skills/report-right/SKILL.md)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-D97757)](#use-it-with-an-ai-assistant)
[![Version 2.3](https://img.shields.io/badge/version-2.3-555555)](skills/report-right/SKILL.md)

Reporting guidelines, quality scores and risk-of-bias tools for AI studies have multiplied, and it is
often unclear which ones apply. ReportRight is an **Agent Skill** that any LLM-based assistant can
load to classify a manuscript and to return the recommended checklists and appraisal tools. The skill asks clarifying questions when key facts are missing, runs specific checks (such as assessing whether ordinal grades were reduced to binary outcomes and image- versus patient-level partitioning) and warns against common pitfalls. The skill returns the checklists, MSK-specific reminders and a machine-readable summary.

ReportRight recommends checklists. It does not complete them or check a manuscript against them.

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
| ● | Main checklist | The one reporting guideline written for the study design. CLAIM 2024 where no more specific guideline exists. |
| ○ | Additional items | Applicable items from other checklists, only where the main checklist is silent. Never a second full checklist. |
| ◆ | Appraisal tools | Quality or risk-of-bias tools for self-checking before submission and for peer review. |

It also flags up to nine MSK-specific checks, such as a reference standard that does not match the
clinical target, ordinal grades collapsed to binary outcomes, or image-level instead of
patient-level data partitioning. Each answer ends with a JSON block, for example for a radiomics model
that predicts local recurrence of soft-tissue sarcoma:

```json
{"study_type": "T3", "secondary_type": null,
 "main_checklist": "CLEAR",
 "additional_items": ["IBSI", "TRIPOD+AI"], "appraisal_tools": ["METRICS", "RQS 2.0"],
 "msk_checks": [1, 3, 4], "path": ["Q1:no", "Q2:no", "Q3:no", "Q4:no", "Q5:no", "Q6:yes"],
 "confidence": "high", "rationale": "..."}
```

## Which checklists for which study

| Type | Study type | ● Main checklist | ○ Additional items (when) | ◆ Appraisal tools |
|---|---|---|---|---|
| T1 | Imaging AI model development: detection, classification or grading | CLAIM 2024 | STARD-AI (accuracy against a reference standard) | QUADAS-3, PROBAST+AI |
| T2 | Segmentation or automated measurement | CLAIM 2024 | RIDGE; Metrics Reloaded (metric choice) | – |
| T3 | Radiomics (hand-crafted or deep features) | CLEAR | IBSI; TRIPOD+AI (future outcome predicted); CLAIM 2024 (deep-learning features or models) | METRICS, RQS 2.0 |
| T4 | Clinical prediction model (diagnostic or prognostic risk) | TRIPOD+AI | CLAIM 2024 (image data and imaging model); CLEAR (radiomic predictors) | PROBAST+AI |
| T5 | Evaluation of an existing AI tool or AI-assisted reading | STARD-AI; CLAIM 2024 if the endpoint is measurement or segmentation agreement or image quality, not diagnostic accuracy against a reference standard | CLAIM 2024 (image data, when STARD-AI is main); GRRAS (agreement, when CLAIM is main) | QUADAS-3 |
| T6 | Image reconstruction, synthesis or enhancement | CLAIM 2024 | GRRAS (agreement and interchangeability); STARD-AI (diagnostic accuracy assessed) | – |
| T7 | LLM or multimodal generative AI in radiology | FLAIR; TRIPOD-LLM if the task involves neither images nor radiology reports | MI-CLEAR-LLM (accuracy evaluation) | AQAT:RoB |
| T8 | Patient-facing chatbot or health advice | CHART | FLAIR (radiology content); MI-CLEAR-LLM (accuracy evaluation) | AQAT:RoB |
| T9 | Early-stage live clinical evaluation | DECIDE-AI | FUTURE-AI | – |
| T10 | Randomised trial of an AI intervention (or its protocol) | SPIRIT-AI / CONSORT-AI | FUTURE-AI | – |
| T11 | Systematic review or meta-analysis of AI studies | PRISMA 2020 / PRISMA-DTA | – | QUADAS-3, PROBAST+AI, METRICS, RQS 2.0 |
| T12 | Imaging dataset publication | TRAM-AI | STANDING Together; CLAIM 2024 (image data and annotation) | – |

Special cases: add REFINE or TRIPOD-LLM items to FLAIR when an LLM or foundation model is fine-tuned
or developed. Use SPIRIT-AI for a trial protocol and PRISMA-DTA for a review that pools diagnostic
accuracy. Consult FUTURE-AI before deployment, and always check what the target journal requires;
some journals ask for CLAIM for every AI study.

## Limitations

- The rules were refined on the same 50 papers and the reference labels were assigned by the authors,
  so the agreement and correctness estimates are probably optimistic. Treat the output as a starting
  point for your own judgement.
- All tested models came from one vendor (Anthropic). Other models may apply the rules differently.
- The instrument list reflects the literature up to October 2026. Check for newer versions.

## Citing this

If you use ReportRight, please cite the accompanying review (manuscript in preparation; full
reference to follow).

## Licence

Apache-2.0. See [`LICENSE`](LICENSE). The reporting guidelines and appraisal tools named here belong
to their authors; ReportRight only helps choose between them.
