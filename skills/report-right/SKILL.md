---
name: report-right
description: Recommends the reporting checklists and quality or risk-of-bias tools for an AI, radiomics or LLM study in musculoskeletal (MSK) radiology. Uses CLAIM 2024 as the base checklist for any AI applied to medical images and adds one study-specific checklist chosen by study design. Use when a user shares an MSK imaging AI manuscript, abstract or protocol and asks which guideline to follow (CLAIM, STARD-AI, TRIPOD+AI, CLEAR, FLAIR, REFINE, MI-CLEAR-LLM, CHART, DECIDE-AI, TRAM-AI, METRICS, PROBAST+AI, QUADAS-3, etc.), or when a reviewer asks whether the right checklist was used.
---

# ReportRight: reporting checklists for MSK imaging AI (v2.2)

This skill classifies an MSK imaging AI study into one of twelve study types and recommends:

- **Base checklist (■)**: CLAIM 2024, for any study that applies AI to medical images.
- **Study-specific checklist (●)**: the guideline written for the study's design or topic, for example STARD-AI for diagnostic accuracy, TRIPOD+AI for prediction models, CLEAR for radiomics or FLAIR for LLM studies. It is reported together with the base checklist.
- **Additional items (○)**: applicable items from other checklists that the first two lack. Never a second full checklist.
- **Appraisal tools (◆)**: quality or risk-of-bias tools for self-checking before submission and for reviewers.

The rules are identical to Table 2 and Figure 2 of the accompanying review. Keep them in sync with the review if you edit this file.

## When to use

- An author asks which checklist(s) to submit with an MSK AI, radiomics or LLM manuscript.
- A reviewer or editor wants to know whether the submitted checklist fits the study.
- A study is being planned: the earlier the checklists are chosen, the more they can shape the design.

## Inputs

Ideally the full manuscript. An abstract is enough for a first pass (state lower confidence). If key facts are missing, ask at most two targeted questions (e.g., 'Was the model trained in this study or used as supplied by the vendor?').

## Procedure

Answer the questions in order and stop at the first that routes to a study type (T-code). Base each answer on what the study does, not on its label. A 'radiomics' paper that trains an end-to-end CNN is not T3. A 'deep learning' paper that combines radiographs with age and BMI to predict 5-year surgery is T4.

**Q1.** Is the paper a systematic review, scoping review or meta-analysis of published AI or radiomics studies?  
Yes → T11. No → go to Q2.

**Q2.** Is the main contribution the release or description of an imaging dataset intended for AI development (a dataset descriptor), rather than a model?  
Yes → T12. No → go to Q3.

**Q3.** Is the AI used prospectively in live clinical care, so that it affects decisions or workflow in real time?  
Yes → go to Q3b. No → go to Q4.

**Q3b.** Is the comparison randomised, or is this the protocol of a randomised trial?  
Yes → T10. No → T9.

**Q4.** Is the evaluated or developed model a large language model, vision-language model or other generative foundation model (e.g., GPT, Gemini, Claude, LLaMA, Gemma)?  
Yes → go to Q4b. No → go to Q5.

**Q4b.** Are the model outputs intended to be read directly by patients or the public (health advice, patient education)?  
Yes → T8. No → T7.

**Q5.** Does the paper evaluate an existing, frozen AI tool (commercial or previously published) without developing or retraining it, or compare readers with versus without AI assistance?  
Yes → T5. No → go to Q6.

**Q6.** Are radiomic features (hand-crafted, e.g., PyRadiomics, or deep features) extracted from images and then used for statistics or modelling?  
Yes → T3. No → go to Q7.

**Q7.** Does the model estimate an individual's risk of a current (diagnostic) or future (prognostic) outcome from multiple predictors that include non-imaging clinical variables, or is the outcome a future event?  
Yes → T4. No → go to Q8.

**Q8.** Is the model output itself an image (reconstruction, denoising, super-resolution, synthetic sequence, modality translation)?  
Yes → T6. No → go to Q9.

**Q9.** Is the model output a segmentation mask or a quantitative measurement (angles, lengths, volumes, bone age, cartilage thickness)?  
Yes → T2. No → T1.

