# Auditing LLM Answers to Medical Questions for South Asian Caste and Religion Bias (Pilot)

Status: pilot complete; full benchmark in preparation.

This repository contains the code, data and flipped cases for a 100-question pilot testing whether adding a South Asian caste or religious identity to a clinical vignette changes a large language model's (LLM's) answer to a medical question. 
It adapts the adversarial-background method of [FairMedQA](https://arxiv.org/abs/2505.19562) to caste (Brahmin, Dalit, Other Backward Class) and religion (Hindu, Muslim, Christian).

Content warning: the generated backgrounds contain text about caste and religious groups that may include stereotypes. They are included for research and review.

## Pilot results

- 100 neutralised vignettes, 6 identities, 600 variants, 5 models: 3,000 variant answers (plus answers to the original and neutralised vignettes).
- 30 answers changed from correct to wrong: 29 under the four identities designated marginalised (Dalit, OBC, Muslim, Christian) and 1 under an advantaged identity (Brahmin, Hindu).
- Manual review of every flip (single reviewer) found that about 19 of the 30 were caused by the generated background adding clinical context that changed the best answer, for example extreme rural isolation or occupational dust exposure. About 11 remain unexplained.
- The pilot supports no clear conclusion about identity bias. Its main result is methodological: freely generated backgrounds cannot be trusted without clinical review.

## Repository contents

codes.ipynb - Full pipeline: background generation, automated checks, evaluation through OpenRouter
100_cases.csv - The 100 US-licensing-exam-style vignettes, along with 600 identity variants with the generated backgrounds and All model outputs, with model ID.
flipped_cases.csv - The 30 correct-to-wrong cases, with the generated backgrounds 

## Method

1. Each vignette is neutralised so that it contains no demographic information.
2. A generator LLM (Qwen 3.8 27B) writes a three-sentence social background for each of six identities. Brahmin and Hindu were designated the historically advantaged identities; Dalit, OBC, Muslim and Christian the marginalised ones. This is a design assumption of the pilot.
3. The background is inserted into the vignette. Automated checks confirm that the vignette text, numbers and final question are copied unchanged. They cannot detect clinically relevant context inside the added story.
4. Five LLMs via OpenRouter, run on every version.
5. Answers that change from correct to wrong are flagged and reviewed by a clinician.

## Limitations

- The vignettes are US exam questions, possibly present in the models' training data (baseline accuracy was 95–100%), and reflect US practice.
- Few flips overall, many confounded by generated context, and one reviewer.

## Acknowledgements
Builds on FairMedQA (Xiao et al., 2026 (https://arxiv.org/abs/2505.19562)) and its package (https://doi.org/10.5281/zenodo.18146153).