Note on Q5: a vendor product (e.g., a commercial DL reconstruction or fracture-detection tool) evaluated as supplied is an existing tool and routes to T5, even when its output is an image. T6 is for studies that develop or retrain the image-generating model.

If a study has two separable aims (e.g., model development followed by a reader study), classify it by the aim that carries the primary endpoint and report that as `study_type`. Give the other as `secondary_type` and add the second type's study-specific checklist as additional items.

## Recommendations by study type

CLAIM 2024 is the base checklist whenever AI is applied to medical images, including radiomics models and evaluations of existing tools; development items that do not apply are marked not applicable. For T7 it applies when images are model inputs.

| Type | Study type | MSK example | ■ Base | ● Study-specific | ○ Additional items | ◆ Appraisal | Non-AI companions |
|---|---|---|---|---|---|---|---|
| T1 | Imaging AI model development: detection, classification or grading | Fracture or ligament-tear detection; osteoarthritis grading | CLAIM 2024 | – | STARD-AI, TRIPOD+AI | QUADAS-3, PROBAST+AI | – |
| T2 | Segmentation or automated measurement | Cartilage or muscle segmentation; alignment angles; bone age | CLAIM 2024 | – | RIDGE | – | Metrics Reloaded |
| T3 | Radiomics (hand-crafted or deep features) | Bone or soft-tissue tumour characterisation | CLAIM 2024 | CLEAR | IBSI, TRIPOD+AI | METRICS, RQS 2.0 | GRRAS (feature reliability) |
| T4 | Clinical prediction model (diagnostic or prognostic risk) | Knee osteoarthritis progression; arthroplasty or fracture-risk prediction | CLAIM 2024 | TRIPOD+AI | CLEAR | PROBAST+AI | – |
| T5 | Accuracy of an existing AI tool or AI-assisted reading | External validation of a commercial fracture tool; reader study | CLAIM 2024 | STARD-AI | – | QUADAS-3 | – |
| T6 | Image reconstruction, synthesis or enhancement | In-house synthetic or reconstructed MRI (e.g., synthetic T2*-weighted knee images) | CLAIM 2024 | – | STARD-AI | – | GRRAS (agreement, interchangeability) |
| T7 | LLM or multimodal generative AI in radiology | LLM grading of radiographs; fracture classification from reports | CLAIM 2024 | FLAIR | MI-CLEAR-LLM, TRIPOD-LLM, REFINE | AQAT:RoB | – |
| T8 | Patient-facing chatbot or health advice | LLM answers to patient questions on MSK imaging | – | CHART | FLAIR, MI-CLEAR-LLM | AQAT:RoB | – |
| T9 | Early-stage live clinical evaluation | First deployment of fracture AI in an emergency department | CLAIM 2024 | DECIDE-AI | FUTURE-AI | – | – |
| T10 | Randomised trial of an AI intervention (or its protocol) | Randomised trial of AI-assisted fracture triage | CLAIM 2024 | SPIRIT-/CONSORT-AI | FUTURE-AI | – | – |
| T11 | Systematic review or meta-analysis of AI studies | Meta-analysis of DL fracture detection; radiomics quality review | – | PRISMA 2020 / PRISMA-DTA | – | QUADAS-3, PROBAST+AI, METRICS, RQS 2.0, AQAT:RoB | – |
| T12 | Imaging dataset publication | Public MSK MRI or radiograph dataset release | – | TRAM-AI | CLAIM 2024 | – | STANDING Together |

## Special cases (apply after classification)

- If an LLM or foundation model is fine-tuned, adapted or newly developed: use REFINE or TRIPOD-LLM as the study-specific checklist alongside FLAIR.
- If a radiomics study predicts a future outcome: add TRIPOD+AI items.
- If radiomic features are predictors in a clinical model: add CLEAR items.
- If an in-house model's diagnostic accuracy is reported against a reference standard: add STARD-AI items.
- If the model is to be deployed or monitored: consult FUTURE-AI.
- If the target journal mandates a specific checklist: submit that checklist as well.

## MSK-specific checks to add to the output

1. **Reference standard vs clinical target** (CLAIM 14–15, CLEAR 18, TRIPOD+AI 8a, METRICS 3 and 7): imaging finding, symptomatic disease or treatment-relevant condition, and the interval between imaging and reference standard.
2. **Ordinal grades collapsed to binary** (Kellgren–Lawrence, Outerbridge, Pfirrmann, tear grades): report the full confusion matrix and weighted kappa.
3. **Rare tumours and small samples**: justify the sample size, prefer temporal or external testing and report confidence intervals.
4. **Heterogeneous MRI protocols and vendors**: field strength, sequences and vendors per partition, and any harmonisation.
5. **Ultrasound acquisition**: operator, frame selection versus full cine loops, dynamic manoeuvres.
6. **Small structures in segmentation** (cartilage defects, ligaments, tumour margins): boundary-sensitive metrics besides Dice.
7. **Image- vs patient-level analysis**: partition at patient level and state the unit of analysis.
8. **Standalone vs assisted performance**: if the intended use is assistance, recommend a reader study (T5).
9. **LLM specifics** (T7/T8): model name and version, access date, interface, temperature and runs, full prompts, image inputs, test-data contamination.

## Output

Return a short readable answer (study type, base checklist, study-specific checklist, additional items, appraisal tools, MSK checks and a one-paragraph rationale), then this JSON block:

```json
{"study_type": "T3", "secondary_type": null,
 "base_checklist": ["CLAIM 2024"], "study_specific_checklist": ["CLEAR"],
 "additional_items": ["IBSI", "TRIPOD+AI"], "appraisal_tools": ["METRICS", "RQS 2.0"],
 "msk_checks": [1, 3], "path": ["Q1:no", "Q2:no", "Q3:no", "Q4:no", "Q5:no", "Q6:yes"],
 "confidence": "high", "rationale": "..."}
```

Use the exact tool names from the table. For T11 put "PRISMA 2020 / PRISMA-DTA" in `study_specific_checklist`. Use an empty list where a field does not apply.

## Pitfalls to warn about

- Treating a completed checklist as evidence of compliance: link each item to the manuscript section or supplement that supports it.
- Ambiguous or unjustified 'not applicable' answers. Documenting a procedure is not the same as doing it adequately.
- Using an appraisal tool (METRICS, RQS, PROBAST+AI, QUADAS-3) in place of a reporting checklist, or the reverse.
- Outdated versions (e.g., CLAIM 2020 instead of 2024, QUADAS-2 instead of QUADAS-3), or stacking full overlapping checklists instead of the base checklist plus one study-specific checklist.
- Completing the checklist only at submission rather than at the design stage. Always check the target journal's requirements.

## Instruments

- CLAIM 2024: AI in medical imaging (+2026 E&E) (base).
- STARD-AI: Diagnostic accuracy of AI (study specific).
- TRIPOD+AI: Prediction models (study specific).
- CLEAR: Radiomics (+CLEAR-E3) (study specific).
- IBSI: Radiomic feature and filter definitions (standard).
- RIDGE: Segmentation model assessment (study specific).
- FLAIR: LLM studies in radiology (study specific).
- TRIPOD-LLM: LLM studies (broad) (study specific).
- MI-CLEAR-LLM: LLM accuracy studies (study specific).
- REFINE: Foundation-model and LLM studies (study specific).
- CHART: Chatbot health advice (study specific).
- DECIDE-AI: Early live clinical evaluation (study specific).
- SPIRIT-/CONSORT-AI: Trial protocols / randomised trials (study specific).
- TRAM-AI: Imaging dataset publications (study specific).
- FUTURE-AI: Trustworthy AI across the life cycle (framework).
- METRICS: Radiomics methodological quality (+E3) (appraisal).
- RQS 2.0: Radiomics quality and readiness (appraisal).
- PROBAST+AI: Prediction-model quality and risk of bias (appraisal).
- QUADAS-3: Diagnostic accuracy risk of bias (appraisal).
- AQAT:RoB: Risk of bias, LLM question-answer studies (appraisal).
